# KYC & onboarding (cross-cutting)

- **Topic / entity:** identity requirements to onboard a DRC mobile-money / fintech app user, and how the MVP can satisfy them
- **Question it addresses:** How can KYC ride existing operator/SIM registration? What do aggregators provide? What's the practical friction for an urban smartphone payer?
- **Date researched:** 2026-06-04
- **Overall confidence:** Medium-High for the regulatory regime and tiered limits; Medium for aggregator KYC-as-a-service offerings; Low for vendor-specific KYC SLAs.

## Bottom line
For the **payer** in DRC, KYC is primarily met by their existing SIM/operator registration: every DRC mobile money wallet is bound to a KYC'd SIM, and the regime tiers wallets by limit. For the **app itself**, the BCC (Banque Centrale du Congo) framework requires KYC on customers above $100 (and full KYC above $500) under BCC Instruction No. 15 (2018), with two-tier CDD. Aggregators (pawaPay, MOKO, Onafriq, DPO) do **merchant** KYC on the integrating business, not on end-users — the end-user KYC remains on the operator side. **Critical flag:** if our MVP holds an end-user balance (rather than passing through), we likely become an EMI under BCC framework and inherit direct KYC obligations. **Legal/licensing requires investigation** (per `00-overview/research-goals.md`).

---

## Regulatory framework

