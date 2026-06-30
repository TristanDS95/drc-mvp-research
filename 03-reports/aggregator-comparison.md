# Aggregator comparison — DRC mobile money

- **Date:** 2026-06-04
- **Author:** Claude Code (agentic research)
- **Built from:** [pawapay.md](../02-findings/aggregators/pawapay.md), [dpo.md](../02-findings/aggregators/dpo.md), [moko.md](../02-findings/aggregators/moko.md), [onafriq.md](../02-findings/aggregators/onafriq.md), [fees-and-costs.md](../02-findings/cross-cutting/fees-and-costs.md)

## Bottom line
**pawaPay is the most MVP-friendly aggregator** evaluated: open sandbox, publicly published per-leg DRC fees (3.5%–5.0% round-trip envelope), 1% own-margin, no setup or monthly fees, three of four DRC operators covered (M-Pesa, Airtel, Orange). **MOKO Afrika is the only researched aggregator that explicitly covers all four DRC operators** including Afrimoney, but its pricing is opaque and the public docs index has some gaps. **Onafriq and DPO Pay are real options** but feel like heavier, enterprise-shaped relationships — DPO is collection-first / card-first with bulk-payout files, and Onafriq targets PSPs/financial institutions. For an MVP that wants to go live fast and budget realistically, **the practical shortlist is pawaPay (lead) + MOKO Afrika (for Afrimoney coverage if it matters)**.

## Detail

### Coverage of DRC operators

| Aggregator | M-Pesa | Airtel | Orange | Afrimoney | Other DRC payment methods |
|---|---|---|---|---|---|
| **pawaPay** | ✅ (`VODACOM_MPESA_COD`) | ✅ (`AIRTEL_COD`) | ✅ (`ORANGE_COD`) | ❌ | None — mobile money only |
| **DPO Pay** | ✅ | ✅ | ✅ | ❓ (not confirmed) | Cards (Visa/Mastercard), bank transfers, mVISA QR |
| **MOKO Afrika** | ✅ (`mpesa`) | ✅ (`airtel`) | ✅ (`orange`) | ✅ (`africell`) — **only researched aggregator with explicit Afrimoney support** | Cards, POS, QR (retail) |
| **Onafriq** | ✅ | ✅ | ✅ | ❓ (Africell on network-level partner logos; DRC-specific Afrimoney not confirmed by Visa Pay launch press) | Card issuance, treasury services |

Sources: pp-002, dp-002, mk-002, on-002.

### Capabilities

| Capability | pawaPay | DPO Pay | MOKO Afrika | Onafriq |
|---|---|---|---|---|
| Real-time collection (C2B) | ✅ | ✅ | ✅ | ✅ (Payins) |
| Real-time per-transaction payout (B2C) | ✅ | Bulk file payout; per-transaction unclear | ✅ (real-time) | ✅ (Payouts) |
| Cross-network bridging (functional) | ✅ | ✅ | ✅ | ✅ (signature capability) |
| Balance / wallet custody | ✅ (Treasury) | Implied | Likely | ✅ (Treasury) |
| Transaction status / reconciliation | ✅ | ✅ | ✅ (HMAC-SHA256 callbacks) | ✅ |
| QR payments | Pay-by-number primarily | mVISA QR | Retail QR | Card-rail-mediated (Visa Pay) |
| Card issuance / additional rails | ❌ | Cards, bank | Cards, POS | ✅ Card Issuance + Agent Banking |

### Developer / onboarding posture

| Aggregator | Sandbox self-serve | Public docs | Auth | Production onboarding |
|---|---|---|---|---|
| **pawaPay** | **Yes — `dashboard.sandbox.pawapay.io`** | Excellent (v2 docs, per-provider currency/precision tables, getting-started guide) | API token | KYC questionnaire → go-live; Standard plan has no setup/monthly fees |
| **DPO Pay** | Yes — referenced in docs (URL not located) | Medium — API reference exists but JS-rendered; community SDKs (Node, PHP) | API key / v6 API | Sales lead form → bilateral commercial agreement |
| **MOKO Afrika** | Yes — referenced on homepage (URL not located) | Medium-High — auth/endpoints documented; some sub-pages 404 on this date | JWT + HMAC-SHA256 callbacks + AES-256-CBC + IP whitelist | "3 simple steps" signup; commercial terms not public |
| **Onafriq** | Yes — `sandbox.gtpportal.com/DeveloperPortal` | Medium-High — two portals (`developers.onafriq.com` + legacy `developer.mfsafrica.com`); some pages fail to fetch | Token-based | Commercial onboarding; targets PSPs / large merchants |

Sources: pp-004, pp-007, pp-008; dp-003, dp-004, dp-005; mk-001, mk-002; on-003, on-004, on-005, on-006, on-007.

