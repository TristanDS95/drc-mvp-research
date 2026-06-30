# Operator API comparison — DRC mobile money

- **Date:** 2026-06-04
- **Author:** Claude Code (agentic research)
- **Built from:** [mpesa.md](../02-findings/operators/mpesa.md), [orange-money.md](../02-findings/operators/orange-money.md), [airtel-money.md](../02-findings/operators/airtel-money.md), [afrimoney.md](../02-findings/operators/afrimoney.md), [fees-and-costs.md](../02-findings/cross-cutting/fees-and-costs.md)

## Bottom line
Of the four DRC mobile money operators, **Airtel Money has the most developer-friendly and publicly documented API surface** (self-serve sandbox at `developers.airtel.africa`, OAuth 2.0, both collection and disbursement live across all Airtel Africa markets including DRC). **M-Pesa is fully API-capable but partner-gated** (sandbox requires sign-up; production via `business.m-pesa.com` and Vodacom DRC commercial onboarding). **Orange Money DRC is collection-only** via its Web Payment API on `developer.orange.com`, with no public B2C/disbursement API for DRC. **Afrimoney has the smallest reach and least public API surface** (no developer portal located; GSMA-spec merchant APIs were *announced* in Dec 2023). **Direct operator integration is not the MVP path** — at minimum, four parallel contracts and per-leg fees that the operators won't even disclose without a partnership conversation — but Airtel and M-Pesa are the most viable if direct integration becomes a v2 ambition.

## Detail

### Coverage and capabilities

| Operator | API portal | Sandbox (self-serve) | Collection (C2B) | Disbursement (B2C) | Transaction status | QR (DRC) | Auth | Currencies |
|---|---|---|---|---|---|---|---|---|
| **M-Pesa (Vodacom)** | `openapiportal.m-pesa.com` (unified); DRC at `business.m-pesa.com/vodacom-drc/` | Yes (after signup) | **Yes (C2B)** | **Yes (B2C to M-Pesa wallets)** | Yes (Transaction Status Query) | Merchant short code (6/7-digit); QR not confirmed for DRC | API key | CDF + USD |
| **Orange Money** | `developer.orange.com/apis/om-webpay` | Not confirmed in public docs | **Yes (Web Payment for merchants)** | **Not found** — no public DRC B2C API | Not explicitly documented (likely) | Merchant subscription (KYA); Visa-linked card; QR not confirmed | Not public | CDF + USD |
| **Airtel Money** | `developers.airtel.africa` (unified across 14 markets) | **Yes — self-serve** (sandbox `openapiuat.airtel.africa`, prod `openapi.airtel.africa`) | **Yes (Collection)** | **Yes (Disbursement)** | Yes (standard pattern) | Not confirmed for DRC | **OAuth 2.0** | CDF + USD (inferred) |
| **Afrimoney (Africell)** | None public; Dec 2023 announcement of forthcoming dev portal | **Not located** | Yes (GSMA-spec merchant APIs announced Dec 2023; DRC rollout unconfirmed) | **Forthcoming** (Dec 2023 announcement; not live publicly) | Implied by GSMA spec | Not confirmed | Not public | CDF + USD |

Sources: mp-001, mp-002, mp-003; om-001; am-001, am-002; af-002, af-003.

### Reach (mobile money subscribers)

| Operator | DRC mobile-money users / share | Date / source |
|---|---|---|
| Vodacom M-Pesa | **6.4M active in 2024 (+28.4% YoY)**; ~51% share of mobile money subscribers; 2025 revenue $207.1M | 2024 (Vodacom RDC company release via BANKABLE — self-reported) |
| Airtel Money | **1M revenue-earning users** in DRC; ~31% share; 2025 revenue $194.8M (closing the gap on M-Pesa) | Mobile World Live + BANKABLE — self-reported, date specifics imperfect |
| Orange Money | ~17% share | Third-party feasibility analysis (2025-08) |
| Afrimoney (Africell) | **<1% share** of mobile money (but Africell has 20–25% telecom share in active provinces) | Third-party feasibility analysis (2025-08) + Africell self-reported telecom share |

Sources: xo-006, xo-007, mp-008, am-007, mp-012, af-004. **All operator figures are self-reported by the operator; share splits are third-party Medium-confidence estimates.**

### Approval flow

- All four use the standard MMO pattern of **USSD/STK push with a PIN prompt** to the payer.
- M-Pesa's verbatim flow is verified for Kenya (Lipa Na M-Pesa Online / STK push); the DRC flow is **inferred to be the same UX** because it runs on the same M-Pesa Open API platform.
- Airtel and Orange flows are similar industry standard but DRC verbatim text is not documented publicly.
- **None offer a smoother "approve-in-app" UX without operator-side partnership** (e.g., a deep link into the operator app), as far as public docs show. The MVP gets the operator's USSD UX regardless of which path it picks.

