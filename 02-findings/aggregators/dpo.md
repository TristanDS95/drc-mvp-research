# DPO Pay (DPO Group) — profile

- **Type:** aggregator / PSP (multi-channel: cards + mobile money + bank transfers + QR)
- **Operates in DRC:** **yes** — DPO Group has a dedicated DRC payment-gateway page and a Kinshasa support team ([DPO Group DRC online payments page](https://dpogroup.com/online-payments/drc/) — page body returned a binary asset to fetch so coverage details came via search summary). **Verified fact** (presence confirmed by multiple sources); **Medium confidence** on the exact DRC operator coverage.
- **Last updated:** 2026-06-04
- **Overall confidence:** Medium — DPO Pay is a real, scaled African PSP with clear DRC presence, but the public docs are gateway-shaped (card-first / collection-first) and do not enumerate DRC MNOs and per-leg fees with the granularity pawaPay does.

## Developer access
- **API / developer portal:** `https://docs.dpopay.com/` (and `https://docs.dpopay.com/api/index.html`). DPO Pay API v6 and v7. Sandbox & Test Credentials documented; Postman Collections published. ([docs.dpopay.com landing](https://docs.dpopay.com/dpo-pay-by-network); [docs.dpopay.com llms.txt](https://docs.dpopay.com/llms.txt)). **Verified fact, High confidence.**
- **Documentation quality:** **Medium.** Has API reference, plugins, Postman, sandbox. The reference pages did not fully render via fetch (JS-rendered); content recovered via the `llms.txt` summary index ([docs.dpopay.com llms.txt](https://docs.dpopay.com/llms.txt)). Community-maintained SDKs exist for Node.js and PHP.
- **Sandbox / test environment:** **Yes** — referenced as "Sandbox & Test Credentials" in the documentation table of contents ([docs.dpopay.com](https://docs.dpopay.com/dpo-pay-by-network)). Exact sandbox URL **not found** in the fetched index. **Confidence: Medium.**

## Capabilities (the core questions)
| Capability | Supported? | Notes | Source | Confidence |
|---|---|---|---|---|
| Collect / debit from a payer wallet | **Yes** — collections via card, mobile money, bank transfer, mVISA QR. M-Pesa, Airtel Money, Orange Money explicitly referenced in DPO Pay docs. | [docs.dpopay.com llms.txt](https://docs.dpopay.com/llms.txt); [dpogroup.com payment methods](https://dpogroup.com/payment-methods/) | High |
| Payout / disburse to any wallet or number | **Yes (bulk payouts)** — "DPO Pay allows customers to submit a file of bulk payments in one go." Real-time single-wallet-payout API not separately documented in public reference. | [dpogroup.com](https://dpogroup.com/) (search summary) | Medium |
| Supported operators (DRC) | **M-Pesa, Airtel Money, Orange Money** for the DRC payment gateway (search summary; the dedicated DRC page returned a binary hero asset to fetch). **Afrimoney NOT confirmed.** | [DPO Group DRC online payments page](https://dpogroup.com/online-payments/drc/) (page body opaque to fetch — referenced by search index); [payatlas.com PSPs in DRC](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd) | Medium |
| Cross-network bridging | **Yes (functional)** — same model as pawaPay: DPO collects from any source it supports and pays out from its account; bridging happens at settlement layer. | inference from architecture | Medium |
| Balance / wallet custody (held-balance model) | **Likely yes** (merchant maintains balance with DPO; settlement to bank). | inference | Medium |
| Transaction status / reconciliation | **Yes** — query/refund endpoints, dashboard, reconciliation features. | [docs.dpopay.com llms.txt](https://docs.dpopay.com/llms.txt) | High |
| QR / payee identification | **Yes — mVISA QR codes supported** by DPO Pay's gateway. | [docs.dpopay.com llms.txt](https://docs.dpopay.com/llms.txt) | High |

## Money movement
- **Payer approval flow:** For mobile-money collections, payer is prompted via their MNO's USSD/STK push and enters PIN (standard operator-driven flow); DPO Pay sits as initiator. **Inference, Medium confidence.**
- **Settlement timing:** Not found in public docs reviewed. Industry baseline in DRC for PSPs is **2–5 business days**, with extended delays for amounts >$10,000 USD due to BCC reviews ([payatlas.com PSPs in DRC](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd) — **secondary, Medium confidence**). DPO-specific cadence **not confirmed**.
- **Currencies (CDF / USD):** **CDF, USD, GBP, "or any other currency"** — DPO Pay positions itself as a multi-currency gateway in DRC. **Verified by [dpogroup.com homepage search summary](https://dpogroup.com/) and the search snippet quoting DPO Pay's DRC pages.** High confidence on multi-currency support.

## Costs
- **Per-transaction fees (DRC):** **Not published.** The public DPO Group pages don't list per-operator transaction fees for DRC. PayAtlas summary cites general DRC PSP norms ("2.5–4.0% for card payments; 1,000–3,000 CDF per payout; 2–5% FX markup; ~5,000–10,000 CDF chargeback fee") but those are **market-wide PSP norms, not DPO-specific** ([payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd)). **Confidence: Low for DPO-specific.**
- **Aggregator margin:** Not publicly disclosed. Requires sales contact.
- **Other costs / minimums:** Not found.

## Onboarding
- **Who can sign up / requirements:** "Get Started" lead form on dpogroup.com leads to commercial sales process. KYC + merchant agreement; based on payatlas DRC PSP norms, expect: company registration, UBO ID, proof of address (3-month utility bill), notarised documents, French translations of foreign documents ([payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd) — DRC-wide, not DPO-specific). **Confidence: Medium.**
- **KYC handled by provider?:** Merchant KYC handled by DPO. End-user KYC remains MNO/SIM-side. **Inference, Medium confidence.**
- **Contract / commercial terms:** Bilateral merchant agreement; terms not public. Note DRC's PSP licensing regime: foreign PSPs need to partner with a licensed local entity or obtain BCC authorisation — DPO Group is registered as a regional PSP, so this is **typically handled on their side as part of their DRC offering**.

## Reliability & track record
- **Scale / adoption (note if self-reported):**
  - DPO Pay positioned as "Africa's leading PSP" — **company self-reported** ([dpogroup.com homepage](https://dpogroup.com/)).
  - **Owned by Network International** (UAE-based payment services group; cited as "Africa's leading Payment Service Provider" — group claim). **Verified fact** ([dpogroup.com footer references and naming as "DPO Pay by Network"](https://dpogroup.com/)). **High confidence.**
  - DPO operates across "20+ African countries" ([docs.dpopay.com llms.txt](https://docs.dpopay.com/llms.txt)). **Self-reported, Medium confidence.**
- **Known issues:**
  - Public DRC-specific operator coverage and fee tables are not as transparently published as pawaPay's.
  - DPO Pay is **collection-first / gateway-shaped**: payout API is bulk-file-driven rather than a per-transaction real-time disbursement API in the public-doc sense; this matters for an MVP doing 1:1 merchant payouts.

## Open questions / gaps
- DRC MNO list — is Afrimoney supported at all? (Likely not, but unconfirmed.)
- Real-time payout API granularity for DRC mobile money (vs bulk file).
- DPO-specific fee schedule for DRC mobile money collection and payout.
- Sandbox URL and exact authentication scheme.
- Settlement cadence to a DRC merchant bank account and to a foreign account.
- Whether DPO maintains a BCC-issued licence in its own name or partners locally.
- Time-to-production (commercial onboarding only — no public time estimate).

[[fees-and-costs]] · [[payee-identification]] · [[kyc-and-onboarding]]
