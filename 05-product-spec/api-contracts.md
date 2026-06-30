# API contracts (v1 MVP)

Endpoints, request/response shapes, conventions for the **merchant-acquiring** API. This documents the **built surface** (`../drc-pay/services/api/src/drc_pay_api/http/` + `ussd/`) plus the **planned** auth and webhook endpoints. The contract between the merchant app (and the USSD bearer) and the backend.

- **Last updated:** 2026-06-11
- **Status:** built endpoints documented as-is; auth + webhook marked planned

> **Direction (2026-06-11): merchant-facing.** Rewritten for the merchant pivot. The payment endpoint takes `{customer_msisdn, merchant_id, amount}` (the **merchant-initiated charge-by-number** fallback); the **primary customer channel is `POST /ussd`** (customer-initiated, served to the USSD bearer). The old consumer endpoints (`/transactions/preview` with recipient+pin, `/recipients/recent`, `/auth/pin/*` as the **payer's** auth) are gone. **Fee fields reflect MDR direction** — the customer pays `amount`; the merchant nets `amount − fee`; there is no `total_debited` payer construct.

> **Subject to revision when team grows.** New engineers may prefer different error-envelope shapes, pagination, or REST-vs-GraphQL (default REST). The endpoint set and semantics are the durable parts; serialization details refine during the build.

## Conventions

### Base URL
- Sandbox: `https://api.sandbox.[brand].cd/v1`
- Production: `https://api.[brand].cd/v1`

The built FastAPI app titles itself **"DRC Pay — Merchant Acquiring API"** and serves interactive docs at `/docs` and `/openapi.json`.

### Authentication
- **Built today:** the payment/merchant/USSD read+write endpoints below are **not yet auth-gated** (the `auth` domain is a stub). Merchant auth is **planned** (phone + OTP + PIN → JWT).
- **Planned:** all endpoints except OTP and webhooks require `Authorization: Bearer <jwt>`; the **USSD bearer** authenticates separately (shared secret / IP allowlist at the `/ussd` boundary); **webhooks authenticate by signature**, not a bearer token.

### Required headers
| Header | Purpose | Status |
|---|---|---|
| `Idempotency-Key: <uuidv4>` | All money-moving `POST` (`/transactions`) — a retry/double-tap returns the original, never a second charge | **built** (`Idempotency-Key` header honoured) |
| `Authorization: Bearer <token>` | Authenticated merchant endpoints | planned |
| `X-App-Version: <semver>` | Force-update check | planned |
| `X-Platform: ios\|android` | Telemetry | planned |
| `Accept-Language: fr-CD` | French strings (merchant app); USSD strings localised at the bearer | planned |

### Error envelope (planned shape)
Non-2xx responses share:
```json
{ "error": { "code": "merchant/not_found", "message": "Commerçant introuvable.", "details": {} } }
```
- `code` — a stable enum the client switches on.
- `message` — French, user-presentable.
- `details` — optional structured context.

> _Built today_ uses FastAPI's default `{"detail": "..."}` for `HTTPException` (e.g. `404 "merchant not found"`, `422 "amount must be positive"`). Harmonising to the envelope above is a planned refinement.

### Standard response codes
`200` success · `201` created · `204` no body · `400` validation · `401` unauthenticated · `403` forbidden (e.g. over-limit) · `404` not found · `409` idempotency replay with a different body · `422` semantic error (bad amount/currency, inactive merchant, unknown scenario) · `429` rate-limited · `503` operator unavailable · `5xx` server error.

### Idempotency
Per-merchant `Idempotency-Key` is unique. **Built behaviour:** on a repeat key the server returns the **original** transaction (with a trace line `"idempotent replay · key already processed; original returned"`) rather than creating a second one. Mismatched body with the same key → `409` (planned).

### Server re-derivation (non-negotiable)
The server **never trusts the client** for the fee, the merchant settlement target, or the resolved operators. The **fee (MDR) is computed server-side** (`pricing.default_fee`), the **settlement number is read from the `Merchant` record** (never from the request body), and operators are resolved via pawaPay predict-provider (or an explicit override only for the customer leg).

---

## Endpoints

### Transactions

#### `POST /transactions`  — built (merchant-initiated charge-by-number)
Record a customer paying a registered merchant, and start the two legs (collect from the customer → settle to the merchant). This is the **charge-by-customer-number fallback** path; the customer then approves on their own handset (the MNO PIN prompt). On the in-process simulator, the demo outcome is played out by `scenario`.

Request:
```json
{
  "customer_msisdn": "+243812345678",
  "merchant_id": "m_alpha",
  "amount": "10.00",
  "currency": "USD",
  "scenario": "success",
  "customer_provider": null
}
```
- `customer_msisdn` — the customer to charge.
- `merchant_id` — the registered merchant being paid (its settlement number + operator are read server-side).
- `amount` — major-units string (the **sticker price the customer pays**).
- `currency` — `USD` (default) or `CDF`.
- `scenario` — **demo control only** (simulator): `success` | `payout_fail` | `collection_fail` | `refund_fail`; **ignored on the live rail** (the real outcome arrives via webhook).
- `customer_provider` — optional pawaPay operator override for the customer leg; resolved via predict-provider when omitted.

Header: `Idempotency-Key: <uuidv4>` (recommended; dedups retries).

Response `200` (`TransactionResponse`):
```json
{
  "id": "9f3c…",
  "customer_msisdn": "+243812345678",
  "merchant_id": "m_alpha",
  "merchant_name": "Alpha Gas Station",
  "merchant_msisdn": "243810000001",
  "amount": "10.00",
  "fee": "0.10",
  "currency": "USD",
  "state": "payout_succeeded",
  "history": ["initiated","collection_pending","collection_succeeded","payout_pending","payout_succeeded"],
  "ledger": [
    { "account": "customer:external", "direction": "debit",  "amount": "10.00", "currency": "USD" },
    { "account": "pawapay:clearing",  "direction": "credit", "amount": "10.00", "currency": "USD" },
    { "account": "pawapay:clearing",  "direction": "debit",  "amount": "10.00", "currency": "USD" },
    { "account": "merchant:external", "direction": "credit", "amount": "9.90",  "currency": "USD" },
    { "account": "revenue:fees",      "direction": "credit", "amount": "0.10",  "currency": "USD" }
  ],
  "customer_provider": "VODACOM_MPESA_COD",
  "merchant_provider": "AIRTEL_COD",
  "deposit_id": null,
  "payout_id": null,
  "refund_id": null,
  "trace": [
    "POST /transactions · 10.00 USD · scenario=success",
    "merchant · Alpha Gas Station (m_alpha) · settle → 243810000001",
    "pricing · fee (MDR) = 1% of 10.00 = 0.10 USD · merchant nets 9.90 USD",
    "…"
  ]
}
```

**Fee fields (MDR direction):** `amount` is what the **customer pays**; `fee` is our **MDR**; the **merchant nets `amount − fee`** (here 10.00 → 9.90). The ledger shows the fee credited to `revenue:fees` only on a successful settlement. **There is no `total_debited`** — the customer is never charged `amount + fee`. On the live rail, `deposit_id` / `payout_id` / `refund_id` carry the pawaPay operation ids (null on the simulator).

Errors (built): `404` merchant not found · `422` inactive merchant / bad amount-or-currency / non-positive amount / unknown `scenario`.

---

#### `GET /transactions/{id}`  — built
Single transaction with current state (same `TransactionResponse` shape; `trace` empty on a plain read). The merchant app uses this to resume a pending charge after a connectivity blip, and to render the payment detail.

Errors: `404` transaction not found.

---

#### `GET /transactions`  — built
List transactions (array of `TransactionResponse`). Backs the merchant **payments list** and **live feed**.

> _Planned refinements:_ scope to the authenticated merchant (`merchant_id` from the token), add cursor pagination (`?limit=&cursor=`) and filters (`?state=&since=`), and add a short-lived `receipt_share_url` for WhatsApp previews.

---

### Merchants

#### `GET /merchants`  — built
List merchants, each as a `MerchantResponse` (includes the computed payment codes). _Planned:_ scope to the authenticated owner.

#### `GET /merchants/{id}`  — built
Single merchant.

Response `200` (`MerchantResponse`):
```json
{
  "id": "m_alpha",
  "name": "Alpha Gas Station",
  "short_code": "1001",
  "settlement_msisdn": "243810000001",
  "status": "active",
  "ussd_string": "*123*1001#",
  "tel_uri": "tel:*123*1001%23",
  "qr_svg_path": "/merchants/m_alpha/qr.svg"
}
```
- `ussd_string` — what the customer dials (`*123*1001#`).
- `tel_uri` — the scannable QR payload (a `tel:` USSD dial-through; offers to dial on Android — iOS blocks `*`/`#`).
- `qr_svg_path` — where to fetch the printable QR.

Errors: `404` merchant not found.

#### `GET /merchants/{id}/qr.svg`  — built
Returns a scannable **SVG QR** of the merchant's `tel:` USSD dial-through (printable on a sticker; rendered with `segno`). `Content-Type: image/svg+xml`.

Errors: `404` merchant not found.

#### Merchant onboarding writes — planned
Not built yet (merchants are seeded in the demo / created via an admin path). Planned surface, consistent with the onboarding screens:
- `POST /merchants` — create a merchant `{ name, settlement_msisdn, settlement_provider? }`; the server assigns/validates a unique `short_code` (till) and returns the `MerchantResponse` (with QR codes). Business-KYC fields are a flagged add.
- `PATCH /merchants/{id}` — update business details or the **settlement account** (`settlement_msisdn` / `settlement_provider`); PIN re-auth required.
- (Status changes `active`/`suspended` via an admin/ops endpoint.)

---

### USSD (customer-initiated channel) — built

#### `POST /ussd`
The **primary customer channel**. The USSD **bearer** (Africa's Talking / Infobip — see [ussd-gateway-providers.md](../02-findings/cross-cutting/ussd-gateway-providers.md)) POSTs each menu step here; the handler replies with the conventional `CON`/`END` string. A USSD payment drives the **same** orchestrator as `POST /transactions` (the money logic is never reimplemented) and shows up in the same `/transactions` surface and merchant feed.

Request (provider-neutral scaffold):
```json
{ "session_id": "ATUid_…", "msisdn": "+243812345678", "text": "1001*10*1" }
```
- `text` — the user's **full accumulated input**, `*`-joined (real aggregators deliver it this way); `""` on the initial dial. A QR/dial that pre-fills the till arrives as `text="1001"` (jumps to the amount), or `text="1001*10"` (jumps to confirm).

Response: `text/plain`, body prefixed `CON` (keep the session open) or `END` (terminate). Step ladder (as built):
| `text` | Reply |
|---|---|
| `""` | `CON Enter merchant till code:` |
| `1001` | `CON Pay Alpha Gas Station\nEnter amount (USD):` |
| `1001*10` | `CON Pay 10.00 USD to Alpha Gas Station?\n1. Confirm\n2. Cancel` |
| `1001*10*1` | `END Payment of 10.00 USD to Alpha Gas Station initiated. Approve on your phone.` → fires the collection; the MNO pushes the **PIN prompt** to the customer |
| `1001*10*2` | `END Cancelled.` |

Unhappy paths: unknown/inactive till → `END Merchant not found.`; bad amount → `END Invalid amount. Please dial again.`; bad choice → `END Invalid choice. Please dial again.`

> **Bearer adapter (the one real delta to go live):** real aggregators POST **form-encoded** fields (`sessionId`, `phoneNumber`, `serviceCode`, `text`) and require a fast HTTPS `200`. Mapping those field names/content-type onto this JSON scaffold (and splitting the `*`-joined `text`) is a **thin adapter at this `/ussd` boundary — no domain/orchestrator change.** USSD is **single-currency (USD)** here for now, and the menu strings are **English in code, French in production**.

---

### System

#### `GET /health`  — built
Liveness probe.
```json
{ "status": "ok", "environment": "dev" }
```

#### `GET /system/operator-status` — planned
Operator availability for the merchant-app banner (one row per operator: `state`, `success_rate_15m`).

#### `GET /system/version` — planned
Force-update check (`minimum_supported_version`, `force_update`, store URLs).

---

### Auth (merchant) — planned (not built)

The `auth` domain is a stub today. Planned surface (PINs **Argon2id**-hashed, never logged, **reset only via OTP**):
- `POST /auth/otp/send` `{ phone_e164, purpose }` — send a 6-digit OTP via Africa's Talking.
- `POST /auth/otp/verify` `{ phone_e164, code, purpose }` — verify; returns tokens + `next_step` (`create_pin` for a new merchant, `ready` otherwise).
- `POST /auth/pin/create` `{ pin }` — set the merchant-app PIN after signup.
- `POST /auth/pin/verify` `{ pin }` — re-auth before sensitive actions (charge-by-number, changing the settlement number).
- `POST /auth/pin/change` `{ current_pin, new_pin }`.
- `POST /auth/refresh` `{ refresh_token }` · `POST /auth/signout`.

> Note this is the **merchant's** auth (the account holder), **not** a payer's. The **customer never authenticates to us** — they authorise each payment on **their operator's own PIN prompt**, which we never see.

---

### Webhooks (server-to-server; no bearer) — planned (Phase D)

#### `POST /webhooks/pawapay`
Receive pawaPay callbacks (the live rail's real outcomes; the simulator plays them out in-process instead). pawaPay signs callbacks with **RFC-9421 HTTP Message Signatures using public-key cryptography (ECDSA-P256-SHA256 by default)** — **not HMAC**. Callbacks carry `Signature`, `Signature-Input`, `Signature-Date`, and `Content-Digest` headers; signed callbacks must be enabled in the pawaPay Dashboard. ([pawaPay — Using the API](https://docs.pawapay.io/using_the_api))

Behaviour:
1. **Verify the callback:** hash the body per `Content-Digest`, reconstruct the signature base from `Signature-Input`, fetch (and cache) pawaPay's public key, and cryptographically verify `Signature` (ECDSA-P256). Reject `401` if invalid or stale.
2. Insert an `operator_event` row (idempotent by `pawapay_event_id`).
3. Correlate to the transaction by the stored `deposit_id` / `payout_id` / `refund_id`; invoke the matching orchestrator handler (`on_collection_result` / `on_payout_result` / `on_refund_result`) to advance the state machine and post the balanced ledger entries.
4. On a terminal state, fan out: merchant-app push, finalised ledger, auto-refund already handled by the state machine on `payout_failed`.
5. Respond `200` fast (well under a second); offload heavy work to a queue.

> Unchanged by the pivot — pawaPay's signing model is the same regardless of payer/payee framing.

---

## Rate limits (planned)
| Endpoint | Limit | Window |
|---|---|---|
| `POST /auth/otp/send` | a few per phone | 15 min |
| `POST /auth/pin/verify` | small wrong-count then lock | per session |
| `POST /transactions` | per-merchant velocity cap | 1 h / 24 h |
| `GET /transactions/{id}` | polling cap | 1 min |
| `POST /ussd` | per-msisdn session cap | short window |
| Webhook ingest | unlimited | n/a |

Velocity caps are enforced both at the rate-limit layer (`429`) and the business layer (`403 limits/exceeded`) — the latter covers monetary caps; the former covers HTTP abuse.

---

## OpenAPI / SDK generation
- The backend serves `/openapi.json` (FastAPI-generated) and interactive docs at `/docs`.
- The merchant app generates a TypeScript client from the spec into `src/generated/` via a pre-commit script; schema drift fails CI.

## Webhook simulator for local dev
- The in-process **simulator rail** plays out pawaPay outcomes (chosen by `scenario`) through the same `on_*_result` handlers — so the whole flow (success, payout-fail → refund, refund-fail → manual review) runs offline and deterministic with **no external services**. (`integrations/pawapay/simulator.py`; standards: `../drc-pay/CLAUDE.md`.)

## Open questions (to settle during build or with new team)
- **Error-envelope harmonisation** — adopt the `{ "error": {...} }` shape across all endpoints (built code uses FastAPI's `{"detail": …}` today).
- **Auth wiring** — when the merchant-auth domain lands, gate the merchant/transaction endpoints and scope list endpoints to the token's merchant.
- **`POST /transactions` field naming** — the built field is `customer_provider` (customer-leg override only); merchant operator is always server-derived. Keep or expose a read-only resolved-operator echo (already returned).
- **GraphQL vs REST** — default REST. Revisit only if web-admin / partner clients proliferate.
- **`receipt_share_url`** — add a short-TTL public receipt page for WhatsApp previews (not built).

## Sources
- Built HTTP/USSD surface: `../drc-pay/services/api/src/drc_pay_api/http/` (`routes.py`, `merchant_routes.py`, `ussd_routes.py`, `schemas.py`) and `ussd/session.py`
- [data-model.md](./data-model.md) — entity definitions + state machine
- [functionality.md](./functionality.md) — behaviour, fee direction, limits
- [tech-stack.md](./tech-stack.md) — language/framework + the USSD bearer
- [ussd-gateway-providers.md](../02-findings/cross-cutting/ussd-gateway-providers.md) — the `CON/END` callback model and the thin `/ussd` adapter
- [pawapay-api-deep-dive.md](../02-findings/aggregators/pawapay-api-deep-dive.md) — webhook signing (RFC-9421/ECDSA-P256), operation ids
