# Afrimoney (Africell) — profile

- **Type:** mobile money operator
- **Operates in DRC:** **yes** — Africell DRC offers Afrimoney; relaunched on a Comviva platform in September 2020 and is one of three Afrimoney markets (DRC, Sierra Leone, Gambia, plus Angola added 2021). ([Africell story "Afrimoney adopts new mobile money industry technical specification" — 2023-12-07](https://www.africell.com/story/afrimoney-adopts-mobile-money-specification/); [africell.cd Afrimoney service page](https://www.africell.cd/afrimoney/)). **Verified fact.**
- **Last updated:** 2026-06-04
- **Overall confidence:** Medium-Low — Africell publicly states a strategic API direction (GSMA Mobile Money API Specification adoption, Comviva platform), but **no public developer portal URL, sandbox URL, or production API endpoints for DRC were located**. Most details require partner contact.

## Developer access
- **API / developer portal:** **Not found as a public, self-serve portal.** Africell announced in Dec 2023 that Afrimoney has implemented **GSMA Mobile Money API-compliant merchant APIs** ([africell.com news 2023-12-07](https://www.africell.com/story/afrimoney-adopts-mobile-money-specification/)) and stated that "Africell plans to release a developer portal and sandbox, adopt the GSMA's Mobile Money APIs, host developer bootcamps, and incubate local start-ups" (search summary citing Africell press materials). As of 2026-06-04 a public Afrimoney developer portal URL is **not found** in indexed results. **Confidence: Low — likely requires Africell partnership contact via `info@africell.cd` or local commercial team.**
- **Documentation quality:** Not assessable publicly (no portal located).
- **Sandbox / test environment:** **Stated as a future direction (Dec 2023); current public availability not found.** **Confidence: Low.**

## Capabilities (the core questions)
| Capability | Supported? | Notes | Source | Confidence |
|---|---|---|---|---|
| Collect / debit from a payer wallet | Yes (merchant APIs, GSMA-spec) | Afrimoney has launched merchant APIs for bill payments, online shopping, food delivery, ridesharing, and tax collection — collection by definition. | [africell.com 2023-12-07 announcement](https://www.africell.com/story/afrimoney-adopts-mobile-money-specification/) | Medium |
| Payout / disburse to any wallet or number | **Stated as forthcoming (Dec 2023): "expanding to disbursements and international money transfers APIs… for governments and NGOs to remit funds."** Current public availability: **not found**. A 2020 example confirms that Afrimoney DRC operationally executed disbursements (World Bank / DRC Social Fund COVID-19 humanitarian transfers in Kinshasa) but via a bespoke/private integration, not a public API. | [africell.com 2023-12-07 announcement](https://www.africell.com/story/afrimoney-adopts-mobile-money-specification/) | Low–Medium |
| Supported operators (which networks) | Afrimoney only (Africell DRC) | Operator API. | inference | High |
| Cross-network bridging | **Not via Afrimoney's own API.** | Inference; Africell's mobile money market share is <1% so it's typically the **target** in third-party aggregator bridging, not a bridge itself. | inference | Medium |
| Balance / wallet custody (held-balance model) | Not found | n/a | n/a | Low |
| Transaction status / reconciliation | Implied by GSMA spec adoption | The GSMA Mobile Money API spec includes status/query endpoints; therefore Afrimoney's merchant API likely exposes them. **Inference**, Medium confidence. | [GSMA Mobile Money API on github.com/Mobile-Money-API](https://github.com/Mobile-Money-API) (spec reference, secondary) | Medium |
| QR / payee identification | Not confirmed in DRC public docs | n/a | n/a | Low |

## Money movement
- **Payer approval flow:** Standard USSD-based (registration via `*1020#`). PIN required for transactions ([africell.cd Afrimoney service page](https://www.africell.cd/afrimoney/)). The merchant API flow likely triggers a USSD/STK push confirmation to the customer; **DRC-specific verbatim UX not located in public docs**. **Inference, Medium confidence.**
- **Settlement timing:** **Not found.** **Confidence: Low.**
- **Currencies (CDF / USD):** **Both CDF and USD** — P2P USD transfers $0–$2,500; CDF transfers 1,000–5,100,000 CDF ([africell.cd Afrimoney service page](https://www.africell.cd/afrimoney/)). **Verified fact, High confidence.**

## Costs
- **Per-transaction fees (per leg):** "Afrimoney fees are automatically deducted from your account for chargeable transactions, and current rates are displayed at all agent and partner points" — i.e., **public consumer-tariff schedule not extractable** ([africell.cd Afrimoney service page](https://www.africell.cd/afrimoney/); search summary of cd.afrimoney pages). Cash withdrawal pages exist (`/afrimoney_services/retirer-du-cash/`) but body content was not fully extracted. **Not found — Confidence: Low.**
- **API-driven merchant fees:** **Not found in public materials.** **Confidence: Low.**
- **Aggregator margin:** N/A (operator).
- **Other costs / minimums:** P2P caps disclosed above ($2,500 USD / 5,100,000 CDF per transfer). Registration is free.

## Onboarding
- **Who can sign up / requirements:**
  - **End-user wallet:** Africell subscriber + valid national ID or passport + active SIM. Dial `*1020#` or visit shop. Free ([africell.cd Afrimoney service page](https://www.africell.cd/afrimoney/)).
  - **Merchant / business API:** No public self-serve flow located. Contact `info@africell.cd` or call `1010`. **Confidence: Low — requires direct outreach.**
- **KYC handled by provider?:** Yes for end-users (national ID / passport, SIM-tied). Merchant KYC via standard Africell commercial onboarding.
- **Contract / commercial terms:** Not public. Likely Africell Master Services Agreement + merchant terms.

## Reliability & track record
- **Scale / adoption (note if self-reported):**
  - **Africell DRC subscribers (telecom): 5M+ active** (since 2012 launch) — **company self-reported via Africell press materials**.
  - **Africell group (all markets): 16M+ subscribers** as of Jan 2026 — **company self-reported** ([Africell Wikipedia entry — secondary, citing Africell figures](https://en.wikipedia.org/wiki/Africell)).
  - **DRC TELECOM market share: 20–25% in provinces it operates in; joint market leader in Kinshasa, Kongo Central, Haut-Katanga** — **company self-reported** (Africell press). **Distinguish from DRC mobile money market share**, where Afrimoney is far smaller (see next bullet).
  - **DRC MOBILE MONEY market share: <1%** (Vodacom 51%, Airtel 31%, Orange 17%) — third-party feasibility analysis ([Feasibility analysis of Mobile Money in eastern DRC, rdc-analyse.org, 2025-08](https://rdc-analyse.org/files/2025/08/2025_08_CAT_Feasibility-analysis-of-Mobile-Money-in-eastern-DRC_VE-1.pdf)). **Medium confidence.**
- **Coverage:** Africell DRC's 2G/3G/4G is concentrated in Kinshasa, Kongo-Central, and Haut-Katanga ([Africell DRC market page](https://www.africell.com/market/drc/)). For a payments MVP this matters: outside these provinces, Afrimoney reach is materially weaker.
- **Track record items of note:**
  - 2020 collaboration with **World Bank** and DRC Social Fund to disburse "millions of dollars" in COVID-19 humanitarian aid in Kinshasa — **operationally demonstrates large-scale disbursement capacity** ([africell.com 2023-12-07 announcement](https://www.africell.com/story/afrimoney-adopts-mobile-money-specification/)).
  - Platform migration to **Comviva** mobile money platform (industry standard); GSMA Mobile Money API spec adoption (Dec 2023) signals an open-API trajectory.

## Open questions / gaps
- **Public Afrimoney developer portal URL** (likely does not exist publicly yet; Dec 2023 announcement was forward-looking).
- **Sandbox URL**, authentication model (OAuth? API key?), and endpoint base URL for DRC.
- **Consumer fee schedule** for transfers, withdrawals, deposits, merchant payments (rates "displayed at agent points" but not extractable from public web).
- **API-driven merchant fees**.
- Whether the **GSMA-spec merchant API is live in DRC specifically** (Dec 2023 announcement is brand-wide; rollout per market not confirmed).
- **Settlement timing** for merchant funds.
- **Time-to-production** for a new partner integration in DRC.
- **Cross-checks of <1% mobile money share** — is this customer-count share, value share, or transactions? Important for sizing reachability.
- Whether disbursement via API can target a **non-Afrimoney Africell number** (likely no — wallet must be provisioned first, but unverified).

[[fees-and-costs]] · [[payee-identification]] · [[kyc-and-onboarding]]
