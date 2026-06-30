# Data model (v1 MVP)

Entities, relationships, state machines for the **merchant-acquiring** MVP. This mirrors the **built domain model** (`../drc-pay/services/api/src/drc_pay_api/`): pure `domains/` (ledger, merchants, transactions) under a shared `application/` service. Schema is language-agnostic; SQL-shaped because the chosen persistent store is Postgres.

- **Last updated:** 2026-06-11
- **Status:** matches the built domain model; auth + webhook-audit entities are planned

> **Direction (2026-06-11): merchant-facing.** Rewritten for the merchant pivot. The core is **Merchant** (the payee), **Transaction** (the workflow record for one customer→merchant transfer), and a double-entry **LedgerEntry** (the source of truth for money). **The MDR is deducted from the merchant's settlement** — the merchant nets `amount − fee`; the customer is **never** charged `amount + fee`. The old payer-centric schema (`user.pin_hash` as the payer, recipient cache, `sender/recipient_phone`, `total_debited = amount + fee`) is gone.

> **Subject to revision when team grows.** New engineers may have preferences on naming conventions (snake_case vs camelCase at the API layer), ORM patterns, ledger partitioning, and model shapes. The entity set and the transaction state machine are the durable parts (and are already built in Python); column-level details will evolve.

## Entity overview

```
Merchant ──┐ is the payee of every Transaction
           │
           └─→ Transaction (1:N)   — one customer→merchant transfer
                     │
                     ├─→ LedgerEntry (1:N, double-entry) — the source of truth for money
                     └─→ OperatorEvent (1:N) — webhook audit from pawaPay  [planned]

MerchantAuth (1:1 with Merchant) — phone + OTP + PIN  [planned, not built]
OtpAttempt — short-lived; tied to a phone, not yet to a merchant  [planned]
OperatorStatus — singleton per operator (Vodacom/Airtel/Orange)  [planned]
```

The **customer is not a stored entity** — they are identified only by the `customer_msisdn` captured on a transaction (no customer account in v1; consumers need no app). The **merchant is the account holder.**

---

## Built entities (the domain core)

These three match the code today (`domains/merchants/models.py`, `domains/transactions/models.py`, `domains/ledger/`). They are the durable heart of the model.

### `merchant`
A registered business that accepts payments — the **payee** side of every transaction. Lightweight MVP record: identity, where to settle, and the till a customer references.

| Column | Type | Notes |
|---|---|---|
| `id` | `text` (uuid-ish) | Primary key. |
| `name` | `text` | The business name; shown to the customer at the USSD confirm ("Pay … to **Alpha Gas Station**"). |
| `short_code` | `text` | The **till / merchant code** a customer enters (e.g. via USSD). Drives the merchant's QR/dial string. Unique. |
| `settlement_msisdn` | `text` | Mobile-money number that receives settlement payouts. Server-derived/confirmed; never client-set at payment time. |
| `settlement_provider` | `text` (nullable) | pawaPay operator code for the settlement wallet; resolved via predict-provider if omitted. |
| `status` | `text` | `active` \| `suspended`. Only `active` merchants can be paid. |

> _Built today:_ the `Merchant` dataclass + read/QR endpoints. _Planned:_ merchant **create/update** writes, plus persisted `created_at`, business-KYC fields, and the auth link below.

### `transaction`
The **workflow tracker** for one customer→merchant transfer. **Not the source of truth for money** — the ledger is; this row tracks where a transfer is in its lifecycle.

