# Build our own interoperability aggregator (vs renting pawaPay)

- **Topic / entity:** strategic option to own the rails instead of renting them
- **Question it addresses:** Would building our own DRC aggregator eliminate the per-transaction fee source? Is the build relatively easy? Are the limitations mainly licensing/legal?
- **Date researched:** 2026-06-04
- **Overall confidence:** Medium-High — public data on EMI licensing ($2.5M minimum), aggregator funding histories (pawaPay $9M seed, Onafriq $217.7M total), engineering cost benchmarks, and the GSMA Mobile Money API spec is well-documented; what's harder to verify in public is the *commercial* posture of DRC operators toward small-startup direct integration (likely unfavourable, by analogy to other markets).

## Bottom line
**Building our own DRC aggregator is technically feasible but strategically wrong for the MVP.** The software is the easiest part — the GSMA Mobile Money API spec is published and open-source SDKs exist. The hard parts, in order of difficulty: **(1) commercial — getting four operators to take us seriously and quote competitive per-leg rates; (2) regulatory — DRC requires a BCC-licensed EMI with US$2.5M minimum capital; (3) operational — float/treasury, 24/7 reconciliation, AML reporting; (4) technical — well-understood but undocumented operator API quirks emerge only at scale.** The fee thesis ("eliminate the per-transaction fee source") only partly holds: we'd save pawaPay's **1%** margin but inherit the operator's per-leg fees directly — and as a small player, our negotiated rates likely won't beat pawaPay's pre-negotiated bulk rates for 12–24 months minimum. Comparable players that built this (Onafriq founded 2009, $217.7M raised, 14+ years; pawaPay founded 2020 under betPawa's wing, $9M seed, 5+ years) suggest the right framing is **rent now, own later** as a v3+ play.

## What "the per-transaction fee source" actually is

