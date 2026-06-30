# MOKO Afrika — profile

- **Type:** aggregator / PSP (multi-channel: mobile money + cards + bank transfers + POS + QR)
- **Operates in DRC:** **yes — and DRC is its primary market.** Active in DRC and Cameroon; plans for West Africa expansion. ([mokoafrika.com homepage](https://www.mokoafrika.com/en); [moko-africa-documentation.vercel.app API docs](https://moko-africa-documentation.vercel.app/)). **Verified fact, High confidence.**
- **Last updated:** 2026-06-04
- **Overall confidence:** Medium-High for capabilities (technical docs are quite explicit); Low for fees, sandbox URL, and contractual onboarding (not publicly documented).

## Developer access
- **API / developer portal:** Public API docs at `https://moko-africa-documentation.vercel.app/`. Primary API endpoint: `https://paydrc.gofreshbakery.net/api/v5/`. Merchant dashboard: `https://cd.merchants.gofreshpay.com/`. The "gofreshbakery"/"gofreshpay" domain suggests a parent/sister group ("Freshpay"). **Verified fact** ([moko-africa-documentation.vercel.app](https://moko-africa-documentation.vercel.app/); [cd.merchants.gofreshpay.com](https://cd.merchants.gofreshpay.com/)). **High confidence.**
- **Documentation quality:** **Medium-High.** Public docs document authentication (JWT via `/api/v5/token`), endpoints (deposit/payout), callback signature (HMAC-SHA256), and currency precision. Some sub-pages (e.g., `/api/v5/deposit`) returned 404 to direct fetch on this date; index pages worked.
- **Sandbox / test environment:** **Stated to exist** ("Sandbox Environment: Available for testing integrations" on the homepage), but **the sandbox URL was not located** in the public docs. **Confidence: Medium.**

## Capabilities (the core questions)
| Capability | Supported? | Notes | Source | Confidence |
|---|---|---|---|---|
| Collect / debit from a payer wallet | **Yes (Deposit / C2B)** | Standard collection endpoint. | [moko-africa-documentation.vercel.app](https://moko-africa-documentation.vercel.app/); [mokoafrika.com homepage](https://www.mokoafrika.com/en) | High |
| Payout / disburse to any wallet or number | **Yes (Payout / B2C)** — including bulk salary/supplier disbursements with "real-time delivery." | [moko-africa-documentation.vercel.app](https://moko-africa-documentation.vercel.app/); [mokoafrika.com homepage](https://www.mokoafrika.com/en) | High |
| Supported operators (DRC) | **All four: Vodacom M-Pesa (`mpesa`), Airtel (`airtel`), Orange (`orange`), AND Africell Afrimoney (`africell`)** — **MOKO is the only aggregator I located that explicitly lists Africell**. | [moko-africa-documentation.vercel.app provider identifiers](https://moko-africa-documentation.vercel.app/) | High |
| Cross-network bridging | **Yes (functional)** — collect from one MNO, pay out to another via merchant account. | inference from architecture | High |
| Balance / wallet custody (held-balance model) | **Likely yes** (merchant maintains balance with MOKO; payout draws from it). | inference | Medium |
| Transaction status / reconciliation | **Yes** — dashboard plus API callbacks (HMAC-SHA256 signed). | [moko-africa-documentation.vercel.app](https://moko-africa-documentation.vercel.app/) | High |
| QR / payee identification | **Yes — "QR code payments for retail" + POS** | [mokoafrika.com homepage](https://www.mokoafrika.com/en) | Medium |

## Money movement
- **Payer approval flow:** Standard MNO USSD/STK PIN approval driven via the operator API; MOKO is initiator. **Inference, Medium confidence.**
- **Settlement timing:** **Not found in public docs.** **Confidence: Low.**
- **Currencies (CDF / USD):** **Both CDF and USD.** Phone validation requires the `+243` prefix (DRC country code) — i.e., MOKO is DRC-focused. **Verified fact, High confidence** ([moko-africa-documentation.vercel.app](https://moko-africa-documentation.vercel.app/)).

## Costs
- **Per-transaction fees (DRC):** **Not publicly disclosed.** Homepage references a "Tarification" page but content not extracted. **Confidence: Low — requires sales contact.**
- **Aggregator margin:** Not publicly disclosed.
- **Other costs / minimums:** Not found.

## Onboarding
- **Who can sign up / requirements:** Homepage describes "3 simple steps": account creation → integration → receive payments. Sign-in page exists at `mokoafrika.com/en/signin`. Merchant dashboard at `cd.merchants.gofreshpay.com`. Public flow appears to be self-serve-style, but contract/KYC is required (industry norm; not publicly itemised). **Confidence: Medium.**
- **KYC handled by provider?:** Merchant KYC required; end-user KYC remains MNO-side. **Inference.**
- **Contract / commercial terms:** Not public.
- **Authentication (API):** **JWT tokens** — merchant generates token via `/api/v5/token` using merchant ID + merchant secret. **Callback security:** HMAC-SHA256 signature + AES-256-CBC encryption + IP whitelisting. **Verified fact, High confidence** ([moko-africa-documentation.vercel.app](https://moko-africa-documentation.vercel.app/)).

## Reliability & track record
- **Scale / adoption (note if self-reported):**
  - "Over **25 million mobile wallets in DRC**" addressable / "47M+ mobile wallets" claimed accessible total — **company self-reported** ([mokoafrika.com homepage](https://www.mokoafrika.com/en)). Note: this is **reachability claim**, not transaction count or merchant count.
  - "Thousands of enterprises in DRC" using the platform — **company self-reported**.
  - Named clients (per homepage): **PremierBet, MulaSport, BetAndU, Equity BCDC, 1XBET, Cris** — **company self-reported**, but the inclusion of established names (Equity BCDC is a major DRC bank; 1XBET, PremierBet etc. are large betting operators) suggests real adoption. **Medium confidence.**
  - Linked to "GoFresh" / "Freshpay" domain group — **parent/sister relationship inferred, not publicly explained**.
- **Known issues:**
  - Pricing opacity (no public tariff).
  - Some API doc sub-pages return 404 to direct fetch on this date (indexing inconsistency).
  - Smaller and less internationally visible than pawaPay or DPO.

## Open questions / gaps
- Founder, ownership, and regulatory status (BCC licence? Operates under partner licence?) — not publicly disclosed.
- Per-transaction fees for DRC mobile money (collection and payout, per operator).
- Sandbox URL and onboarding speed.
- Whether Afrimoney support in the docs is **live** versus listed (and how stable Afrimoney integration is, given Afrimoney's smaller share and platform churn — Comviva platform, Dec 2023 GSMA spec adoption).
- Settlement cadence (T+0 / T+1 / weekly) and to what type of accounts (USD/CDF DRC bank? foreign?).
- Rate limits and per-transaction maximums.
- Compliance posture for foreign merchants (BCC partner-PSP arrangement?).

[[fees-and-costs]] · [[payee-identification]] · [[kyc-and-onboarding]]