| Column | Type | Notes |
|---|---|---|
| `id` | `text` (uuid hex) | PK. Returned to clients; the transaction reference. |
| `customer_msisdn` | `text` | The customer paying the merchant. |
| `merchant_id` | `text` (nullable) | FK → `merchant.id` — the registered merchant this payment is for. |
| `merchant_msisdn` | `text` | The merchant's settlement number (snapshot from the merchant at start). |
| `amount` | `Money` (minor units + currency) | The **sticker price the customer pays** — exactly. |
| `fee` | `Money` | Our charge (**MDR**), same currency. The **merchant nets `amount − fee`**; the customer is never charged this on top. |
| `state` | `TxState` enum | State machine — see below. |
| `history` | `TxState[]` | Every state, in order (append-only audit of the lifecycle). |
| `idempotency_key` | `text` (nullable) | Client-supplied dedup key; a retry/double-tap returns the original, never a second charge. |
| `customer_provider` | `text` (nullable) | Resolved customer operator (pawaPay code); captured at start so the refund leg formats to the right decimal precision. |
| `merchant_provider` | `text` (nullable) | Resolved merchant operator; drives the settlement payout. |
| `deposit_id` | `text` (nullable) | pawaPay deposit (collection) operation id (UUIDv4). Persisted so async callbacks correlate and a refund can reference the original deposit. |
| `payout_id` | `text` (nullable) | pawaPay payout (settlement) operation id. |
| `refund_id` | `text` (nullable) | pawaPay refund operation id (only on the failure→refund path). |

Invariants enforced at start (`orchestrator.start_transaction`): `amount > 0`; `fee.currency == amount.currency`; **`fee < amount`** (we never settle a negative amount to the merchant).

> _Persistence note:_ the Postgres adapter adds the usual `created_at` / `state_changed_at` / `completed_at` timestamps and indexes; the **domain dataclass** above is the authoritative field set. Money is stored as integer **minor units + currency**, never a float.

### `ledger_entry` (double-entry)
**The source of truth for money.** Every money movement is a balanced **posting** of two-or-more **entries**; for any posting, debits == credits per currency, or it is rejected (`UnbalancedPosting`). The transaction row is just a tracker on top of this.

| Field | Type | Notes |
|---|---|---|
| `transaction_id` | `text` | The transaction this posting belongs to. |
| `account` | `text` | One of the fixed accounts below. |
| `direction` | `text` | `debit` or `credit`. |
| `amount` | `Money` | Minor units + currency. |

**Accounts (built):**
| Account | Meaning |
|---|---|
| `customer:external` | The customer's mobile-money wallet (outside our system). |
| `merchant:external` | The merchant's settlement wallet (outside our system). |
| `pawapay:clearing` | Funds held at pawaPay mid-transfer. |
| `revenue:fees` | Our fee (MDR) income. |

**Postings the orchestrator makes (exactly as built):**
1. **Collection succeeds:** `DEBIT customer:external amount` · `CREDIT pawapay:clearing amount` — the customer's funds are now held at pawaPay.
2. **Settlement succeeds:** `DEBIT pawapay:clearing amount` · `CREDIT merchant:external (amount − fee)` · `CREDIT revenue:fees fee` — the merchant nets `amount − fee`; **the fee is booked as revenue only here**; clearing drains to zero. (The `revenue:fees` line is omitted only if the fee is zero.)
3. **Refund succeeds** (settlement had failed): `DEBIT pawapay:clearing amount` · `CREDIT customer:external amount` — the customer is made whole; **no fee is charged**.

> **Fee direction, stated plainly:** the customer pays `amount`. The merchant receives `amount − fee`. We keep `fee`, and only on a successful settlement. There is **no `total_debited = amount + fee`** anywhere — that payer-side construct from the old consumer model does not exist here.

> _Built today:_ the in-memory balancing ledger primitives (`Posting`, `Entry`, the per-currency balance invariant). _Planned:_ the persistent **append-only** Postgres ledger (immutable rows) built on these types, plus the daily reconciliation sweep against pawaPay's status API.

---

## Planned entities (specced, not yet built)

These are designed and consistent with the engineering standards (`../drc-pay/CLAUDE.md`) but are **not in the code yet** (the `auth` domain is a stub; the webhook receiver is Phase D).

### `merchant_auth` (1:1 with `merchant`)
Merchant-app sign-in: phone + OTP, then PIN.

| Column | Type | Notes |
|---|---|---|
| `merchant_id` | `text` | FK → `merchant.id`. |
| `phone_e164` | `text` | The merchant's login number (`+243…`). Unique. |
| `pin_hash` | `text` | **Argon2id** hash of the PIN. Never logged, never recoverable. |
| `pin_failed_count` | `integer` | Resets on success; lock at a threshold. |
| `pin_locked_until` | `timestamptz` (nullable) | Set on lockout; cleared after OTP re-verify. |
| `created_at` / `last_login_at` | `timestamptz` | |