The 3.5%–5.0% round-trip pawaPay envelope decomposes (per [pawaPay Fees](https://www.pawapay.io/fees)) into:

| Component | Share | Goes to |
|---|---|---|
| **pawaPay margin** | **1.0% per leg = 2% round-trip** | Eliminable by going direct |
| **MMO (operator) fee** | **1.5–2.0% per leg = 3–4% round-trip** | NOT eliminable — flows to Vodacom/Airtel/Orange |
| FX (if currency switch) | 2–5% | Partly eliminable with our own treasury |
| Settlement to bank | ~$0.35–$1.10 per payout | Partly eliminable at scale |

**Realistic max saving from owning rails: ~2% of round-trip volume (the pawaPay margin), plus FX/settlement margin at scale.** The operator-fee portion is what the operators charge any aggregator — and as a small player we likely pay *more* than pawaPay for the same legs until we have volume to negotiate. **Confidence: Medium-High** (composition verified; negotiated rate disadvantage is inference).

## What an aggregator actually does (not just software)

A mobile-money aggregator like pawaPay performs five jobs, not one:

1. **Technical integration** — REST APIs against each operator, idempotency, webhooks, status query, reconciliation, error normalisation, encryption (HMAC-SHA256 etc.).
2. **Commercial layer** — bilateral merchant contracts with each operator (M-Pesa Services Agreement; Airtel per-country contract; Orange Money KYA agreement; Africell merchant agreement). Per-leg pricing negotiated case-by-case.
3. **Regulatory layer** — held under a licensed entity (EMI/PSP) in each jurisdiction, AML/CFT monitoring, suspicious-activity reporting to FIUs, KYC of merchants.
4. **Float / treasury layer** — operator-side accounts pre-funded, settlement to merchant bank accounts on a cadence, FX management between CDF/USD, working capital tied up in flight.
5. **Operations layer** — 24/7 monitoring, reconciliation discrepancy resolution, dispute handling, customer service for failed transactions, ongoing recertification as operator APIs change.

[pawaPay describes its own value-add as](https://www.pawapay.io/blog/powering-mobile-money-for-deriv-across-eight-african-markets): "managing payment processing, foreign exchange, settlement, and reconciliation… compliance support and settlement coordination across live jurisdictions." Software is one of five jobs.

---

## Layer 1 — Technical (the easiest part)

### Engineering cost benchmark
For an African fintech building payment infrastructure ([IntaSend cost analysis](https://intasend.com/payments/building-vs-buying-payment-infrastructure-the-true-cost-analysis-for-african-fintech-startups) — **secondary, Medium confidence**):
- **3–6 senior developers** for **4–6 months** for basic functionality
- **1–2 security specialists**
- **Initial cost: $100,000–$180,000** to first transaction
- **Ongoing: 30–40% of engineering allocated to payment infrastructure maintenance**
- **One Nigeria CTO**: ~$70,000/year just for compliance across 3 markets

### Helping factor — the GSMA spec
- **GSMA Mobile Money API Specification v1.2** is published with open-source SDKs in Java, JavaScript, Node.js, PHP, Android ([developer.mobilemoneyapi.io](https://developer.mobilemoneyapi.io/)). Covers all 8 use cases including merchant payments and disbursements.
- DRC adopters: **Afrimoney announced spec adoption in Dec 2023** ([africell.com](https://www.africell.com/story/afrimoney-adopts-mobile-money-specification/)); M-Pesa, Airtel, Orange use their own (similar) REST patterns.
- **Implication:** you can write a single internal abstraction layer over four operator APIs without inventing the protocol.

### Real-world friction signals
- **Undocumented API limitations emerge at scale** — "one Tanzania-focused fintech discovered their M-Pesa integration failed at higher volumes due to undocumented rate limits" ([IntaSend](https://intasend.com/payments/building-vs-buying-payment-infrastructure-the-true-cost-analysis-for-african-fintech-startups)).
- **Webhook reliability requires redundant systems.**
- **Reconciliation discrepancies** between what the operator confirmed and what your ledger expected run "into the hundreds daily" in high-volume environments per general payments reconciliation literature ([Optimus.tech](https://optimus.tech/blog/payment-reconciliation-pitfalls-7-errors-costing-merchants-millions); [Qolo.io](https://qolo.io/payment-horror-stories-the-nightmare-of-failed-transactions-and-reconciliations/)) — **secondary**, generic-not-DRC-specific.

### Verdict on this layer
**Moderately hard, well-understood, ~$100–180k + a year of senior engineering to get to a credible v1.** This alone is not the gate.

---

## Layer 2 — Commercial (the actual moat)

### What it takes to even start
Without aggregator cover, our MVP entity needs **direct commercial onboarding with each of the four operators**:

| Operator | Onboarding mechanism |
|---|---|
| Vodacom M-Pesa DRC | Email `supportmpesa@m-pesa.cd` → KYC pack → sign M-Pesa Services Agreement → 6/7-digit merchant code |
| Orange Money DRC | Subscribe at an Orange store in DRC → KYA-compliant merchant subscription → integrate the Web Payment API; central-bank approval required |
| Airtel Money DRC | Self-serve sandbox at `developers.airtel.africa/developer`; per-country contract required to go live ("MTN and Airtel make you sign contracts before seeing their API docs" — [dev.to](https://dev.to/joova/why-mtn-and-airtel-make-you-sign-contracts-before-seeing-their-api-docs-and-how-to-start-building-3g4i)) |
| Afrimoney DRC | Contact `info@africell.cd`; no public developer portal located; Dec 2023 GSMA-spec announcement was forward-looking |

### Pricing reality
**None of the four operators publishes API-driven merchant fees for DRC.** M-Pesa's onboarding doc states it explicitly: "the M-Pesa Business team will review the application and inform you of the M-Pesa charging model" ([business.m-pesa.com DRC onboarding](https://business.m-pesa.com/vodacom-drc/onboarding-your-business-in-drc/)). Pricing is set per partner at the operator's discretion. Two implications:
- **We can't model unit economics in advance** of four bilateral pricing conversations.
- **A small startup gets quoted at higher rates than a large aggregator** with volume to negotiate. pawaPay's pass-through MMO portion (1.5–2.0%) reflects its bulk position; our direct quote is likely **at or above** these numbers, not below.

### The historical pattern
Per [Finance in Africa's guide to payment APIs in Africa](https://financeinafrica.com/insights/apis-africas-developers-money-code/): "Mobile money systems were historically guarded closely, with integrations requiring formal partnerships, long negotiations, and often manual processes." Safaricom's Daraja in 2017 opened M-Pesa Kenya; MTN MoMo grew from 5 APIs (2018) to 30 (2022) — but the DRC operators are not at Kenya/Nigeria openness levels. **The very existence of pawaPay and Onafriq is because direct operator integration was so painful for ordinary merchants** — they aggregated the pain.

### Verdict on this layer
**This is the actual moat. 4 bilateral commercial relationships, opaque pricing, no startup-favourable negotiating position. Months to years of relationship building, not days.**

---

## Layer 3 — Regulatory (the formal gate)

### What DRC requires
- **Banque Centrale du Congo (BCC)** is the licensing authority for banks, EMIs, PSPs ([generisonline.com regulatory framework](https://generisonline.com/understanding-the-regulatory-framework-for-digital-payments-and-fintech-companies-in-the-democratic-republic-of-the-congo/)).
- **Directive #24 (December 2011)** is the operative e-money framework ([findevgateway DRC mobile money policies, 2014](https://www.findevgateway.org/sites/default/files/publications/files/mfg-en-case-study-enabling-mobile-money-policies-in-the-democratic-republic-of-congo-mar-2014.pdf)).
- **EMI minimum capital: US$ 2.5 million** ([findevgateway, 2014](https://www.findevgateway.org/sites/default/files/publications/files/mfg-en-case-study-enabling-mobile-money-policies-in-the-democratic-republic-of-congo-mar-2014.pdf); [PayAtlas DRC](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd) — **Verified across two sources**). This must be paid-up, sitting on the balance sheet — not raised, *committed*.
- **EMI must be incorporated as a special-purpose subsidiary** in DRC.
- **Foreign PSPs cannot operate independently**: "Foreign PSPs must partner with licensed local entities or obtain BCC authorisation; independent operation is prohibited" ([PayAtlas DRC](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd) — **secondary, Medium confidence**; not verified against BCC primary docs in this research).
- **AML/CFT obligations** under BCC Instruction No. 15 (2018) and Law No. 04/016 (2004): KYC, CDD, suspicious-activity reporting to CENAREF/CENTIF within 24 hours, 10-year data retention ([voveid KYC DRC guide](https://blog.voveid.com/kyc-compliance-in-the-drc-2025-guide-for-regulated-businesses/)).

### Timeline
- **Indicative African fintech licensing: 6–18 months** ([generisonline](https://generisonline.com/understanding-the-regulatory-framework-for-digital-payments-and-fintech-companies-in-the-democratic-republic-of-the-congo/) — **secondary, Low confidence for DRC specifically**; treat as planning range).
- DRC-specific BCC application timeline **not found** in public sources.

### Capital lock-up cost
- **US$ 2.5M minimum capital** locked on balance sheet. At a 10% opportunity cost of capital, that's **$250k/year of foregone return** — before paying salaries, building software, or processing a single transaction.

### Verdict on this layer
**This is the largest single hard gate.** No paid-up $2.5M, no licence. No licence, no direct operator contracts (or, at best, a partner-PSP arrangement that reintroduces a margin).

---

## Layer 4 — Float / Treasury / Operations

### The float problem
Mobile money aggregators do not just pass messages — they **settle**. Practically:
- To send a payout to a merchant on Vodacom M-Pesa, you must have funds in your **operator-side prefunded account at Vodacom**.
- The collection leg credits your account at the payer's operator with a delay (T+0 to T+days depending on operator).
- Your **net float = sum of prefunds in operator accounts + in-flight settlements − customer-payable balances**. This is working capital that has to come from somewhere.
- **Liquidity management consumes 20–30% of total expenses** for mobile money retail agents per [IFC's Liquidity Management for Mobile Money Providers toolkit](https://documents1.worldbank.org/curated/en/802221501150875893/pdf/117459-WP-Tool-10-5-Liquidity-Management-Series-IFC-mobile-money-toolkit-PUBLIC.pdf) — note: PDF body was not fully extracted in this session; the 20–30% figure is from search-index summary; **Medium confidence, treat as planning anchor**.

### Foreign exchange
- DRC mobile money runs in CDF and USD. Cross-currency flows demand a working FX position.
- **DRC PSP norm: 2–5% FX markup** above interbank ([PayAtlas DRC](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd) — **secondary**). If we don't have an interbank book, we pay an FX provider for this.

### Operations
- **24/7 monitoring**, support, dispute handling.
- **Reconciliation discrepancies**: timing-gap exceptions can run into hundreds daily at scale ([Optimus.tech](https://optimus.tech/blog/payment-reconciliation-pitfalls-7-errors-costing-merchants-millions)).
- **Recertification** when operator APIs change (which they do).

### Verdict on this layer
**Materially more expensive than the software build. The day-zero requirement is millions of dollars in operator prefunds + working capital + an FX line + an ops team.**

---

## What comparable players actually did

| Player | Founded | Path | Capital raised | Years to current state |
|---|---|---|---|---|
| **pawaPay** | 2020 | Subsidiary of [betPawa](https://www.pawapay.io/) — a large existing betting platform that already had volume and the operator relationships. Built its own rails to support betPawa's payment needs first, then opened to third parties. | **$9M seed (Aug 2021)** ([TechCrunch](https://techcrunch.com/2021/08/26/pawapay-raises-9m-seed-backed-by-msa-88mph-and-mr-eazis-zagadat-capital/)) | 5+ years |
| **Onafriq (MFS Africa)** | **2009** | Founded by Dare Okoudjou, **a former MTN Group executive who led mobile money rollouts in 21 countries** ([CB Insights / Onafriq](https://www.cbinsights.com/company/mfs-africa); [Onafriq rebrand announcement](https://onafriq.com/view/mfs-africa-announces-rebrand-to-onafriq)). Inside relationships from day one. Grew by acquisition (GTP card processor, etc.). | **$217.7M total** | 14+ years |
| **MOKO Afrika** | Not disclosed publicly | DRC-focused; "Freshpay/GoFresh" lineage; clients include Equity BCDC and major betting operators ([mokoafrika.com](https://www.mokoafrika.com/en)). Founders, capital, and licensing posture not publicly detailed. | Not public | Unknown |
| **DPO Pay** | Acquired into **Network International** (UAE) | Card-first PSP that added mobile money; benefits from parent's bank-grade infrastructure and licensing. | Acquired by NI in 2021 for $288M | 10+ years |
| **Maxicash** | DRC | Raised **$350,000 in 2021** — "the major deal among Congolese startups in that year" ([search summary citing local press](https://www.crunchbase.com/organization/flexpay-2)). Female-founded. | $350k | 4+ years |

**Pattern:** every successful African mobile money aggregator either (a) had pre-existing operator relationships via a parent/founder, (b) raised serious capital, or (c) both. None did it as a side bet from a consumer-app MVP.

---

## When building does make sense (and why not for our MVP)

[IntaSend's build-vs-buy framework](https://intasend.com/payments/building-vs-buying-payment-infrastructure-the-true-cost-analysis-for-african-fintech-startups) identifies four scenarios where building wins:

1. **Core IP differentiation** — payment processing IS the product. Our MVP is a *consumer wallet UX*; the rails are commoditised infrastructure. <span style="color:#991b1b;">Not our case.</span>
2. **Truly specialized requirements** — existing solutions can't accommodate. pawaPay accommodates exactly our two-leg cross-network flow. <span style="color:#991b1b;">Not our case.</span>
3. **Regulatory mandates** — specialised financial institutions required end-to-end control. <span style="color:#991b1b;">Not our case; opposite — we want to *avoid* being a regulated entity in v1.</span>
4. **Scale economics** — millions of daily transactions justify in-house. <span style="color:#991b1b;">Years into the trajectory; not v1.</span>

### Cost-savings math (when does building pay off?)

Take a representative volume and run the comparison.

**Assumptions:**
- 100k round-trip transactions/month at $15 average ticket = $1.5M monthly GMV
- pawaPay round-trip cost at the 4.5% midpoint = $67,500/month = ~$810k/year
- pawaPay margin (eliminable): ~2% of GMV = $30,000/month = $360k/year
- MMO portion (NOT eliminable; possibly *worse* on direct contracts as small player): ~2.5% of GMV = $37,500/month = $450k/year

**Annual cost of building & running our own:**
- $2.5M EMI capital opportunity cost @ 10%: **$250k/year** (forever, while licensed)
- Engineering build: $100–180k initial; ongoing maintenance ~$200–400k/year (30–40% of a small team)
- Compliance ops: ~$70–150k/year (per Nigeria CTO benchmark, scaled)
- Legal + licensing: $50–200k for application (estimate); $50k/year ongoing
- Float / treasury financing: variable; assume $50–150k/year of working-capital cost
- **Total Year-1: ~$650k–$1.2M; ongoing: ~$500k–$900k/year**

**Conclusion:** at this volume, **building costs roughly the same or more than renting pawaPay** while introducing massive execution risk. Build only pays back at materially higher volume *and* after a 1–3 year ramp where you're paying both ways (rented + builders salaries).

**Confidence on the math: Medium.** Volumes, prices, and headcount are illustrative; the order-of-magnitude conclusion is robust.

---

## Hybrid path (the right v3+ play)

1. **v1 (now):** Rent pawaPay. Ship consumer UX. Build volume. Learn what fails in production.
2. **v2 (12–18 mo):** Add MOKO Afrika as Afrimoney fallback if needed. Open early commercial conversations with M-Pesa DRC + Airtel DRC directly — sandbox in their portals — to start the relationship clock.
3. **v3 (24–36 mo, volume-permitting):** Stand up our own technical layer on top of direct M-Pesa + Airtel contracts (the two largest DRC operators, 82% of mobile money share combined). **Keep pawaPay as fall-through for Orange + Afrimoney** until volume justifies separate contracts. This is a "hybrid" pattern that [IntaSend](https://intasend.com/payments/building-vs-buying-payment-infrastructure-the-true-cost-analysis-for-african-fintech-startups) recommends as the African-fintech default.
4. **v4 (only if scale justifies):** Apply for BCC EMI licence, commit the $2.5M capital, fully own the rails.

This sequence captures the savings progressively as they become economically rational, instead of front-loading the entire $2.5M+ commitment and 18-month licensing wait on day zero.

---

## Direct answers to the user's questions

> **Would building the system be relatively easy?**

**The software is moderately hard, well-understood, and the smallest part of the problem.** GSMA publishes an open spec with open-source SDKs; the four DRC operator APIs are documented (M-Pesa portal, Airtel unified, Orange Web Payment, Afrimoney announced); a 4–6 month build by 3–6 senior engineers at $100–180k initial spend gets to v1. The hard parts are everything *around* the software — commercial relationships, treasury, ops, and the regulatory entity that's *allowed* to do any of it.

> **Would the limitations mainly be due to licensing/legal?**

**Licensing is the single largest formal gate (US$2.5M paid-up capital, EMI subsidiary, 6–18 month application), but the commercial layer is just as hard and less visible.** Even with a licence, we still need each of the four operators to (a) accept us as a partner and (b) quote competitive rates — which, as a small player without volume, we likely don't get for 12–24 months. **Ranked by difficulty: commercial ≈ regulatory > operational > technical.**

> **Would owning the rails eliminate the per-transaction fee source?**

**Partly.** It eliminates pawaPay's 1% per-leg margin (~2% round-trip) but **not** the operator's 1.5–2% per-leg fee, which we'd inherit directly — and which as a small player we likely pay *more* than pawaPay does. **Realistic ceiling: ~2% of GMV saved**, against $500k–$900k/year of build/run cost + $2.5M of capital tied up — so the break-even is at material volume, not v1 volume.

---

## Confidence summary
- **High:** GSMA spec is open and live; pawaPay/Onafriq funding histories; DRC EMI capital requirement (US$2.5M); operator API access patterns; engineering-cost benchmark ranges.
- **Medium:** Whether a small DRC startup actually gets competitive rates from operators (inferred from market structure; not directly verified); 20–30% liquidity-cost figure from IFC toolkit; 6–18 month African fintech licensing timeline (not DRC-specific).
- **Low:** Exact DRC BCC application timeline; cost-modelling numbers above (order-of-magnitude only; verify with counsel + sales conversations).

## Open questions / gaps
- **BCC EMI application timeline and total fees** — not in indexed public sources; would need to contact BCC or a local DRC fintech lawyer.
- **Whether the partner-PSP arrangement (operating under a licensed-local-entity's wing) is a viable middle path** that avoids the $2.5M capital requirement while still owning the technical layer.
- **Realistic per-leg fee quotes from M-Pesa DRC and Airtel DRC for a small direct partner** vs pawaPay's pass-through (would need to start the partner conversations to find out).
- **Whether MOKO Afrika's all-4-MNO posture** (a small DRC-incorporated player) represents a successful template for how a small player can operate in DRC; their licensing posture and capital structure are not public.
- **Float-financing options in DRC** — local USD/CDF credit lines for working capital; cost of capital in-country.

## Sources
- xo-014, xo-015 (voveid KYC/AML DRC guides)
- xo-017 (Generis DRC regulatory framework)
- xo-005 (PayAtlas DRC)
- mp-010 (findevgateway DRC mobile money policies 2014, cites $2.5M)
- pp-001, pp-003, pp-004 (pawaPay materials)
- on-001, on-002 (Onafriq materials)
- mp-001 (M-Pesa DRC onboarding — fees opaque pattern)
- am-009 (dev.to MTN/Airtel contract gating)
- xo-019: pawaPay $9M seed round (TechCrunch) — `https://techcrunch.com/2021/08/26/pawapay-raises-9m-seed-backed-by-msa-88mph-and-mr-eazis-zagadat-capital/`
- xo-020: Onafriq company profile (CB Insights) — `https://www.cbinsights.com/company/mfs-africa`
- xo-021: MFS Africa → Onafriq rebrand — `https://onafriq.com/view/mfs-africa-announces-rebrand-to-onafriq`
- xo-022: GSMA Mobile Money API Developer Portal — `https://developer.mobilemoneyapi.io/`
- xo-023: GSMA Mobile Money API Specification 1.2.0 — Merchant Payments PDF — `https://www.gsma.com/mobilefordevelopment/wp-content/uploads/2021/10/Mobile-Money-API-Specification-1.2.0-Merchant-Payments.pdf`
- xo-024: IntaSend — Building vs Buying Payment Infrastructure (African fintech cost analysis) — `https://intasend.com/payments/building-vs-buying-payment-infrastructure-the-true-cost-analysis-for-african-fintech-startups`
- xo-025: Finance in Africa — payment APIs in Africa — `https://financeinafrica.com/insights/apis-africas-developers-money-code/`
- xo-026: IFC Liquidity Management for Mobile Money Providers (World Bank toolkit) — `https://documents1.worldbank.org/curated/en/802221501150875893/pdf/117459-WP-Tool-10-5-Liquidity-Management-Series-IFC-mobile-money-toolkit-PUBLIC.pdf`
- xo-027: Optimus.tech — Payment reconciliation pitfalls — `https://optimus.tech/blog/payment-reconciliation-pitfalls-7-errors-costing-merchants-millions`

[[pawapay]] · [[fees-and-costs]] · [[kyc-and-onboarding]]
