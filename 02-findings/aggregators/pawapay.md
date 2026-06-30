# pawaPay — profile

- **Type:** aggregator / PSP (mobile-money-only)
- **Operates in DRC:** **yes** — DRC is one of 20+ supported African markets ([pawapay.io homepage](https://pawapay.io/); [pawaPay v2 providers docs — lists VODACOM_MPESA_COD, AIRTEL_COD, ORANGE_COD](https://docs.pawapay.io/v2/docs/providers)). **Verified fact, High confidence.**
- **Last updated:** 2026-06-04
- **Overall confidence:** **High** — pawaPay is the most transparently documented aggregator I've researched: public API docs name DRC providers explicitly with currency codes, fee table is published per-country per-provider, sandbox is self-serve.

## Developer access
- **API / developer portal:** Public docs at `https://docs.pawapay.io/` and `https://docs.pawapay.io/v2/docs/providers`. Sandbox dashboard at `https://dashboard.sandbox.pawapay.io/`. **Verified fact** ([pawaPay docs](https://docs.pawapay.io/v2/docs/providers); search snippet citing sandbox dashboard URL).
- **Documentation quality:** **High** — Public v2 reference with provider list, currency support per provider (incl. decimal precision), and authentication patterns; also a "Getting Started" guide. Community SDK examples on GitHub. **Verified, High confidence.**
- **Sandbox / test environment:** **Yes, self-serve.** "You can create an account in the pawaPay sandbox in just a few minutes and start testing the API immediately" ([pawaPay docs](https://docs.pawapay.io/getting_started); [pawapay.io product overview](https://www.pawapay.io/product-overview)). **Verified fact, High confidence.**

## Capabilities (the core questions)
| Capability | Supported? | Notes | Source | Confidence |
|---|---|---|---|---|
| Collect / debit from a payer wallet | **Yes** | "Deposits" in pawaPay's vocabulary. | [pawaPay v2 providers](https://docs.pawapay.io/v2/docs/providers); [pawapay.io product payments](https://www.pawapay.io/product-payments) | High |
| Payout / disburse to any wallet or number | **Yes** | "Payouts" — disbursement of funds from pawaPay account to a customer's mobile money wallet. | [pawapay.io product payments](https://www.pawapay.io/product-payments) | High |
| Supported operators (DRC) | **Vodacom M-Pesa, Airtel, Orange** — Afrimoney is **NOT** in the v2 provider list for DRC | DRC providers: `VODACOM_MPESA_COD`, `AIRTEL_COD`, `ORANGE_COD`. **Verified fact** ([pawaPay v2 providers](https://docs.pawapay.io/v2/docs/providers)). **High confidence.** | [pawaPay v2 providers](https://docs.pawapay.io/v2/docs/providers) | High |
| Cross-network bridging | **Yes (functional)** — pawaPay collects from one operator and pays out to another via its merchant account; the bridge is at pawaPay's settlement layer, not a "direct on-net+off-net atomic swap." | inference from the deposit+payout architecture | High |
| Balance / wallet custody (held-balance model) | **Yes** — Treasury services include "local/global currency settlement, competitive FX rates"; merchant maintains a balance in their pawaPay account. | [pawaPay plans page](https://www.pawapay.io/plans) | High |
| Transaction status / reconciliation | **Yes** — centralised dashboard with real-time transaction tracking; status query in the API. | [pawaPay plans page](https://www.pawapay.io/plans); inference from API design | High |
| QR / payee identification | **Pay by mobile number / wallet ID** (provider-dependent). QR not a native pawaPay feature in public docs. | inference; [pawapay.io product overview](https://www.pawapay.io/product-overview) | Medium |

## Money movement
- **Payer approval flow:** USSD/STK push native to each operator — pawaPay initiates the prompt via the underlying MMO API; the payer sees their normal operator UX (e.g., M-Pesa PIN prompt). **Inference based on standard aggregator model; consistent with pawaPay's "one integration to many MMOs" framing.** Medium confidence.
- **Settlement timing:** Not stated explicitly in materials reviewed; pawaPay's "Treasury services" handle settlement to merchant currency. **Not found in detail — Confidence: Low for exact cadence.**
- **Currencies (CDF / USD):** **Both supported.** Vodacom M-Pesa supports CDF (no decimals) and USD (2 decimals); Airtel and Orange both support CDF and USD with 2 decimals. **Verified fact** ([pawaPay v2 providers](https://docs.pawapay.io/v2/docs/providers)). **High confidence.**

## Costs
- **Per-transaction fees (DRC, per leg):** (per published fees page, retrieved via search summary on [pawapay.io/fees](https://www.pawapay.io/fees))
  - **Airtel DRC:** Collection 3% (2% MMO + 1% pawaPay); Disbursement 2%.
  - **Orange DRC:** Collection 3% (2% MMO + 1% pawaPay); Disbursement 1%.
  - **Vodacom M-Pesa DRC:** Collection 2.5% (1.5% MMO + 1% pawaPay); Disbursement 2%.
  - All exclusive of taxes. Treat as **published, Medium-High confidence** (these are pawaPay's stated rates as a PSP; the MMO-pass-through component is what each operator charges pawaPay and may not equal the published consumer tariffs).
- **Aggregator margin (pawaPay fee):** Flat **1% on collections and disbursements**, same across all markets and MMOs (Standard plan). No setup or monthly fees on Standard. **Verified fact** ([pawapay.io plans page](https://www.pawapay.io/plans); [pawapay.io fees page](https://www.pawapay.io/fees)).
- **Enterprise plan:** Custom rates, dedicated support, premium onboarding. DRC providers do **not** require Enterprise (the Enterprise-gated set is Kenya M-Pesa, Mozambique M-Pesa & Movitel, Nigeria Airtel & MTN, Tanzania Vodacom, Benin Celtiis betting, and "Ivory Coast Orange — coming soon"). **Verified fact** ([pawapay.io plans page](https://www.pawapay.io/plans)).
- **Other costs / minimums:** No currency conversion, settlement, or bank withdrawal fees mentioned in the public docs reviewed. "Gaming/betting MMO fees differ from standard rates." Cross-border settlements unavailable in Ethiopia and Mozambique (so by inference, available elsewhere including DRC).

## Onboarding
- **Who can sign up / requirements:** **Self-serve sandbox signup** is open. Production: (1) Sign up & test in sandbox; (2) complete KYC questionnaire; (3) specify markets and use case; (4) go live post-approval ([pawapay.io plans page](https://www.pawapay.io/plans)). **Verified fact, High confidence.**
- **KYC handled by provider?:** **pawaPay runs merchant KYC** (the "KYC questionnaire" is the merchant-side). End-user wallet KYC remains with the MMO. **Verified fact, High confidence.**
- **Contract / commercial terms:** Standard plan = published rates, no setup/monthly fees. Enterprise = bilateral commercial agreement. **Verified fact.**

## Reliability & track record
- **Scale / adoption (note if self-reported):**
  - "20+ African markets" and "covers 85% of all mobile money in Africa" — **company self-reported** ([pawapay.io homepage](https://pawapay.io/)).
  - ISO 27001 certified; OWASP ASVS compliance; 24/7 payment support — **company self-reported** ([pawapay.io plans page](https://www.pawapay.io/plans)).
- **Known issues:** None surfaced in this research. Documentation quality and DRC three-operator coverage are the strongest plus signals; Afrimoney absence is the main DRC-coverage gap.

## Open questions / gaps
- **Afrimoney DRC: not supported.** This is a real gap (Africell mobile money market share <1%, but reachability to Africell users is zero through pawaPay).
- **Settlement cadence to merchant bank account** — public docs reference "settle where and when you want" but cadence (T+0, T+1, etc.) for DRC is not stated.
- **Detailed transaction limits and rate limits** for the DRC providers via pawaPay.
- **End-user UX**: precise screens shown to the payer in DRC when pawaPay-initiated collection or payout fires (assumed to be standard MMO USSD/STK; not directly verified for DRC).
- **MMO pass-through fee composition** — the "MMO portion" pawaPay quotes for DRC (1.5% to 2%) may differ from the published consumer tariffs of those operators; how pawaPay negotiates this is opaque.

[[fees-and-costs]] · [[payee-identification]] · [[kyc-and-onboarding]]