### Pricing transparency

| Aggregator | Per-leg DRC fees public? | Aggregator margin | Setup / monthly |
|---|---|---|---|
| **pawaPay** | **Yes — full table** ([pawaPay Fees](https://www.pawapay.io/fees)) | 1% flat (Standard plan), same across markets | None on Standard |
| **DPO Pay** | No | Not public | Not public |
| **MOKO Afrika** | No (Tarification page not extracted) | Not public | Not public |
| **Onafriq** | No | Not public | Not public |

**pawaPay's published DRC rates** (collection / disbursement, per leg, includes MMO pass-through):

| Provider | Collection | Disbursement |
|---|---|---|
| Vodacom M-Pesa DRC | 2.5% (1.5% MMO + 1% pawaPay) | 2.0% |
| Airtel DRC | 3.0% (2.0% MMO + 1% pawaPay) | 2.0% |
| Orange DRC | 3.0% (2.0% MMO + 1% pawaPay) | 1.0% |

Round-trip cross-network costs: **3.5%–5.0%** (see [fees-and-costs.md](../02-findings/cross-cutting/fees-and-costs.md)).

### Scale and trust signals

| Aggregator | Headline claim | Reliability of claim |
|---|---|---|
| pawaPay | "20+ African markets, 85% of all mobile money in Africa" | Self-reported |
| DPO Pay | "Africa's leading PSP", owned by Network International, 20+ countries | Self-reported (Network International is a major regulated entity, lends credibility) |
| MOKO Afrika | "25M+ mobile wallets in DRC; 47M+ accessible; thousands of enterprises; PremierBet/Equity BCDC/1XBET clients" | Self-reported (some named clients are recognizable DRC entities) |
| Onafriq | "1B mobile wallets, 500M bank accounts, 43 markets, Visa Pay DRC launched 2025-09-11" | Self-reported but anchored to verifiable Visa partnership |

Sources: pp-001, pp-005; dp-001, dp-002; mk-001; on-001, on-002.

### Money movement details

- **Settlement timing:** pawaPay's "settle where and when you want" implies flexibility but cadence is not stated for DRC. DPO/MOKO/Onafriq: not public. PayAtlas's DRC PSP norm is **2–5 business days**, longer for amounts >$10,000 USD due to BCC review ([payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd)). **Confidence: Medium for the market norm; Low for vendor-specific.**
- **Currencies:** All four aggregators support both CDF and USD for DRC mobile money (pawaPay confirms in provider docs; others by reasonable inference from the underlying operator dual-currency support).
- **Approval flow:** All four delegate the payer-side approval to the underlying MNO's USSD/STK PIN prompt; there is **no smoother "approve-in-app" UX** available through any researched aggregator without operator partnership.

## Comparison / key data

| Dimension | pawaPay | DPO Pay | MOKO Afrika | Onafriq |
|---|---|---|---|---|
| DRC ops covered (of 4) | 3 | ~3 (Afrimoney unclear) | **4** | ~3 (Afrimoney unclear) |
| Public per-leg DRC pricing | ✅ | ❌ | ❌ | ❌ |
| Self-serve sandbox | ✅ | ✅ | ✅ | ✅ |
| Setup/monthly fees | None on Standard | Not public | Not public | Not public |
| Best-fit shape | Mobile-money pass-through MVP | Card-and-collection gateway | DRC-focused multi-MNO incl. Afrimoney | PSP-to-PSP hub, cards + wallet bridging |
| Time-to-live (inference) | Days–weeks (sandbox open, light KYC) | Weeks (commercial onboarding) | Days–weeks | Weeks–months (enterprise-shaped) |

## Confidence & open questions
- **Confident about:**
  - pawaPay's DRC provider list and published fees.
  - MOKO Afrika is uniquely the only researched aggregator with explicit Afrimoney support.
  - All four require merchant KYC; none does end-user KYC.
- **Uncertain / unverified:**
  - DPO Pay and Onafriq exact DRC coverage of Afrimoney.
  - Settlement cadence for each (only PayAtlas market-norm reference, no vendor-specific).
  - DRC-specific fees for DPO, MOKO, Onafriq (sales-contact required).
  - Whether pawaPay will add Afrimoney coverage (no public roadmap signal located).
- **Still missing (and how to get it):**
  - Sales-quoted DRC rate cards from DPO, MOKO, Onafriq.
  - Time-to-production benchmarks per aggregator.
  - Per-transaction limits and rate limits on the DRC product surface.
  - Vendor stance on a non-DRC-incorporated MVP (partner-licensing arrangement).

## Sources
By ID: pp-001..pp-008; dp-001..dp-007; mk-001..mk-005; on-001..on-007; xo-001, xo-002, xo-005.
