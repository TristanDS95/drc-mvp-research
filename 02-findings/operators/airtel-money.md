# Airtel Money — profile

- **Type:** mobile money operator
- **Operates in DRC:** **yes** — Airtel Africa lists DRC as one of its 14 mobile money markets. ([Airtel Africa Wikipedia listing supported countries — secondary](https://en.wikipedia.org/wiki/Airtel_Africa); [airtel.cd Airtel Money — landing page](https://www.airtel.cd/airtelmoney/envoi-argent-international-a-partir-d-airtel-money)). **Verified fact.**
- **Last updated:** 2026-06-04
- **Overall confidence:** Medium-High — Airtel Africa runs a single unified developer portal across markets including DRC, with technical details (auth, sandbox/prod URLs, product list) publicly documented. DRC-specific consumer fees and contractual onboarding details are not publicly extractable from airtel.cd (JS-rendered pages).

## Developer access
- **API / developer portal:** `https://developers.airtel.africa/` (and `https://developers.airtel.africa/developer` for the application registration flow) — the unified Airtel Africa Open API portal across all 14 Airtel Money markets, **including DRC**. **Verified fact** ([developers.airtel.africa Developer Portal](https://developers.airtel.africa/); [Airtel developer portal upgrade article, tech-ish.com 2021-11-19, secondary](https://tech-ish.com/2021/11/19/developer-portal-airtel-money-api/)).
- **Documentation quality:** Self-serve. Developer can sign up, register an app, add products (collection, disbursement, KYC, etc.), select one or multiple countries, and go live after onboarding ([Airtel developer portal upgrade article, tech-ish.com — secondary, 2021-11-19](https://tech-ish.com/2021/11/19/developer-portal-airtel-money-api/)). Full reference is behind portal authentication. **Confidence: Medium-High** (multiple sources corroborate; DRC-specific docs not directly inspected).
- **Sandbox / test environment:** **Yes — staging URL `https://openapiuat.airtel.africa`** (production: `https://openapi.airtel.africa`). **Verified fact** ([tech-ish.com Airtel developer portal article, 2021-11-19, secondary](https://tech-ish.com/2021/11/19/developer-portal-airtel-money-api/); cross-referenced in [Medium integration guide by Muhanzi, secondary](https://medium.com/@muhanzi/how-to-integrate-airtel-money-api-for-payment-collections-and-remittances-67d2202cd488) — body returned 504 so URL pattern verified only via search index).

## Capabilities (the core questions)
| Capability | Supported? | Notes | Source | Confidence |
|---|---|---|---|---|
| Collect / debit from a payer wallet | Yes | Airtel Africa portal exposes Collection API for merchants. | [tech-ish.com Airtel developer portal article, secondary](https://tech-ish.com/2021/11/19/developer-portal-airtel-money-api/) | High |
| Payout / disburse to any wallet or number | Yes (Disbursement to Airtel Money wallets) | Portal lists Disbursement. Whether the target wallet must be a registered Airtel Money user is **not explicitly stated** in public docs; in most Airtel Africa markets the API expects an Airtel Money MSISDN. | [tech-ish.com](https://tech-ish.com/2021/11/19/developer-portal-airtel-money-api/) | Medium |
| Supported operators (which networks) | Airtel Money only (Airtel DRC) | Operator API. Cross-network is at consumer level via the interop page on airtel.cd (URL exists: `/airtelmoney/interopearbilite-avec-orange-money-m-pesa`); page body did not extract via fetch so cross-network mechanics couldn't be verified. | [airtel.cd interop page URL](https://www.airtel.cd/airtelmoney/interopearbilite-avec-orange-money-m-pesa) (page body opaque) | High for operator API; Low for interop mechanics |
| Cross-network bridging | **Not via API** | Bridging Airtel Money ↔ Orange Money ↔ M-Pesa exists at consumer level (URL on airtel.cd advertises this) but the Airtel Africa Open API is single-operator. | [airtel.cd interop page URL](https://www.airtel.cd/airtelmoney/interopearbilite-avec-orange-money-m-pesa) | Medium |
| Balance / wallet custody (held-balance model) | Not found | n/a | n/a | Low |
| Transaction status / reconciliation | Likely yes (standard Open API pattern) — **but not confirmed from a primary DRC source** | Other Airtel Money markets expose transaction status endpoints; this is consistent for the unified portal but not DRC-verified. | Inference | Medium |
| QR / payee identification | **Not found in public docs for DRC** | n/a | n/a | Low |

## Money movement
- **Payer approval flow:** USSD/STK push with PIN approval (standard Airtel Money pattern across markets); DRC-specific verbatim text not located in public docs. **Inference based on shared platform, Medium confidence.**
- **Settlement timing:** **Not found in public docs.** **Confidence: Low.**
- **Currencies (CDF / USD):** **Both CDF and USD** — Airtel Money DRC operates in both per standard DRC mobile money practice; airtel.cd pages list services but did not return fee details via fetch. **Confidence: Medium (inference from cross-operator DRC practice).**

## Costs
- **Per-transaction fees (per leg):** The DRC tariff page exists at `https://www.airtel.cd/airtel_money/charges` but **returned only generic site headers when fetched** (JS-rendered); fees not extracted. Search snippets did not surface DRC-specific Airtel Money tariffs. **Not found — likely available via the airtel.cd site in browser or by partner contact.** **Confidence: Low.**
- **API-driven merchant fees:** **Not found** — the unified portal does not publish DRC-specific API pricing publicly. **Confidence: Low.**
- **Aggregator margin:** N/A (operator).
- **Other costs / minimums:** Not found.

## Onboarding
- **Who can sign up / requirements:** **Self-serve portal signup** is possible at `developers.airtel.africa/developer` (anyone can register an application and start exploring/testing) — broader than M-Pesa's process. **However**, "to go live," contracts are required: a dev.to article specifically notes "Why MTN and Airtel make you sign contracts before seeing their API docs" ([dev.to article — secondary, undated](https://dev.to/joova/why-mtn-and-airtel-make-you-sign-contracts-before-seeing-their-api-docs-and-how-to-start-building-3g4i)), and tech-ish describes an "after completion of onboarding and integration, they can go live" flow ([tech-ish.com](https://tech-ish.com/2021/11/19/developer-portal-airtel-money-api/)). So sandbox is open; production requires contract per country. **Confidence: Medium-High.**
- **KYC handled by provider?:** Merchant onboarding requires KYC of the business; end-user KYC rides the SIM registration.
- **Contract / commercial terms:** Required for go-live; per-country agreement; terms not public.
- **Authentication:** **OAuth 2.0** ([tech-ish.com](https://tech-ish.com/2021/11/19/developer-portal-airtel-money-api/)). **Verified fact (third-party), High confidence.**

## Reliability & track record
- **Scale / adoption (note if self-reported):**
  - **Airtel Money DRC: 1 million revenue-earning users** — **company self-reported via mobileworldlive.com news headline** ([Mobile World Live, "Airtel Money has 1M revenue-earning users in DRC"](https://www.mobileworldlive.com/money/news-money/airtel-money-1m-revenue-earning-users-drc/) — body returned 403 so date not confirmed; **Medium confidence** for the figure; date uncertain).
  - **Airtel Money group: 54.1 million customers** across all 14 markets — **company self-reported** (search summary attributing to Airtel Africa). **Medium confidence.**
  - **DRC market share among mobile money subscribers: ~31%** — third-party feasibility analysis ([Feasibility analysis of Mobile Money in eastern DRC, rdc-analyse.org, 2025-08](https://rdc-analyse.org/files/2025/08/2025_08_CAT_Feasibility-analysis-of-Mobile-Money-in-eastern-DRC_VE-1.pdf)). **Medium confidence.**
- **Known issues:**
  - airtel.cd pages are heavily JS-rendered and not amenable to programmatic content extraction; programmatic discovery of DRC-specific consumer details requires headless-browser scraping or partner outreach.

## Open questions / gaps
- DRC-specific consumer fee schedule (airtel.cd page renders only via browser).
- API per-transaction pricing for DRC (not publicly disclosed).
- Settlement cadence to partner bank account.
- Rate limits and per-transaction maximums on the DRC API.
- Exact mechanics of the airtel.cd "Interopérabilité avec Orange Money / M-Pesa" page (URL exists; cannot read body — is this a consumer-direct cross-network send, an OTP-bridging product, or aggregator-mediated?).
- Whether disbursement to a non-Airtel-Money MSISDN is supported (and if so, how the recipient receives funds).
- Time-to-production (sandbox is easy; contract signing for DRC may take weeks/months — typical, not verified).

[[fees-and-costs]] · [[payee-identification]] · [[kyc-and-onboarding]]
