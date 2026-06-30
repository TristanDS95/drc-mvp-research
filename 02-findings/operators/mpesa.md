# M-Pesa (Vodacom) — profile

- **Type:** mobile money operator
- **Operates in DRC:** **yes** — Vodacom (Vodacash subsidiary) has run M-Pesa in DRC since 2012 ([Vodacom Group / M-Pesa Wikipedia entry](https://en.wikipedia.org/wiki/M-Pesa); [Mobile Europe article on M-Pesa Rallonge, 2022-11-25](https://www.mobileeurope.co.uk/vodacom-launches-m-pesa-rallonge-in-democratic-republic-of-congo/)).
- **Last updated:** 2026-06-04
- **Overall confidence:** Medium — developer portal is unified across M-Pesa markets and DRC business onboarding is documented; DRC-specific API endpoint documentation, exact API per-leg fees, and rate limits are gated behind the partner onboarding process and not publicly disclosed.

## Developer access
- **API / developer portal:** `https://openapiportal.m-pesa.com` (the same portal serves all Vodacom-operated M-Pesa markets; signup at `https://openapiportal.m-pesa.com/sign-up/organisation`, login at `.../login?userType=org`). Business info hub: `https://business.m-pesa.com/developers/`. DRC-specific landing: `https://business.m-pesa.com/vodacom-drc/onboarding-your-business-in-drc/`. **Verified fact** ([business.m-pesa.com DRC onboarding page](https://business.m-pesa.com/vodacom-drc/onboarding-your-business-in-drc/); [business.m-pesa.com Developers page](https://business.m-pesa.com/developers/)).
- **Documentation quality:** Portal-mediated. The public "Developers" hub describes the workflow (sign up → sandbox → "Go Live!") and lists features (Application Management, Rapid Integration, Sandbox Testing, Developer/Organisation Linking, Push for Review Workflow). It also lists code samples (Python, a `portal-sdk.jar` for Java) ([business.m-pesa.com Developers page](https://business.m-pesa.com/developers/)). The full API reference is behind portal authentication and was not inspected. **Confidence: Medium.**
- **Sandbox / test environment:** **yes** — explicitly described: "developer will create and test the product in our sandbox environment before releasing it" ([business.m-pesa.com DRC onboarding page](https://business.m-pesa.com/vodacom-drc/onboarding-your-business-in-drc/)). **Verified fact, High confidence.**

## Capabilities (the core questions)
| Capability | Supported? | Notes | Source | Confidence |
|---|---|---|---|---|
| Collect / debit from a payer wallet | Yes (C2B) | The portal exposes C2B as one of three initial products ("Customer to Business" — collection). | [business.m-pesa.com Developers](https://business.m-pesa.com/developers/) | High |
| Payout / disburse to any wallet or number | Yes, to M-Pesa accounts | B2C Funds Disbursement is offered as a product ("typically used for disbursement of salaries and charity pay-outs"). Recipients are described as "individual M-Pesa customers" — i.e., payouts go to M-Pesa wallets on-net. Whether the API rejects an unregistered number or auto-creates a wallet is **not found** in the public docs. | [business.m-pesa.com B2C product page](https://business.m-pesa.com/products/b2c-funds-disbursement/); [business.m-pesa.com Products](https://business.m-pesa.com/products/) | Medium |
| Supported operators (which networks) | M-Pesa only (Vodacom DRC) | The API is for the Vodacom M-Pesa network. The DRC central bank has historically not mandated cross-network interoperability (see "Notes" below), so off-net payouts via this API are **not supported** unless bridged via an aggregator. | inference; [Onafriq + Visa DRC interop news, Developing Telecoms](https://developingtelecoms.com/telecom-technology/financial-services/19062-onafriq-and-visa-launch-visa-pay-in-drc-for-wallet-interoperability.html) | Medium |
| Cross-network bridging | **No (operator API alone)** | M-Pesa's own API does not bridge to Airtel/Orange/Afrimoney; bridging is an aggregator job. | inference from product scope | Medium |
| Balance / wallet custody (held-balance model) | Not found | The B2C product moves funds out of an organisation account; whether the API supports a held-balance / ledger-for-end-users model is not described publicly. | n/a | Low |
| Transaction status / reconciliation | Yes | "Transaction Status Query" is one of the three initial portal products. Reversals API also exposed. | [business.m-pesa.com Developers](https://business.m-pesa.com/developers/) | High |
| QR / payee identification | Merchant short code; QR not confirmed in DRC API | The DRC onboarding doc says the organisation is issued "a 6-digit or 7-digit code used for customers or businesses to send money to you" — i.e., a paybill/till-style merchant code. QR support exists in M-Pesa consumer flows in other markets but DRC-specific API support is **not found**. | [business.m-pesa.com DRC onboarding page](https://business.m-pesa.com/vodacom-drc/onboarding-your-business-in-drc/) | Medium for short code, Low for QR |

## Money movement
- **Payer approval flow:** STK push / USSD push to the SIM toolkit — the customer sees a prompt with the business name, amount, and reference, and enters their M-Pesa PIN to authorise. This is **verified for the Kenya implementation** ([Lipa Na M-Pesa Online / Daraja STK push, secondary](https://dev.to/msnmongare/m-pesa-express-stk-push-api-guide-40a2)); **the DRC implementation is inferred to be the same UX** because it uses the same M-Pesa Open API platform, but the DRC-specific flow text was not located in public docs. **Confidence: Medium (inference based on shared platform).**
- **Settlement timing:** **Not found in public docs.** The onboarding doc says "the M-Pesa Business team will review the application and inform you of the M-Pesa charging model" without disclosing settlement terms ([business.m-pesa.com DRC onboarding page](https://business.m-pesa.com/vodacom-drc/onboarding-your-business-in-drc/)). **Confidence: Low.**
- **Currencies (CDF / USD):** **Both CDF (Congolese franc, locally "FC") and USD.** Vodacom DRC publishes tariff schedules in both currencies, and M-Pesa DRC accounts hold both. ([Vodacom CD M-Pesa tariffs page](https://www.vodacom.cd/particulier/m-pesa/combien-ca-coute) — landing page reachable but detailed fee table not extractable via fetch; tariff details retrieved via web search summary). **Confidence: High** for dual-currency support; **Medium** for the exact tariff figures below.

## Costs
- **Per-transaction fees (per leg):** Public DRC tariff data summarised by web search of the Vodacom CD pages:
  - **CDF**: P2P transfers free for 1,000–500,000 FC; deposits free; withdrawals percentage-based — 13.50% for 1,000–5,000 FC declining to ~0.63% above 401,000 FC.
  - **USD**: P2P transfers free for $1–$500; withdrawal fees in tiers (e.g., $0.45 for $1–$10; $5.36 for $301–$400); withdrawals as percentage range 18.00% for $1–$5 declining to 1.50% above $100.
  - **Source**: search summary of [vodacom.cd particulier tariffs](https://www.vodacom.cd/particulier/m-pesa/combien-ca-coute) and [vodacom.cd entreprise tariffs](https://www.vodacom.cd/entreprise/m-pesa/combien-ca-coute). The Vodacom CD pages themselves returned only headers via WebFetch (likely client-rendered), so these figures are **company-self-reported, retrieved via search snippets, Medium confidence**. **The fees a business pays for an API-driven C2B collection or a B2C payout are not the consumer tariff** and are **not publicly disclosed** ("M-Pesa Business team will review the application and inform you of the M-Pesa charging model" — [business.m-pesa.com DRC onboarding page](https://business.m-pesa.com/vodacom-drc/onboarding-your-business-in-drc/)). **Confidence on the API-driven fee: Low — not found, requires partner contact.**
- **Aggregator margin (if applicable):** N/A (operator, not aggregator).
- **Other costs / minimums:** Account balance caps: Standard M-Pesa max $100 (or 100,000 FC); Premium max $3,000 (or 3,000,000 FC). Tariff grid may be modified without notice. Source: same as fees. **Company self-reported, Medium confidence.**

## Onboarding
- **Who can sign up / requirements:** Organisations contact `supportmpesa@m-pesa.cd` to start. Required: complete KYC, sign the M-Pesa Services Agreement, provide bank account details, then receive a 6- or 7-digit merchant code ([business.m-pesa.com DRC onboarding page](https://business.m-pesa.com/vodacom-drc/onboarding-your-business-in-drc/)). The developer signup on the portal is separate and lighter (email + phone confirmation) and is for building/testing in sandbox; production access requires the organisation onboarding. **Verified fact, High confidence.**
- **KYC handled by provider?:** The merchant onboarding requires KYC of the business. End-user wallet KYC is the operator's own SIM-registration process, which Vodacom runs separately. **Confidence: High.**
- **Contract / commercial terms:** M-Pesa Services Agreement (terms not public). Charging model communicated after review. **Verified that contract exists; terms not public.**

## Reliability & track record
- **Scale / adoption (note if self-reported):**
  - 6 million M-Pesa customers in DRC as of 2022 — **company self-reported** by Vodacash MD Hashim Mukudi ([Mobile Europe / Vodacom announcement, 2022-11-25](https://www.mobileeurope.co.uk/vodacom-launches-m-pesa-rallonge-in-democratic-republic-of-congo/)).
  - 8.5M active users, 46,000+ merchants — figure repeated in DRC mobile money sector summaries (search snippet, likely Vodacom-sourced press); treat as **company self-reported, Medium confidence**.
  - Market share among DRC mobile money subscribers: ~51% (Vodacom M-Pesa), Airtel ~31%, Orange ~17%, Afrimoney <1% — figure from a third-party feasibility study; cross-check against GSMA before relying on it. **Treat as third-party, Medium confidence** ([Feasibility analysis of Mobile Money in eastern DRC, rdc-analyse.org, 2025-08](https://rdc-analyse.org/files/2025/08/2025_08_CAT_Feasibility-analysis-of-Mobile-Money-in-eastern-DRC_VE-1.pdf)).
- **Known issues:**
  - DRC is a less-documented M-Pesa market than Kenya/Tanzania; public technical docs for DRC are sparse.
  - Multiple Vodacom CD subpages (`/particulier/m-pesa/service-financier/les-apis-mpesa`, `/particulier/m-pesa/combien-ca-coute`, `/entreprise/m-pesa/combien-ca-coute`) returned only "Vodacom RDC" headers when fetched programmatically (heavy client-side rendering); content reachable through Google's indexing.
  - DRC central bank position has historically been **not** to mandate cross-network interoperability ([findevgateway DRC mobile money case study, 2014](https://www.findevgateway.org/sites/default/files/publications/files/mfg-en-case-study-enabling-mobile-money-policies-in-the-democratic-republic-of-congo-mar-2014.pdf) — **dated**; verify against current BCC docs).

## Open questions / gaps
- Exact API-driven per-transaction fees for C2B collection and B2C payout in DRC (not public; "will be communicated after review").
- Whether B2C payout can target a non-M-Pesa-registered Vodacom number, and what happens if so (e.g., auto-wallet provisioning).
- Settlement timing for collected funds to a partner's bank account.
- Rate limits and per-transaction maximums for API users (consumer wallet caps above are documented; API/partner caps are not).
- DRC-specific QR support in the API (Kenya/Tanzania M-Pesa has QR; DRC API surface for QR is not documented publicly).
- USSD/STK push UX verbatim text and language options for DRC (French/Lingala/Swahili/etc.) — not documented publicly.
- Whether the M-Pesa Open API portal lists DRC explicitly as a selectable market at sandbox-creation time (the public Developers page does not say; the DRC-specific onboarding flow on `business.m-pesa.com/vodacom-drc/` implies yes).
- Current regulatory position on interoperability (BCC 2014 position cited; need 2025/2026 view).

[[fees-and-costs]] · [[payee-identification]] · [[kyc-and-onboarding]]
