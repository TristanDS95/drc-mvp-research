# Orange Money — profile

- **Type:** mobile money operator
- **Operates in DRC:** **yes** — Orange Money is one of the four DRC mobile money networks. ([orange.cd Orange Money page](https://www.orange.cd/fr/orange-money-new.html); [developer.orange.com Orange Money Web Payment API — lists "RD Congo"](https://developer.orange.com/apis/om-webpay)). **Verified fact.**
- **Last updated:** 2026-06-04
- **Overall confidence:** Medium — public docs confirm Orange Money in DRC and a collection-only API for merchants; payout (B2C) capability and exact API fees are not publicly documented for Orange Money DRC.

## Developer access
- **API / developer portal:** `https://developer.orange.com/apis/om-webpay` — **"Orange Money Web Payment / M Payment (1.0) API"**. DRC ("RD Congo") is explicitly listed alongside Mali, Cameroon, Côte d'Ivoire, Senegal, Madagascar, Botswana, Guinea Conakry, Guinea Bissau, Sierra Leone, and Central African Republic. **Verified fact, High confidence** ([developer.orange.com Orange Money Web Payment API page](https://developer.orange.com/apis/om-webpay)). Note: this is the **only Orange Money API explicitly extending to DRC** that I located; other Orange Money API products (e.g., disbursement/B2C) are not enumerated for DRC.
- **Documentation quality:** Overview-level on the public-facing page; sandbox/auth/endpoint details require portal access. The page provides workflow steps ("Subscribe → Integrate → Accept") but not endpoint details, authentication mechanisms, sandbox URLs, or pricing in the public view. **Confidence: Medium** ([developer.orange.com om-webpay overview](https://developer.orange.com/apis/om-webpay)).
- **Sandbox / test environment:** **Not confirmed in public docs** — Orange Developer normally provides sandbox keys after portal signup, but the Orange Money Web Payment overview page does not explicitly say so. Treat as **Not found — likely requires portal signup** ([developer.orange.com om-webpay overview](https://developer.orange.com/apis/om-webpay)).

## Capabilities (the core questions)
| Capability | Supported? | Notes | Source | Confidence |
|---|---|---|---|---|
| Collect / debit from a payer wallet | Yes (Web Payment / M Payment) | "Orange Money Web Payment / M Payment service is dedicated to merchants" for accepting online payments. | [developer.orange.com om-webpay](https://developer.orange.com/apis/om-webpay) | High |
| Payout / disburse to any wallet or number | **Not found via Orange's own public API for DRC** | The om-webpay overview explicitly describes collection only. A separate Orange Money disbursement/B2C API for DRC is **not found** on developer.orange.com. Third-party providers (Klasha, Digitalpaye, Nuvei) advertise Orange Money payout coverage but these are aggregator-mediated, not Orange's own API. | [developer.orange.com om-webpay](https://developer.orange.com/apis/om-webpay); [Klasha MOMO Payout press](https://www.klasha.com/blog/klasha-launches-momo-payout-api-powering-seamless-mobile-money-disbursements-across-africa) (secondary, self-reported); [Nuvei Orange Money page](https://www.nuvei.com/apm/orange-money) (secondary, self-reported) | Medium |
| Supported operators (which networks) | Orange Money only (Orange DRC) | Operator API. Cross-network transfers at consumer level are 1% (see "Costs"), but the **API** is single-operator. | [orange.cd tariffication page](https://www.orange.cd/fr/orange-money/tarifications-orange-money.html) | High |
| Cross-network bridging | **Consumer-level "to other networks" transfer is priced at 1%; API-level bridging is not in Orange's API** | The consumer-tariff page lists "transfer to other networks" with a 1% rate — indicating off-net consumer transfer is possible (likely via on-screen prompts). Whether this is technically interop or operates via a bridging hub is **not stated**. | [orange.cd tariffication page](https://www.orange.cd/fr/orange-money/tarifications-orange-money.html) | Medium |
| Balance / wallet custody (held-balance model) | Not found | The API is for merchant collection; held-balance/ledger models are not described in public docs. | n/a | Low |
| Transaction status / reconciliation | **Not found in public docs** — most Orange Money APIs have a status-query endpoint but the om-webpay overview page does not list one publicly. | [developer.orange.com om-webpay](https://developer.orange.com/apis/om-webpay) | Low |
| QR / payee identification | Visa card linked to OM account (an Orange-Visa product is offered); QR for merchant payments in DRC is **not explicitly confirmed** | The orange.cd OM page mentions a Visa card product. Merchant identification is **via merchant subscription/Orange Money merchant code** — i.e., the merchant becomes a registered OM merchant. | [orange.cd Orange Money page](https://www.orange.cd/fr/orange-money-new.html); [developer.orange.com om-webpay](https://developer.orange.com/apis/om-webpay) | Medium |

## Money movement
- **Payer approval flow:** Standard Orange Money flow uses a USSD/STK prompt with the customer entering a PIN; the merchant integration via the Web Payment API expects this flow but the public API overview does not describe verbatim text or specific UX for DRC. **Inference, Medium confidence** ([developer.orange.com om-webpay](https://developer.orange.com/apis/om-webpay)).
- **Settlement timing:** **Not found in public docs** — orange.cd consumer page says "Your beneficiary can immediately use received funds" for P2P transfers ([orange.cd tariffication page](https://www.orange.cd/fr/orange-money/tarifications-orange-money.html)), but the merchant-to-bank settlement cadence for the Web Payment API is not disclosed publicly. **Confidence: Low.**
- **Currencies (CDF / USD):** **Both CDF and USD** — tariff schedule lists both currencies side by side. **Verified fact, High confidence** ([orange.cd tariffication page](https://www.orange.cd/fr/orange-money/tarifications-orange-money.html)).

## Costs
- **Per-transaction fees (per leg) — consumer tariff:** ([orange.cd tariffication page](https://www.orange.cd/fr/orange-money/tarifications-orange-money.html))
  - **Withdrawal (national, CDF):** 9.00% (100–30,000 FC) → 6.20% (30,001–60,000) → 3.20% (60,001–150,000) → 2.00% (150,001–1,200,000) → 1.00% (1,200,001–7,500,000)
  - **Withdrawal (national, USD):** 9.00% ($0.01–10) → 6.20% ($10.01–20) → 3.20% ($20.01–50) → 2.00% ($50.01–400) → 1.00% ($400.01–2,500)
  - **Withdrawal regional:** Western region 8.50% → 1.00%; **Eastern region flat 1.00% across all tiers**
  - **Transfer to OM account (USSD/app, national):** 1.00% flat for all tiers
  - **Transfer to other networks (off-net):** 1.00% flat for all tiers
  - **Coupon withdrawal:** Free
  - Source labelling: this is the **published consumer tariff**, which is company self-reported / regulatory-approved. **Confidence: High for the figures retrieved; Medium for currency completeness** because the page may have additional sub-sections (deposit, merchant pay) that the WebFetch summary did not surface.
- **API-driven merchant fees (per collection):** **Not found** in the public Orange Money Web Payment API documentation. **Confidence: Low — requires partner contact.**
- **Aggregator margin (if applicable):** N/A (operator, not aggregator).
- **Other costs / minimums:** Tier upper bound on withdrawals is 7,500,000 FC ≈ $2,500. **Verified fact** ([orange.cd tariffication page](https://www.orange.cd/fr/orange-money/tarifications-orange-money.html)).

## Onboarding
- **Who can sign up / requirements:** **Merchants only** for the Web Payment API. The API page states the service is "dedicated to merchants" and requires "officially registered retailers (Orange Money merchants - fully KYA compliant)." Onboarding flow: (1) Subscribe to Orange Money at an Orange store in country, (2) integrate the API, (3) accept payments. Documents must comply with local legislation, and "must be approved by the relevant central bank." **Verified fact, High confidence** ([developer.orange.com om-webpay](https://developer.orange.com/apis/om-webpay)).
- **KYC handled by provider?:** Merchant KYC is part of becoming an Orange Money merchant ("KYA-compliant" — Orange's KYC variant). End-user wallet KYC is the SIM-registration / consumer KYC done at Orange agents/stores.
- **Contract / commercial terms:** Merchant agreement via local Orange entity; commercial terms not public. Contact for DRC: `Serviceclients.RDC@orange.com`; +243 840 001 777; 372, Avenue Colonel Mondjiba, Commune de Ngaliema, Kinshasa ([orange.cd contact details, retrieved via search summary](https://www.orange.cd/)).

## Reliability & track record
- **Scale / adoption (note if self-reported):**
  - **Orange Money group-wide: 17 countries, ~100 million customers** — self-reported by Orange. The 100M is **company self-reported, Medium confidence** ([Orange Money Wikipedia — secondary](https://en.wikipedia.org/wiki/Orange_Money)).
  - **DRC market share among mobile money subscribers: ~17%** — third-party feasibility analysis ([Feasibility analysis of Mobile Money in eastern DRC, rdc-analyse.org, 2025-08](https://rdc-analyse.org/files/2025/08/2025_08_CAT_Feasibility-analysis-of-Mobile-Money-in-eastern-DRC_VE-1.pdf)). **Medium confidence.**
  - Orange Money RDC is also listed in the **GSMA mobile money deployment tracker** (page returned 403 to direct fetch but is indexed at `https://www.gsma.com/mobilefordevelopment/m/orange-money-rdc/`).
- **Known issues:**
  - Public API surface for Orange Money DRC is narrow (collection only, dedicated to KYA-compliant merchants); payout via Orange's own API is not advertised for DRC.
  - Vodacom + Orange formed a joint venture in DRC for **network (telecom infrastructure)** coverage — not directly related to Orange Money interoperability ([Orange/Vodacom JV press release](https://newsroom.orange.com/orange-and-vodacom-create-a-joint-venture-to-expand-network-coverage-in-rural-areas-in-the-drc/)).

## Open questions / gaps
- Does Orange Money DRC publish a **disbursement/B2C API** at all (not found in developer.orange.com om-webpay scope; not found via general search)? — likely requires direct partner inquiry.
- API-driven per-transaction fees for merchant collection (no public tariff for the API; only consumer-side tariffs are public).
- Sandbox URL and authentication model for the Orange Money Web Payment API (not in public overview; behind portal signup).
- Whether the on-net 1% / off-net 1% consumer transfer can be initiated from the API (i.e., can a partner programmatically push a transfer from Orange Money to an Airtel/M-Pesa wallet on the user's behalf)? — likely **no via Orange's own API**, but the consumer USSD flow supports it.
- Settlement cadence to a partner bank account.
- Rate limits and per-transaction maximums for API users.

[[fees-and-costs]] · [[payee-identification]] · [[kyc-and-onboarding]]
