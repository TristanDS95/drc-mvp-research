# pawaPay — v2 API deep-dive (build-oriented)

- **Date accessed:** 2026-06-11
- **Overall confidence:** **High** for capabilities/endpoints (pawaPay's public v2 docs are explicit); **Medium** for exact per-provider limits (exposed dynamically via `/v2/active-conf`, not statically published per market).
- **Purpose:** Settle what we can rent vs. must build for the pawaPay integration (Phase A client already built in `drc-pay`).

## Decision-relevant summary
1. **Operator detection is solved by pawaPay** — `POST /v2/predict-provider` maps a phone number → provider + sanitised number. We do **not** need to build our own detector (see `operator-detection.md`).
2. **Per-provider rules are dynamic** — `GET /v2/active-conf` returns each provider's operational status, currencies, **decimal rule** (`TWO_PLACES`/`NONE`), and **min/max amount**. We should consume this rather than hard-code.
3. **The API is asynchronous** — every deposit/payout/refund returns only an `ACCEPTED/REJECTED/DUPLICATE_IGNORED` ack; the **final outcome arrives via callback (webhook)**. So a **callback receiver is mandatory**, not optional. (Verified: [using_the_api](https://docs.pawapay.io/using_the_api).)
4. **pawaPay is itself idempotent** on `depositId`/`payoutId`/`refundId` (duplicate → `DUPLICATE_IGNORED`) — complements our own idempotency keys.

## What pawaPay CAN do (verified — primary docs)
| Capability | Endpoint | Source |
|---|---|---|
| Collect from a payer wallet | `POST /v2/deposits` | [initiate-deposit](https://docs.pawapay.io/v2/api-reference/deposits/initiate-deposit) |
| Pay out to a wallet | `POST /v2/payouts` | [initiate-payout](https://docs.pawapay.io/v2/api-reference/payouts/initiate-payout) |
| Bulk payouts | `POST /v2/payouts/bulk` (**max 20/request**) | [llms-full.txt](https://docs.pawapay.io/llms-full.txt) |
| Refund a deposit | `POST /v2/refunds` (needs the original `depositId`) | [initiate-refund](https://docs.pawapay.io/v2/api-reference/refunds/initiate-refund) |
| Status check (poll) | deposit/payout/refund status endpoints | [llms-full.txt](https://docs.pawapay.io/llms-full.txt) |
| Callback resend | per deposit/payout/refund | [llms-full.txt](https://docs.pawapay.io/llms-full.txt) |
| **Predict provider from a number** | `POST /v2/predict-provider` | [changelog](https://www.pawapay.io/changelog); [providers](https://docs.pawapay.io/v2/docs/providers) |
| **Active configuration** (status, currencies, decimals, min/max, callback URLs, PIN-prompt behaviour) | `GET /v2/active-conf` | [active-configuration](https://docs.pawapay.io/v2/api-reference/toolkit/active-configuration) |
| Wallet balances | `GET /v2/wallet-balances` | [llms-full.txt](https://docs.pawapay.io/llms-full.txt) |
| Statements (CSV/GZIP) | `POST /v2/statements` | [llms-full.txt](https://docs.pawapay.io/llms-full.txt) |
| Hosted checkout | `POST /v2/paymentpage` | [payment_page](https://docs.pawapay.io/v2/docs/payment_page) |
| Optional request signing + signed callbacks | RFC-9421 (Content-Digest + Signature headers) | [signatures](https://docs.pawapay.io/v2/docs/signatures) |

**Auth:** `Authorization: Bearer <token>`, required on all financial calls; separate sandbox/prod tokens. **Base URLs:** `https://api.sandbox.pawapay.io` / `https://api.pawapay.io`. (Verified: [using_the_api](https://docs.pawapay.io/using_the_api).)

## DRC specifics (verified — [providers](https://docs.pawapay.io/v2/docs/providers))
| Provider | Code | CDF | USD | Auth |
|---|---|---|---|---|
| Vodacom M-Pesa | `VODACOM_MPESA_COD` | **no decimals** | 2 dp | `PROVIDER_AUTH` |
| Airtel | `AIRTEL_COD` | 2 dp | 2 dp | `PROVIDER_AUTH` |
| Orange | `ORANGE_COD` | 2 dp | 2 dp | `PROVIDER_AUTH` |

- `PROVIDER_AUTH` = the payer authorises on their own handset via the operator's native prompt (e.g. M-Pesa PIN). **Inference (High):** no `preAuthorisationCode`/OTP collection by us for these three.
- **Amount format gotcha (verified):** Vodacom M-Pesa **CDF takes no decimals** → our `Money.to_major_str()` (always 2 dp) would send an invalid amount. Amount formatting must be **provider/currency-aware** (use `active-conf`'s `decimalsInAmount`).
- **Afrimoney: not supported** by pawaPay in DRC (confirmed in the profile finding `pawapay.md`).

## What pawaPay CANNOT do / limits
- **No Afrimoney (DRC)** — reachability to Africell users via pawaPay is zero.
- **No synchronous completion** — async only; you must handle callbacks (or poll). (Verified.)
- **No QR / non-mobile-money channels** — MMO rails only. (Inference, Medium.)
- **Prediction is not 100%** — provider prediction is "very high accuracy… not 100%, allow override." (Verified — [changelog](https://www.pawapay.io/changelog).)
- **Documented numeric limits:** bulk payouts ≤20/request; batch ≤1000; statements: date range ≤31 days, retention 90 days, global search last 3 months, download URL expires 1 day. (Verified — [llms-full.txt](https://docs.pawapay.io/llms-full.txt).)
- **Settlement minimums:** > $50 local / > $100 cross-border per settlement; one EUR/USD/GBP settlement account. (Verified — [llms-full.txt](https://docs.pawapay.io/llms-full.txt).)
- **Per-provider min/max transaction amounts:** exposed via `active-conf` but **specific DRC values not captured** (would need a live `active-conf` call with credentials). **Not found — Medium/Low.**
- **Settlement cadence (T+?) for DRC:** still **not found** (carried over from `pawapay.md`).

## What WE must build (the integration gaps)
| We build | Why |
|---|---|
| **Callback/webhook receiver** | Mandatory — pawaPay delivers the *final* deposit/payout/refund outcome only via callback. Must verify the RFC-9421 signature and map onto our `on_*_result` handlers. |
| **Persist pawaPay op-ids** | We must store `depositId`/`payoutId`/`refundId` to correlate callbacks back to our transaction, and a refund **requires the original `depositId`**. |
| **Provider-aware amount formatting** | Vodacom M-Pesa CDF = no decimals; others 2 dp. Drive off `active-conf` `decimalsInAmount`. |
| **Prediction + override** | Call `predict-provider`; allow a manual override path for the not-100% cases (wrong-provider failures). |
| **Reconciliation / polling fallback** | Callbacks can be missed → a sweep that polls the status endpoints (our `jobs/reconciliation`). |
| (Optional) **Consume `active-conf`** | For live operator status banner + pre-flight validation (limits/availability). |

**We already have:** the orchestration spine, the double-entry ledger, our own idempotency keys, and the outbound client (Phase A). pawaPay's own idempotency is a bonus safety layer.

## Confidence & open questions
- **High:** the endpoint set, async model, predict-provider, active-conf, DRC provider codes + decimal rules, signing scheme.
- **Open (need sandbox credentials to resolve):** exact DRC per-provider min/max amounts; the precise `predict-provider` and callback JSON payloads; settlement cadence; whether `predict-provider` reliably distinguishes the three DRC operators given number portability.
- **Action flagged:** obtaining sandbox credentials (self-serve at `dashboard.sandbox.pawapay.io`) is a **team action** required before any live verification.

## Sources
- pawaPay — Using the API (base URLs, Bearer auth): https://docs.pawapay.io/using_the_api — primary, 2026-06-11
- pawaPay — Initiate deposit / payout / refund: https://docs.pawapay.io/v2/api-reference/deposits/initiate-deposit · /payouts/initiate-payout · /refunds/initiate-refund — primary, 2026-06-11
- pawaPay — Active configuration: https://docs.pawapay.io/v2/api-reference/toolkit/active-configuration — primary, 2026-06-11
- pawaPay — Providers (DRC codes + decimals): https://docs.pawapay.io/v2/docs/providers — primary, 2026-06-11
- pawaPay — Signatures (RFC-9421): https://docs.pawapay.io/v2/docs/signatures — primary, 2026-06-11
- pawaPay — full docs (capabilities/limits): https://docs.pawapay.io/llms-full.txt — primary, 2026-06-11
- pawaPay — Changelog (provider prediction): https://www.pawapay.io/changelog — primary/self-reported, 2026-06-11