PIN reset is **OTP-only** (no recoverable PIN). Sessions (JWT) can be modelled as a `session` table for server-side revocation if/when needed.

### `otp_attempt`
Short-lived; tied to a phone until verified.

| Column | Type | Notes |
|---|---|---|
| `id` | `uuid` | PK. |
| `phone_e164` | `text` | Indexed. |
| `code_hash` | `text` | Argon2id hash of the 6-digit code. |
| `purpose` | `text` | `signup` \| `signin` \| `pin_reset`. |
| `expires_at` | `timestamptz` | TTL ~5 minutes. |
| `attempt_count` | `integer` | Wrong-guess counter; capped. |
| `verified_at` | `timestamptz` (nullable) | Set on success; row purged ~24h later. |

Rate-limit policy: a few sends per phone per 15 min; a daily cap per phone.

### `operator_event` (webhook audit)
Audit log of every pawaPay webhook related to a transaction — the basis for advancing the state machine asynchronously (Phase D).

| Column | Type | Notes |
|---|---|---|
| `id` | `uuid` | PK. |
| `transaction_id` | `text` (nullable) | FK → `transaction.id`; nullable to capture unmatched events for investigation. |
| `event_type` | `text` | `deposit.pending` / `deposit.completed` / `deposit.failed` / `payout.completed` / `refund.completed` / etc. (mirrors pawaPay). |
| `pawapay_event_id` | `text` | Unique — idempotency on the webhook side. |
| `payload_json` | `jsonb` | Full payload (encrypted at rest; may contain PII). |
| `signature_valid` | `boolean` | Whether pawaPay's **RFC-9421 public-key signature (ECDSA-P256)** verified against its fetched public key — **not HMAC**. |
| `received_at` / `processed_at` | `timestamptz` | |

> This is **still valid and unchanged** by the merchant pivot — pawaPay's callback security model is the same regardless of payer/payee framing.

### `operator_status`
One row per operator (3 in v1) for the availability banner.

| Column | Type | Notes |
|---|---|---|
| `operator` | `text` | PK. `vodacom` \| `airtel` \| `orange`. |
| `success_rate_15m` | `real` | 0.0–1.0; recomputed periodically. |
| `degraded_threshold` | `real` | Default ~0.80. |
| `state` | `text` | `healthy` \| `degraded` \| `unavailable`. |
| `manual_override` | `text` (nullable) | Ops force-set when telemetry is wrong. |

---

## Transaction state machine

The single most important diagram in the system — and it is **built exactly as drawn** (`domains/transactions/state_machine.py`). A payment is two legs (collect from the customer, settle to the merchant) with an automatic refund to the customer if settlement fails after collection already succeeded. Illegal transitions **raise** — they are bugs, not edge cases.

```
                 ┌─────────────────────────┐
                 │      initiated          │  (transaction created;
                 │                         │   not yet sent to pawaPay)
                 └────────────┬────────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │   collection_pending    │  (Deposit API called;
                 │                         │   customer gets the MNO PIN prompt)
                 └────────────┬────────────┘
                ┌─────────────┴─────────────┐
                │                           │
                ▼                           ▼
   ┌─────────────────────┐     ┌─────────────────────┐
   │ collection_failed   │     │ collection_succeeded│
   │ TERMINAL            │     └──────────┬──────────┘
   │ (declined;          │                │  ledger: DEBIT customer:external,
   │  nothing taken)     │                │          CREDIT pawapay:clearing
   └─────────────────────┘                ▼
                               ┌──────────────────────┐
                               │   payout_pending     │  (settle amount − fee
                               │                      │   to the merchant)
                               └──────────┬───────────┘
                              ┌───────────┴───────────┐
                              │                       │
                              ▼                       ▼
                  ┌────────────────────┐  ┌─────────────────────┐
                  │ payout_succeeded   │  │  payout_failed      │
                  │ TERMINAL (success) │  │                     │
                  │ ledger: DEBIT      │  └──────────┬──────────┘
                  │ clearing; CREDIT   │             │
                  │ merchant (amt−fee),│             ▼
                  │ CREDIT revenue:fees│  ┌──────────────────────────┐
                  └────────────────────┘  │   refund_pending         │
                                          │  (auto-refund the full   │
                                          │   amount to the customer)│
                                          └────────────┬─────────────┘
                              ┌────────────────────────┴───────────────┐
                              ▼                                         ▼
                  ┌─────────────────────┐                  ┌────────────────────────┐
                  │  refunded           │                  │  manual_review         │
                  │  TERMINAL           │                  │  (refund failed; funds │
                  │  ledger: DEBIT      │                  │   stuck at pawaPay;    │
                  │  clearing; CREDIT   │                  │   ops resolves)        │
                  │  customer:external  │                  │  NOT terminal          │
                  └─────────────────────┘                  └────────────────────────┘
```

