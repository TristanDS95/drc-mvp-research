# Operator detection — which network owns a phone number?

- **Date accessed:** 2026-06-11
- **Confidence:** **High** that pawaPay solves this for us; **Medium** on accuracy specifics for the three DRC operators (needs sandbox testing).

## The problem
Our product is "pay any number." But pawaPay (and every MMO rail) needs the **provider code** — `VODACOM_MPESA_COD`, `AIRTEL_COD`, or `ORANGE_COD` — to route a deposit/payout. A recipient only gives us a phone number. So: **how do we turn a number into an operator?**

## Answer: pawaPay does it — `POST /v2/predict-provider`
pawaPay exposes a **Provider Prediction** endpoint that we already pay for as part of the API. (Verified — [changelog](https://www.pawapay.io/changelog); referenced from [providers](https://docs.pawapay.io/v2/docs/providers).)

- **Request:** `{ "phoneNumber": "<raw, messy input ok>" }`
- **Response:** `{ "country": "...", "provider": "<CODE>", "phoneNumber": "<sanitised E.164-ish>" }`
- **It also validates/sanitises** the number — strips whitespace/characters, checks the digit count for the country, handles leading zeros. So it doubles as **phone-number validation**, which we need anyway (some countries don't strictly follow ITU E.164).
- **Self-learning**, "gets more accurate over time."
- **Caveat (verified):** "accuracy is very high in most countries, **not 100%** — recommended to make it possible to **override** the prediction." So we must allow a fallback when prediction is wrong or a transaction fails as wrong-provider.

**Implication:** we do **not** build our own detector. We call `predict-provider`, use its provider + sanitised number, and provide an override path.

## Alternatives considered (and why not)
| Method | Verdict |
|---|---|
| **pawaPay `predict-provider`** | **Use this.** Purpose-built for African MMOs, included in the API, also validates the number, self-improving. |
| Third-party number/HLR lookup (generic telco lookup services) | Rejected for MVP — extra vendor + cost, and generic lookups don't reliably map to pawaPay's MMO provider codes for DRC. Inference, Medium. |
| DIY number-prefix table (DRC numbering plan) | Rejected — brittle: **mobile number portability** means prefix ≠ current operator; ongoing maintenance burden. Inference, Medium. |
| Ask the user to pick the network | Keep only as the **override/fallback**. Reliable but dents the "just type a number" UX, and most users don't know the recipient's network. |

## Recommended approach for the app
1. On entering a recipient number, call `predict-provider` → get provider + sanitised number (also our validation step).
2. Proceed with the predicted provider for the deposit/payout.
3. **Override path:** if the user knows better, or a transaction fails with a wrong-provider failure code, let them (or a retry policy) select the operator manually.
4. (Optional) cross-check against `active-conf` operational status before initiating, to fail fast if a provider is `CLOSED`/`DELAYED`.

## Open questions
- **DRC-specific accuracy** of `predict-provider` across Vodacom/Airtel/Orange (esp. with portability) — **not verifiable without sandbox testing.** Flagged for Phase E live testing.
- Whether `predict-provider` returns a confidence score or just a single best guess — **not found**; assume single guess + override.
- DRC mobile number portability status/prevalence — **not researched here**; relevant to how often the override is needed.

## Sources
- pawaPay — Changelog (Provider Prediction / predict-correspondent): https://www.pawapay.io/changelog — primary/self-reported, 2026-06-11
- pawaPay — Providers: https://docs.pawapay.io/v2/docs/providers — primary, 2026-06-11
- pawaPay — Active configuration (operational status): https://docs.pawapay.io/v2/api-reference/toolkit/active-configuration — primary, 2026-06-11
- (endpoint path `POST /v2/predict-provider` + request/response shape obtained via pawaPay docs index search, 2026-06-11; confirm exact schema against the live doc page / sandbox)
