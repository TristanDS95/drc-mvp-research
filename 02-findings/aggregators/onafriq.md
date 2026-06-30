# Onafriq (formerly MFS Africa) — profile

- **Type:** aggregator / PSP (pan-African digital payments network)
- **Operates in DRC:** **yes — with a Kinshasa office** ([onafriq.com homepage](https://onafriq.com/)); DRC explicitly named in the September 2025 Visa Pay launch ([Onafriq press: Visa Pay launch in DRC, 2025-09-11](https://onafriq.com/press/article/onafriq-and-visa-partner-to-launch-visa-pay-unlocking-interoperability-between-card-and-mobile-money-in-the-drc)). **Verified fact, High confidence.**
- **Last updated:** 2026-06-04
- **Overall confidence:** Medium-High for capabilities at a network level; Medium for DRC-specific operator coverage; Low for pricing.

## Developer access
- **API / developer portal:** Two portals visible:
  - **Onafriq developer portal:** `https://developers.onafriq.com/` (current Onafriq-branded)
  - **MFS Africa Developer Hub:** `https://developer.mfsafrica.com/` and `https://developer.mfsafrica.com/docs/combined-api-information` (legacy branding; same network)
  - **Sandbox portal:** `https://sandbox.gtpportal.com/DeveloperPortal` (referenced by search index — accessible via Onafriq developer signup).
  - **Verified fact** (URLs exist; landing pages returned minimal content via fetch).
- **Documentation quality:** **Medium-High** — public docs cover API products, endpoints, authentication; specific DRC coverage matrix not easily extractable in this session. **Confidence: Medium.**
- **Sandbox / test environment:** **Yes** — sandbox portal exists at `sandbox.gtpportal.com`. Token-based auth. **Verified fact (URL existence), Medium confidence (access flow not fully traced).**

## Capabilities (the core questions)
| Capability | Supported? | Notes | Source | Confidence |
|---|---|---|---|---|
| Collect / debit from a payer wallet | **Yes (Payins)** — receive payments from customers. | [MFS Africa API endpoints docs](https://developer.mfsafrica.com/docs/api-endpoints) | High |
| Payout / disburse to any wallet or number | **Yes (Payouts)** — primary API for initiating customer payouts; cross-border and domestic. | [MFS Africa API endpoints docs](https://developer.mfsafrica.com/docs/api-endpoints) | High |
| Supported operators (DRC) | **M-Pesa, Airtel Money, Orange Money confirmed** for the Visa Pay launch in DRC. **Afrimoney not mentioned in DRC context** (Africell is shown on Onafriq partner logos at network level, but DRC-specific Afrimoney support is not confirmed). | [Onafriq press release, 2025-09-11](https://onafriq.com/press/article/onafriq-and-visa-partner-to-launch-visa-pay-unlocking-interoperability-between-card-and-mobile-money-in-the-drc); [Developing Telecoms reporting](https://developingtelecoms.com/telecom-technology/financial-services/19062-onafriq-and-visa-launch-visa-pay-in-drc-for-wallet-interoperability.html) | High for the three named; Low for Afrimoney inclusion |
| Cross-network bridging | **Yes (signature capability)** — Onafriq's value proposition is the cross-network/cross-border hub. The Visa Pay launch demonstrates the bridge between Visa cards and the three major DRC mobile money networks. | Onafriq press release; homepage | High |
| Balance / wallet custody (held-balance model) | **Yes (Treasury Services)** — "access to multiple currencies across the continent." | [onafriq.com homepage](https://onafriq.com/) | Medium |
| Transaction status / reconciliation | **Yes** — standard for the API set. | [MFS Africa API endpoints docs](https://developer.mfsafrica.com/docs/api-endpoints) | High |
| QR / payee identification | Card-issuance + wallet-funding-via-Visa via Visa Pay; native QR not extracted from public docs in this session. | inference | Low-Medium |

## Money movement
- **Payer approval flow:** For DRC, when initiating a collection (Payin) through Onafriq, the payer is prompted via the underlying MNO USSD/STK and enters PIN. **Inference, Medium confidence.**
- **Settlement timing:** Not extracted in this session. **Confidence: Low.**
- **Currencies (CDF / USD):** Onafriq's network supports multiple currencies including local DRC currencies. **DRC supports CDF and USD per the three operator profiles**; Onafriq forwards those. **Inference / Medium confidence.**

## Costs
- **Per-transaction fees (DRC):** **Not publicly disclosed**, requires sales contact. **Confidence: Low.**
- **Aggregator margin:** Not publicly disclosed.
- **Other costs / minimums:** Not found.

## Onboarding
- **Who can sign up / requirements:** Developer portal signup is open (sandbox); production requires commercial onboarding. Onafriq is a regulated network operator and typically prefers larger merchants and PSPs; KYC and contract required. **Inference (consistent with peer aggregators), Medium confidence.**
- **KYC handled by provider?:** Yes for merchants; end-user KYC remains MNO-side.
- **Contract / commercial terms:** Not public.
- **Authentication:** **Token-based** (API key generated from logged-in account) ([MFS Africa Combined API info — search summary](https://developer.mfsafrica.com/docs/combined-api-information); search snippet). **Verified, Medium confidence.**
- **Base URLs:**
  - **Domestic transactions:** `https://api.mfsafrica.com/api`
  - **Cross-border transactions:** `https://mfsafrica.beyonicpartners.com/api`
  - **Verified fact (third-party citation of MFS Africa docs)** ([search summary citing MFS Africa API endpoints](https://developer.mfsafrica.com/docs/api-endpoints)). **Medium confidence** — base URLs may have been updated post-Onafriq rebrand; the `.beyonicpartners.com` host suggests a Beyonic acquisition lineage.

## Reliability & track record
- **Scale / adoption (note if self-reported):**
  - **1 billion connected mobile wallets** across 43 African markets; **500 million bank accounts**; **2,000+ cross-border payment corridors**; **400,000+ agents** (primarily Nigeria via Baxi). **Company self-reported** ([onafriq.com homepage](https://onafriq.com/)). **Medium confidence.**
  - **34 African markets with live BINs (card issuance).** **Self-reported.**
  - **DRC mobile payments market context:** projected to reach **$3.85 billion in transaction value in 2025** with **19% CAGR** ([Onafriq press release citing market data, 2025-09-11](https://onafriq.com/press/article/onafriq-and-visa-partner-to-launch-visa-pay-unlocking-interoperability-between-card-and-mobile-money-in-the-drc) — third-party number cited by Onafriq). **Medium confidence.**
- **Known issues:**
  - The two-portal situation (`developers.onafriq.com` and `developer.mfsafrica.com`) reflects a still-in-progress rebrand from MFS Africa; navigating both may add integration overhead.
  - DRC operator coverage is **definitively three of four** (M-Pesa, Airtel, Orange) per the Visa Pay launch context — Afrimoney status in DRC for Onafriq is **unclear**.

## Open questions / gaps
- **Afrimoney DRC: confirmed or not?** (Onafriq lists Africell at network level, but the DRC Visa Pay launch only named the three big operators.)
- **Per-transaction fees** for DRC (collection and payout) — not public.
- **Settlement cadence** and currency options for merchants in DRC.
- **Time-to-production** — Onafriq tends to onboard PSPs/financial institutions rather than small merchants; the onboarding bar for a small startup is uncertain.
- **Current sandbox URL** (gtpportal.com vs. developers.onafriq.com vs. developer.mfsafrica.com) and the active prod URL post-rebrand.
- **Rate limits / per-transaction maximums.**
- **Regulatory licensing** posture in DRC — does Onafriq run under partner licences or hold its own BCC authorisation?
- **Visa Pay role:** Is Visa Pay a *product* Onafriq sells, or a *use case demonstrating* the underlying Payins/Payouts APIs that our app could use directly?

[[fees-and-costs]] · [[payee-identification]] · [[kyc-and-onboarding]]