Allowed transitions (from the code):
- `initiated → {collection_pending, manual_review}`
- `collection_pending → {collection_succeeded, collection_failed, manual_review}`
- `collection_succeeded → {payout_pending, manual_review}`
- `payout_pending → {payout_succeeded, payout_failed, manual_review}`
- `payout_failed → {refund_pending, manual_review}`
- `refund_pending → {refunded, manual_review}`
- `manual_review → {payout_pending, refund_pending, refunded, collection_failed}` (a human pushes it back onto a normal path)
- **Terminal:** `collection_failed`, `payout_succeeded`, `refunded`. `manual_review` is the only non-terminal state needing human action.

Additional rules (planned, per the standards): any pending state untouched too long → the reconciliation sweep queries pawaPay and forces a transition; persistently-stuck transactions surface in an admin view.

---

## Indexing strategy (highlights, for the Postgres adapter)
- `transaction(state, created_at)` partial WHERE `state` not terminal — the reconciliation worker's bread and butter.
- `transaction(merchant_id, created_at DESC)` — the merchant's payments list + live feed.
- `transaction(deposit_id)` / `transaction(payout_id)` / `transaction(refund_id)` unique — webhook correlation.
- `transaction(idempotency_key)` unique — dedup on retry/double-tap.
- `merchant(short_code)` unique — USSD till lookup (`get_by_short_code`).

## Retention
- `transaction` / `ledger_entry`: indefinite (regulatory).
- `operator_event`: indefinite for matched transactions; ~90 days for unmatched events.
- `otp_attempt`: ~24 hours after verification or expiry.

## Future considerations (not v1)
- Partitioning `transaction` and `operator_event` by month once volume requires it.
- Persisted append-only ledger with immutable rows + a separate read replica for analytics.
- Column-level PII encryption (KMS-backed) — mandatory before any held-balance model (which v1 deliberately avoids).
- Customer accounts — only if/when a consumer app is built (out of scope now).

## Open questions (to settle during build or with new team)
- **Argon2id parameters** for the planned PIN hashing (e.g. memory=64 MB, iterations=3, parallelism=1) — validate cost on the chosen runtime.
- **Store the full webhook JSON encrypted vs plaintext** — default encrypted (it can carry the customer number / PII).
- **Whether `merchant` needs soft-delete** vs `status='suspended'` only — default: `suspended` covers v1; add soft-delete if a hard removal is ever required.
- **Persisted timestamps on the domain model** — the dataclass omits them today; the Postgres adapter adds them. Decide whether to lift `created_at`/`state_changed_at` into the domain entity.

## Sources
- Built domain model: `../drc-pay/services/api/src/drc_pay_api/domains/` (merchants, transactions, ledger) and `application/` (start_merchant_payment, payment_codes)
- [functionality.md](./functionality.md) — fee direction, flows, state-machine outcomes
- [tech-stack.md](./tech-stack.md) — Postgres/Redis choice
- [own-aggregator.md](../02-findings/cross-cutting/own-aggregator.md) — double-entry ledger rationale
- [pawapay-api-deep-dive.md](../02-findings/aggregators/pawapay-api-deep-dive.md) — webhook signing (RFC-9421/ECDSA-P256), operation ids