### Fees and pricing posture

- **None of the four operators publish their API-driven merchant fees for DRC**. M-Pesa's onboarding doc says explicitly: "the M-Pesa Business team will review the application and inform you of the M-Pesa charging model." Orange and Airtel show the same posture (consumer tariff published; API-merchant tariff not).
- **Consumer tariffs that are public:**
  - **Vodacom M-Pesa DRC**: Tier withdrawals 18% → 1.5% USD; 13.5% → 0.63% CDF (search summary of vodacom.cd tariff page).
  - **Orange Money DRC**: Withdrawals 9% → 1% (tiered); on-net **and** off-net transfer 1% flat (a notable signal that consumer cross-network transfer is a designed feature, not just an exception).
  - **Airtel Money DRC**: Not extractable from public web (JS-rendered).
  - **Afrimoney DRC**: "Rates displayed at agent points" — not on public web.
- For the MVP, the relevant cost is the **API-driven fee a partner pays**, not the consumer tariff. pawaPay's published DRC pass-through fees (1.5%–2% MMO for M-Pesa, 2% for Airtel/Orange collection legs) are the best public proxy for what each operator charges an aggregator.

### Onboarding posture

| Operator | Sandbox access | Production access |
|---|---|---|
| M-Pesa | Self-serve developer signup; sandbox after email/phone confirmation | Organisation onboarding via `supportmpesa@m-pesa.cd`: KYC + sign M-Pesa Services Agreement + bank-account details → merchant code |
| Airtel | Self-serve at `developers.airtel.africa/developer`; sandbox immediately | Contract signing per country to go live ("after completion of onboarding and integration") |
| Orange | Portal signup; OM Web Payment limited to KYA-compliant Orange Money merchants registered through an Orange store in country | Multi-step: subscribe to OM service → integrate → accept; central-bank approval required |
| Afrimoney | None located publicly | Contact `info@africell.cd` or `1010` |

## Comparison / key data

| Question | M-Pesa | Orange | Airtel | Afrimoney |
|---|---|---|---|---|
| Collection (C2B)? | ✅ | ✅ (Web Payment) | ✅ | ✅ (announced Dec 2023; live status unclear) |
| Disbursement (B2C)? | ✅ to M-Pesa wallets | ❌ via Orange's own DRC API (not public) | ✅ | ❓ (forthcoming) |
| Sandbox URL public? | After signup | Not public | **Yes — `openapiuat.airtel.africa`** | None |
| API per-leg fees public? | ❌ | ❌ | ❌ | ❌ |
| Currencies | CDF + USD | CDF + USD | CDF + USD (inferred) | CDF + USD |
| Reach (mobile money) | ~51% / 6.4M | ~17% | ~31% / 1M revenue-earners | <1% |
| Easiest to integrate today | — | — | **Yes** (open dev portal, OAuth, sandbox) | — |

## Confidence & open questions
- **Confident about:**
  - Each operator's existence and broad capability set (M-Pesa: C2B+B2C+status; Orange: collection-only via om-webpay; Airtel: C2B+B2C through unified portal; Afrimoney: forthcoming).
  - Both CDF and USD are supported by all four.
  - No operator publishes API-driven merchant fees for DRC.
- **Uncertain / unverified:**
  - DRC-specific tariffs for M-Pesa and Airtel API merchants (operators decline to publish; aggregator pass-through is best proxy).
  - Whether B2C payouts can target non-registered numbers in any operator (likely no, but unverified).
  - The DRC mechanics of the airtel.cd "interopérabilité" page (URL exists; body opaque).
  - Settlement timing per operator.
  - Current BCC regulatory stance on interoperability (2014 source cited; 2025/2026 view not located).
- **Still missing (and how to get it):**
  - Vodacom DRC API fee quote — email `supportmpesa@m-pesa.cd`.
  - Orange Money DRC B2C/payout availability — contact `Serviceclients.RDC@orange.com`.
  - Airtel Africa DRC sandbox usage notes and contract terms — sign up at `developers.airtel.africa`.
  - Afrimoney DRC API status — `info@africell.cd`.
  - Current BCC interoperability and licensing position — BCC primary documents.

## Sources
By ID, from `04-sources/sources.md`: mp-001, mp-002, mp-003, mp-004, mp-005, mp-006, mp-007, mp-008, mp-009, mp-010, mp-011, mp-012; om-001, om-002, om-003, om-004, om-005, om-006, om-007, om-008, om-009; am-001, am-002, am-003, am-004, am-005, am-006, am-007, am-008, am-009; af-001, af-002, af-003, af-004, af-005, af-006; xo-001, xo-002, xo-005, xo-006, xo-007.