- **Banque Centrale du Congo (BCC)** is the supervising authority for banks, **Electronic Money Institutions (EMIs)**, **Payment Service Providers (PSPs)**, and foreign exchange.
- **Directive #24 (December 2011)** — original e-money framework that opened the DRC market to non-bank issuers; within months, four MNOs were licensed and three launched ([findevgateway DRC mobile money policies, 2014](https://www.findevgateway.org/sites/default/files/publications/files/mfg-en-case-study-enabling-mobile-money-policies-in-the-democratic-republic-of-congo-mar-2014.pdf)). **Verified, High confidence.**
- **BCC Instruction No. 15 (2018)** — current KYC, customer-identification, risk-based due-diligence, and record-keeping rules for banks, microfinance, mobile money providers. ([voveid blog summary, 2026 guide](https://blog.voveid.com/kyc-compliance-in-the-drc-2025-guide-for-regulated-businesses/) — **secondary, Medium-High confidence**).
- **Law No. 04/016 of 19 July 2004** — DRC's AML/CFT base law mandates KYC for all financial institutions. (Some PayAtlas materials reference a **Law No. 18/004 of 2018** on AML as the more recent operative law — flagged conflict; verify against BCC primary sources before final use.)
- **CENAREF / CENTIF** — DRC's Financial Intelligence Unit, processes suspicious transaction reports within 24 hours of detection ([voveid blog](https://blog.voveid.com/kyc-compliance-in-the-drc-2025-guide-for-regulated-businesses/) — **secondary, Medium confidence**; sources cite both "CENAREF" and "CENTIF" — flagged for verification).
- **Data protection:** Law No. 18/023 of 2018 — basic data-protection principles, oversight by CNPDCP ([payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd) — **secondary, Medium confidence**).

## End-user KYC tiers (mobile money)

| Tier | Account limit | Daily limit | Monthly limit | KYC level |
|---|---|---|---|---|
| Tier 1 (Standard) | Up to **US$ 100** (or operator-set lower cap below the legal $500) | Up to **US$ 100** | Up to **US$ 2,500** | Light CDD — SIM registration + basic ID |
| Tier 2 (Premium / Enhanced) | Up to **US$ 3,000** | Up to **US$ 500** | Up to **US$ 2,500** | Full CDD — verified ID document |

- M-Pesa DRC consumer caps confirm this: Standard $100 (or 100,000 FC), Premium $3,000 (or 3,000,000 FC) — **verified for Vodacom M-Pesa DRC** (search summary of Vodacom CD tariff page); Airtel/Orange/Afrimoney follow the same regulatory ceiling. **Confidence: Medium-High** for the structure; **Medium** for the exact $500/day Tier 2 figure.
- Sources: [voveid KYC DRC guide](https://blog.voveid.com/kyc-compliance-in-the-drc-2025-guide-for-regulated-businesses/); [vodacom.cd M-Pesa caps](https://www.vodacom.cd/particulier/m-pesa/combien-ca-coute) via search summary.

## Accepted identity documents

- **National ID card**, **passport**, **driver's licence**, **voter card**, **student card**, **service card** (specifically for SIM registration; mobile-money KYC typically accepts the same set plus regional ID variants). **Verified** ([voveid](https://blog.voveid.com/kyc-compliance-in-the-drc-2025-guide-for-regulated-businesses/)).
- **SIM registration is mandatory in DRC** — by extension, every active mobile money wallet sits on a SIM that has already cleared SIM-side ID. **Verified.**
- **Challenge:** **60% of the DRC population lacks official IDs; only ~10% hold national ID cards** ([voveid](https://blog.voveid.com/kyc-compliance-in-the-drc-2025-guide-for-regulated-businesses/) — **secondary, Medium confidence**). For the urban smartphone target (Kinshasa, Lubumbashi), ID coverage is materially higher than the national average, but **a meaningful share of even urban payers will hit ID-friction**.

## Merchant / app KYC obligations

- **AML triggers:** Mandatory suspicious-transaction reporting threshold around **CDF 5M (~US$ 2,000)**; enhanced due diligence applies above ~US$ 2,500 ([voveid](https://blog.voveid.com/kyc-compliance-in-the-drc-2025-guide-for-regulated-businesses/)). **Secondary, Medium confidence.**
- **Risk-based CDD** required; PEP screening; UN/OFAC sanctions screening; **10-year data retention**.
- **A foreign-incorporated MVP** without a BCC licence is typically expected to **partner with a licensed local PSP/EMI** rather than operate directly. PayAtlas summarises this as "Foreign PSPs must partner with licensed local entities or obtain BCC authorisation; independent operation is prohibited" ([payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd) — **secondary, Medium confidence**). **This is the key legal flag for the MVP** — track in 03-reports/recommendation.md and consult counsel before live launch.

## What aggregators provide (KYC-as-a-service)

| Aggregator | Merchant KYC | End-user KYC | Notes |
|---|---|---|---|
| **pawaPay** | Yes — KYC questionnaire is part of go-live onboarding ([pawaPay Plans page](https://www.pawapay.io/plans)) | No — passes through to MMO | Standard plan: self-serve + KYC review. Confidence: High. |
| **MOKO Afrika** | Yes — implied by signup flow; not itemised publicly | No — passes through to MMO | Documentation light on KYC steps. Confidence: Medium. |
| **DPO Pay** | Yes — bilateral commercial onboarding | No — passes through to MMO | Process via "Get Started" lead form; manual sales. Confidence: Medium. |
| **Onafriq** | Yes — also offers Card Issuance ([onafriq.com](https://onafriq.com/)) which includes KYC tooling. **Card Issuance product** includes ID verification, KYC checks, and AML compliance for issued cards. | No directly — but card-issuance product has end-user KYC plumbing | Confidence: Medium. |
| (3rd-party) | **Smile ID** ([usesmileid.com DRC page](https://usesmileid.com/countries/the-democratic-republic-of-congo/)) — pan-African KYC SDK with DRC ID support (national ID, passport, driver's licence, voter card). Could plug into the MVP independently of aggregator. Confidence: Medium. |

- **No aggregator we researched does end-user KYC for the wallet** — that's the operator's role. So an MVP that simply passes a payment through is mostly inheriting operator-side KYC for free.
- **An MVP that holds an end-user balance** (e.g., maintains a top-up wallet for the user that's not bound 1:1 to their operator wallet) crosses into EMI territory and needs its own KYC pipeline — Smile ID or similar plus partner-PSP licensing.

## Practical friction for the urban smartphone payer

For the target user (Kinshasa/Lubumbashi smartphone-holding adult):

1. They already have a SIM (mandatory) — so the operator-side KYC for the wallet is already in place.
2. They likely have a Tier 1 wallet by default; Tier 2 requires upgrading at an operator agent with verified ID, which adds friction outside the app.
3. **App-side onboarding can therefore be ID-light in v1** if it relies on the existing operator wallet for value transfer (pass-through model). The MVP UX is: confirm phone, confirm operator, link to the operator wallet (which they already KYC'd), pay.
4. If the MVP needs to onboard the user with its **own** ID step (e.g., for chargeback handling, loyalty, ledger custody), the lift is moderate: ~70% urban ID coverage (inferred from 10% nationally + urban skew), and Smile ID or similar can OCR/verify in-flow.

## Confidence summary
- **High:** BCC supervises the framework; SIM registration is mandatory; tiered limits exist.
- **Medium:** Specific Tier 1/Tier 2 caps cited; aggregator KYC-as-a-service is on the merchant side only; CENAREF/CENTIF naming inconsistency.
- **Low:** Whether MVP would be licensed directly as EMI or partner-mediated — **legal/licensing flag for separate investigation** per `00-overview/research-goals.md`.

## Sources
- xo-005 (PayAtlas DRC PSPs / compliance)
- xo-003 (GSMA DRC newsroom)
- mp-010 (findevgateway DRC mobile money policies 2014)
- xo-014: voveid KYC DRC guide (2026) — `https://blog.voveid.com/kyc-compliance-in-the-drc-2025-guide-for-regulated-businesses/`
- xo-015: voveid AML DRC guide (2025) — `https://blog.voveid.com/aml-compliance-in-the-democratic-republic-of-the-congo-2025-guide-for-fintechs-and-startups/`
- xo-016: Smile ID DRC page — `https://usesmileid.com/countries/the-democratic-republic-of-congo/`
- xo-017: Generis — DRC digital payments regulatory framework — `https://generisonline.com/understanding-the-regulatory-framework-for-digital-payments-and-fintech-companies-in-the-democratic-republic-of-the-congo/`
- xo-018: UNCDF Payment Infrastructure Assessment DRC (April 2025) — `https://migrantmoney.uncdf.org/wp-content/uploads/2025/05/Payment-Infrastructure-Assessment-DRC-April2025.pdf` (PDF body not extracted in this session; referenced for future read)

[[mpesa]] · [[orange-money]] · [[airtel-money]] · [[afrimoney]] · [[pawapay]]
