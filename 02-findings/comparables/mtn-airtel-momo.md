# Comparable: MTN MoMo & Airtel Money — operator-led mobile money at continental scale

- **Topic / entity:** MTN Mobile Money (MoMo) and Airtel Money / Airtel Mobile Commerce — the two largest pan-African telco-led mobile money platforms
- **Question it addresses:** How do operator-led mobile money businesses structure their product (USSD → app → merchant → open API), monetise, and sequence rollout — and what does that mean for a consumer cross-network pass-through app that *consumes* aggregator APIs and serves feature-phone users?
- **Date researched:** 2026-06-09
- **Confidence:** Medium–High (corporate financials High; API UX/pricing detail Medium; DRC-specific Medium)
- **Claim type:** mix — labelled inline per claim

---

## Why these two are the key comparable

MTN and Airtel are the **incumbents we would both ride and compete with**. They are the rails. In the DRC specifically (see caveat below), Airtel is the #2 mobile money operator (~31% of subscriptions) behind Vodacom M-Pesa (~51%), with Orange (~17%) and Africell (<1%) — so when our app collects from / pays out to a Congolese MSISDN, a large share of those legs terminate in an **Airtel Money** or M-Pesa wallet. ([RDC-Analyse feasibility study, Aug 2025](https://rdc-analyse.org/files/2025/08/2025_08_CAT_Feasibility-analysis-of-Mobile-Money-in-eastern-DRC_VE-1.pdf), regional/NGO analysis, Medium). The DRC mobile-money market reached ~29 million active users by end-2024 (same source).

**Critical DRC caveat (verified fact, High):** **MTN does *not* operate a licensed mobile network in the DRC.** The DRC market is Vodacom, Airtel, Orange and Africell; MTN's nearest networks are in Republic of Congo (Brazzaville), Rwanda, Uganda, Zambia and South Sudan. In Feb 2026 the DRC regulator (ARPTC) accused MTN of providing service without a licence in eastern border areas (signal spillover). ([Connecting Africa](https://www.connectingafrica.com/regulation/drc-accuses-mtn-of-illegally-operating-in-country); [Ecofin Agency, Feb 2026](https://www.ecofinagency.com/news-digital/1202-52842-drc-accuses-mtn-of-illegal-operations-spotlighting-border-frequency-issues)). **So MTN MoMo is studied here as a model/playbook, not as a DRC rail. Airtel Money is the directly relevant one for our DRC legs.**

---

## Lens 1 — Product structure & architecture

### USSD-first ubiquity, then app, then merchant, then API
Both started as **USSD-first** services bolted onto the telco SIM, reaching any handset with no smartphone or data required. This is the foundation of their reach across ~14+ countries each. The broader context (GSMA, verified fact, High): mobile money surpassed **2 billion registered accounts** globally in 2024, Africa holding 1.1bn (53%); ~28 million registered agents (10m active monthly); ~$1.68 trillion processed, with Africa at ~$1.1tn (65% of global value). USSD + a human **agent cash-in/cash-out network** is what made that possible. ([GSMA State of the Industry 2025](https://www.gsma.com/sotir/wp-content/uploads/2025/04/The-State-of-the-Industry-Report-2025_English.pdf); [GSMA newsroom](https://www.gsma.com/newsroom/press-release/maturing-global-mobile-money-market-hits-1-4tn-in-transaction-value/)).

The product surface then layered up:
- **USSD menu** (feature phones) → the universal base layer.
- **Smartphone apps** — the **MoMo app** (MTN) and **Airtel Money / Airtel Thanks** app — push richer UX, QR, and cross-sell. The MoMo consumer app is on Google Play. ([MoMo on Google Play](https://play.google.com/store/apps/details?id=mtnft.momo.consumer&hl=en)).
- **Merchant pay** — **MoMoPay** lets merchants accept via QR code, merchant ID, or payment request. ([MoMoPay, MTN Uganda](https://www.mtn.co.ug/helppersonal/momopay/); [ugtechmag](https://ugtechmag.com/mtn-momo-pay/)).
- **Open API / developer platform** — the layer most relevant to us (below).
- **Agent network** — the physical cash-in/out layer underpinning all of the above; in the DRC "an already structured agent network exists around major towns" across Vodacom, Airtel, Orange, Africell. ([Communications Africa](https://communicationsafrica.com/mobile/mtn-airtel-money-boost-mobile-money-in-congo)).

### The MoMo Open API (directly relevant — we consume aggregator APIs)
MTN launched its **Open API program in Uganda in 2018** and it now spans **14+ African countries**. It exposes the same primitives an aggregator like pawaPay would expose to us (self-reported / verified-fact mix, Medium–High):
- **Collections** — pull funds from a consumer wallet (C2B, bill/fee/merchant payment). ([MoMo Dev Community](https://momodevelopercommunity.mtn.com/getting-started-in-the-community-2/what-are-momo-open-apis-107)).
- **Disbursements** — push funds to one or many wallets (payroll, refunds, aid). (same).
- **Remittances** — cross-border individual transfers. ([MoMo API page](https://momo.mtn.com/api/)).
- **Collection widget** and notifications/refund. (same).

**User-experience flow (verified fact, High — important for our UX design):** A MoMo "request to pay" (collection) moves through three states — **(1) approval**: the consumer enters their **MoMo PIN to confirm**; **(2) pending**: awaiting approve/reject; **(3) rejected**. Crucially, "services created to be accessed via Web, App and USSD channels can consume the Open API," and the **MoMo PIN is the mechanism for accepting terms and authorising**. ([MoMo Dev Community — what are Open APIs](https://momodevelopercommunity.mtn.com/getting-started-in-the-community-2/what-are-momo-open-apis-107)). **This confirms the pattern we already assume for pawaPay-style collections: the payer authorises the debit in their own operator's PIN prompt, not inside our app.** Pre-approval mandates also exist for repeat debits (community-discussed, Medium).

**Airtel Money Developer Portal** mirrors this: a pan-African open-API portal (signup → register an app → add products per country → go live) supporting **Collections** (accept wallet payments) and **Disbursements** (refunds, payroll, commissions, transfers, cross-border remittance) with **instant settlement** to Airtel wallets, OAuth2 client-credentials, sandbox + production endpoints. ([Airtel developer portal](https://developers.airtel.africa/); [The Condia](https://thecondia.com/airtel-money-developer-portal-mtn-chenosis-api/); [Apps Africa](https://www.appsafrica.com/airtel-africa-developer-portal-aims-to-expand-digital-payments/)).

**Relevance:** both operators expose exactly the collect-leg + payout-leg primitives our pass-through model needs. An aggregator (pawaPay) sits on top of *several* such operator APIs and gives us one integration; these portals show what the underlying per-operator capability looks like, and confirm the PIN-in-operator-menu authorisation UX.

---

## Lens 2 — Monetisation & value capture

### How they make money (multiple revenue lines)
Operator mobile money monetises across (verified fact / self-reported, Medium–High):
- **P2P transfer fees & cash-out fees** — the historic core (cash-out at agents especially).
- **Merchant payments (MoMoPay)** — MTN entered the formal merchant-payments market pricing MoMoPay at **0.5%**; between Jan–Jun 2025 merchants processed **$9.9bn** in MoMoPay value. ([TechCabal, May 2025](https://techcabal.com/2025/05/21/mtns-momo-pay-enters-the-payments-market-with-0-5-fee/)).
- **Lending / advances ("BankTech")** — MoMo-Advance, Merchant-Advance, Agent-Advance, **Airtime-Advance**; nano-loans/overdrafts underwritten on telco + transaction data. In **H1 2025 MTN disbursed ~$1.3bn in microloans, averaging ~$20/loan**, and airtime lending (Xtratime) is an explicit fintech-revenue driver. ([TechCabal, Nov 2025](https://techcabal.com/2025/11/04/mtn-fintech-revenue-2025-airtime-lending/); [MTN Fintech & Digital Services presentation](https://mtn-investor.com/mtn-cmd/pdf/presentations/fintech-and-digital-services.pdf)).
- **Airtime / bill / utility commissions** — users resell airtime, prepaid electricity, transport tickets, DStv etc., earning commission per transaction. ([ugtechmag](https://ugtechmag.com/mtn-momo-pay/)).
- **Remittance / cross-border** — MTN reported a cross-border payment surge (~$2.1bn) and is pushing simpler remittance. ([TechCabal, Oct 2025](https://techcabal.com/2025/10/13/mtn-momo-eyes-simpler-cross-border-payments-after-2-1bn-surge/)).
- **API / aggregator fees** — the Open API portal itself is **free to register, sandbox and test; MTN earns on a fee per transaction once live**, deducted directly from the partner's collections account; rates vary by country/product and are set in the contract schedule (not publicly disclosed). ([momo.mtn.com pricing](https://momo.mtn.com/pricing/); [CGAP "was it worth it"](https://www.cgap.org/blog/mtn-mobile-money-opened-apis-was-it-worth-it)).

### Scale as standalone fintech businesses (verified fact, High)
**MTN Group fintech (FY2024):**
- ~**63 million active MoMo users** (up <1% YoY). ([MTN FY24 results](https://www.mtn.com/wp-content/uploads/2025/03/MTN-Group-FY-24-results-complete-booklet-HR.pdf); [Statista](https://www.statista.com/statistics/1139539/mtn-mobile-money-active-accounts/)).
- Fintech **transaction volumes 20.3bn (+15.3%)**; **transaction value ~US$321.3bn (+35.1%)**; **fintech revenue +21.6%** (Q4 +38.3%). ([MTN FY24 results booklet](https://www.mtn.com/wp-content/uploads/2025/03/MTN-Group-FY-24-results-complete-booklet-HR.pdf); [MTN press release](https://www.mtn.com/in-a-challenging-macro-mtn-group-reports-strong-underlying-2024-performance/)).
- **Valuation: ~US$5.2bn** set by the **Mastercard** minority-investment deal (up to $200m). ([TechCrunch, Aug 2023](https://techcrunch.com/2023/08/14/mastercard-to-purchase-a-minority-stake-in-mtns-5-2b-fintech-business/)).

**Airtel Money / Airtel Mobile Commerce (Airtel Africa, FY ended 31 Mar 2024 + later):**
- **~31.5m mobile money customers** at FY24 (+20.4%), rising to ~41.5m by H1 FY25. ([Airtel Africa Strategic Report 2024](https://cdn-webportal.airtelstream.net/website/investor/main/pdf/annual-report/Strategic-Report-2024.pdf); [Ecofin Agency, Oct 2024](https://www.ecofinagency.com/news-finances/2910-49935-airtel-africa-profit-soars-375-in-h1-2025-on-data-and-mobile-money-growth)).
- **Mobile money revenue ~$649m** (FY24, +32.8%); **ARPU ~$262/customer/month** transaction value (+13.1% cc); H1 FY25 annualised transaction value ~$128bn. ([Airtel Africa Strategic Report 2024](https://cdn-webportal.airtelstream.net/website/investor/main/pdf/annual-report/Strategic-Report-2024.pdf)).
- **Throughput ~$200bn–$210bn** annualised by late 2025 as it readied a public listing. ([TechCabal, Oct 2025](https://techcabal.com/2025/10/28/airtel-africas-mobile-money-processes-nearly-200bn-as-it-readies-public-listing/); [BusinessDay NG](https://businessday.ng/companies/article/airtel-africa-mobile-money-ipo-listing-on-track-as-transaction-hits-210bn/)).

---

## Lens 3 — Feature rollout & sequencing

### The shared playbook: USSD → app → merchant → open API → spun-off fintech arm
1. **USSD + agents** establish ubiquity on any handset (the base nobody can dislodge).
2. **Smartphone apps** add richer UX and cross-sell once data penetration rises.
3. **Merchant acceptance (MoMoPay)** monetises the payment graph beyond P2P.
4. **Open APIs** let third parties build on the rails — *lower cost to MTN, new revenue, broader use cases.* MTN deliberately launched in **Uganda (2018)** because it had early-stage innovators + an enabling ecosystem (accelerators, hubs); it even hired a local developer for in-person training and worked with universities. Adoption: by Jan 2020 (14 months in) ~**3m successful API calls**; by Aug 2021 **>50m**; **>900 partners live**, **>10,000–15,600 in sandbox**. ([CGAP — MTN opens APIs](https://www.cgap.org/blog/mtn-uganda-opens-mobile-money-apis); [CGAP — was it worth it](https://www.cgap.org/blog/mtn-mobile-money-opened-apis-was-it-worth-it)).
5. **Structurally separating the fintech arm** from the telco to raise capital and unlock value:
   - **Airtel** carved out **Airtel Mobile Commerce BV** and sold ~22.1% to **QIA, TPG/Rise Fund, Mastercard** (~$550m total) at a **$2.65bn** valuation in 2021, with a stated aim to **list the mobile money business within ~4 years**. ([Nairametrics, 2021](https://nairametrics.com/2021/07/30/airtel-africa-receives-200-million-for-its-mobile-money-operations-from-qia-at-a-2-65-billion-valuation/); [HSF Kramer](https://www.hsfkramer.com/insights/2021-08/herbert-smith-freehills-advises-airtel-africa-on-us200-million-investment-from-qia); [GPCA — TPG/QIA/Mastercard add $125m](https://www.globalprivatecapital.org/newsroom/tpg-qia-and-mastercard-invest-additional-usd125m-in-africas-airtel-mobile-commerce/)). By Sep 2025 the IPO was being pitched at **>$4bn**, with later reporting of a **$7–10bn** range (London preferred venue). ([TechCabal, Oct 2025](https://techcabal.com/2025/10/28/airtel-africas-mobile-money-processes-nearly-200bn-as-it-readies-public-listing/)).
   - **MTN** is separating its fintech units (starting Nigeria, Ghana, Uganda) into **MTN Group Fintech**, with **Mastercard** taking a minority stake at the **$5.2bn** valuation; expected to finalise ~H1 2025. ([TechCrunch, Aug 2023](https://techcrunch.com/2023/08/14/mastercard-to-purchase-a-minority-stake-in-mtns-5-2b-fintech-business/); [ITWeb](https://www.itweb.co.za/article/mastercard-buys-minority-stake-in-mtns-fintech-business/RgeVDMPRN12vKJN3)).

**Pattern (inference, High):** the fintech is deliberately structured as a **separate, separately-capitalised entity** with strategic payment partners (Mastercard) rather than left inside the telco P&L — the value-capture and capital-raising endgame of the mobile-money business.

---

## What it means for us

1. **USSD-first reach validates our phased feature-phone channel (High).** The entire $1.1tn African mobile-money economy runs on USSD + agents, not apps. Our smartphone-first MVP is the *minority* access pattern; the USSD channel is how we reach the mass market. The operators prove USSD is not a downgrade — it is the base layer. (Caveat: their USSD runs on *their own* SIM/shortcode; ours would need per-operator shortcode provisioning — already flagged in `offline-ussd-feasibility.md` as a lead-time item.)

2. **The collection-leg UX we assume is correct (High).** MoMo's three-state "request to pay" confirms the payer authorises the debit with **their own operator PIN, outside our app**. We don't (and can't) capture the payer's PIN — we trigger a collection and the operator prompts them. Design our flow around an async "pending → approved/rejected" state machine with callbacks, exactly as MoMo/pawaPay do.

3. **Operators are both our rails and our competitors (High).** Airtel Money (a DRC rail we'd depend on via pawaPay) is simultaneously building its *own* app, MoMoPay-style merchant acceptance, and cross-network ambitions. Our cross-network differentiator (pay *any* network from one app) is exactly what a single operator structurally *cannot* offer well — that's our wedge. But they can squeeze the economics of the legs we rent, and could undercut us within their own footprint.

4. **The money is in advances/lending and merchant fees, not P2P (Medium→High).** MTN's growth story is **BankTech (lending) + MoMoPay merchant + airtime advances**, not transfer fees (P2P is being competed toward zero; MoMoPay is just 0.5%). For us, **pure pass-through P2P is thin-margin by industry trajectory.** Durable value capture lives in adjacent services (merchant acceptance, credit, bill/airtime commissions) — all explicitly out of scope for v1, but this is where the operators show the margin is, so it should shape our post-MVP roadmap.

5. **The open-API model is our template for what pawaPay abstracts (High).** MoMo/Airtel expose collections + disbursements + remittance with free sandbox and **per-transaction fees deducted from a collections account**. This is precisely the cost structure to expect underneath our aggregator: a percentage/flat fee on each leg, deducted at settlement. Confirms our two-leg cost model (collect fee + payout fee per payment).

6. **Flag — operator-only capabilities (we are NOT an operator):** issuing the wallet/float, the agent cash-in/out network, USSD shortcodes tied to a SIM, airtime-data-as-collateral lending, and the regulatory e-money licence all sit with operators. We rent access to wallets via the aggregator; we do **not** hold balances (pure pass-through), so we avoid e-money/float regulation — but we also can't capture float income or lend on telco data. The fintech-spin-off capital-raise playbook (Mastercard/QIA/TPG at multi-$bn valuations) is an *operator/incumbent* endgame, not directly replicable by a thin pass-through layer — though it shows where strategic acquirers/investors place value.

---

## Notes / caveats / contradictions
- **MTN ≠ DRC.** Repeating because it matters: MTN MoMo is a *playbook* comparable, not a DRC rail. Our actual DRC operator legs are Vodacom M-Pesa, Airtel Money, Orange Money, Afrimoney. (Verified fact, High.)
- **Airtel Money valuation conflict:** $2.65bn (2021 QIA/TPG/Mastercard round, primary-ish) vs. ">$4bn" (Sep 2025 IPO pitch) vs. "$7–10bn" (later 2025/2026 reporting). These are different dates and different process stages (private round vs. pre-IPO marketing); not independently confirmed final figures. Treat pre-IPO ranges as **self-reported / press, Medium-Low**.
- **MoMo API per-transaction rates are NOT public** — "fee per transaction, set in contract schedule, varies by country/product." Specific DRC/aggregator rates: **Not found** (and MTN doesn't operate in DRC anyway). Our cost numbers must come from pawaPay/operator quotes, not these portals.
- **Some product/UX detail comes from MTN dev-community pages and country-specific (Uganda/Ghana) help pages** — capabilities may differ by country; labelled Medium where not in a financial filing.
- **"Active user" definitions differ** between MTN (~63m) and Airtel (~31.5m→41.5m) and may not be comparable; both are company-reported in investor materials (High for existence of figure, but definitions vary).
- Financial figures are FY2024 / 2025 interims as reported; **time-sensitive** — re-verify before any external use.

---

SOURCES_FOR_REGISTRY:
| cm-mtn-001 | What are MoMo Open APIs? — MoMo Dev Community | https://momodevelopercommunity.mtn.com/getting-started-in-the-community-2/what-are-momo-open-apis-107 | vendor docs | 2026-06-09 | Medium-High (primary vendor) | mtn-airtel-momo.md |
| cm-mtn-002 | MoMo API / Developer Portal — momo.mtn.com | https://momo.mtn.com/api/ | vendor docs | 2026-06-09 | Medium-High (primary vendor) | mtn-airtel-momo.md |
| cm-mtn-003 | MoMo pricing — momo.mtn.com | https://momo.mtn.com/pricing/ | vendor docs | 2026-06-09 | Medium (primary vendor, rates not disclosed) | mtn-airtel-momo.md |
| cm-mtn-004 | MTN Group FY2024 results complete booklet | https://www.mtn.com/wp-content/uploads/2025/03/MTN-Group-FY-24-results-complete-booklet-HR.pdf | investor filing | 2026-06-09 | High (primary) | mtn-airtel-momo.md |
| cm-mtn-005 | MTN: strong underlying 2024 performance (press release) | https://www.mtn.com/in-a-challenging-macro-mtn-group-reports-strong-underlying-2024-performance/ | company press | 2026-06-09 | High (primary, self-reported) | mtn-airtel-momo.md |
| cm-mtn-006 | MTN Fintech & Digital Services investor presentation | https://mtn-investor.com/mtn-cmd/pdf/presentations/fintech-and-digital-services.pdf | investor presentation | 2026-06-09 | High (primary, self-reported) | mtn-airtel-momo.md |
| cm-mtn-007 | Mastercard buys minority stake in MTN's $5.2B fintech business — TechCrunch | https://techcrunch.com/2023/08/14/mastercard-to-purchase-a-minority-stake-in-mtns-5-2b-fintech-business/ | tech press | 2026-06-09 | Medium-High | mtn-airtel-momo.md |
| cm-mtn-008 | CGAP — MTN Mobile Money Opened APIs: Was It Worth It? | https://www.cgap.org/blog/mtn-mobile-money-opened-apis-was-it-worth-it | research/NGO | 2026-06-09 | High (independent) | mtn-airtel-momo.md |
| cm-mtn-009 | CGAP — MTN Uganda Opens Mobile Money APIs | https://www.cgap.org/blog/mtn-uganda-opens-mobile-money-apis | research/NGO | 2026-06-09 | High (independent) | mtn-airtel-momo.md |
| cm-mtn-010 | MTN's MoMo Pay enters payments market with 0.5% fee — TechCabal | https://techcabal.com/2025/05/21/mtns-momo-pay-enters-the-payments-market-with-0-5-fee/ | tech press | 2026-06-09 | Medium-High | mtn-airtel-momo.md |
| cm-mtn-011 | MTN's fintech revenue growing thanks to airtime lending — TechCabal | https://techcabal.com/2025/11/04/mtn-fintech-revenue-2025-airtime-lending/ | tech press | 2026-06-09 | Medium-High | mtn-airtel-momo.md |
| cm-mtn-012 | MTN MoMo eyes simpler cross-border payments after $2.1bn surge — TechCabal | https://techcabal.com/2025/10/13/mtn-momo-eyes-simpler-cross-border-payments-after-2-1bn-surge/ | tech press | 2026-06-09 | Medium | mtn-airtel-momo.md |
| cm-mtn-013 | Airtel Africa plc Strategic Report 2024 (Annual Report) | https://cdn-webportal.airtelstream.net/website/investor/main/pdf/annual-report/Strategic-Report-2024.pdf | investor filing | 2026-06-09 | High (primary) | mtn-airtel-momo.md |
| cm-mtn-014 | Airtel Money processes nearly $200bn, readies 2026 listing — TechCabal | https://techcabal.com/2025/10/28/airtel-africas-mobile-money-processes-nearly-200bn-as-it-readies-public-listing/ | tech press | 2026-06-09 | Medium | mtn-airtel-momo.md |
| cm-mtn-015 | QIA invests $200m in Airtel mobile money at $2.65bn valuation — Nairametrics | https://nairametrics.com/2021/07/30/airtel-africa-receives-200-million-for-its-mobile-money-operations-from-qia-at-a-2-65-billion-valuation/ | press | 2026-06-09 | Medium | mtn-airtel-momo.md |
| cm-mtn-016 | HSF Kramer — Airtel Africa $200m QIA investment, $2.65bn valuation | https://www.hsfkramer.com/insights/2021-08/herbert-smith-freehills-advises-airtel-africa-on-us200-million-investment-from-qia | law firm advisory | 2026-06-09 | Medium-High | mtn-airtel-momo.md |
| cm-mtn-017 | Airtel Money Developer Portal | https://developers.airtel.africa/ | vendor docs | 2026-06-09 | Medium-High (primary vendor) | mtn-airtel-momo.md |
| cm-mtn-018 | Airtel Money launches Developer Portal / open APIs — The Condia | https://thecondia.com/airtel-money-developer-portal-mtn-chenosis-api/ | tech press | 2026-06-09 | Medium | mtn-airtel-momo.md |
| cm-mtn-019 | GSMA State of the Industry Report on Mobile Money 2025 | https://www.gsma.com/sotir/wp-content/uploads/2025/04/The-State-of-the-Industry-Report-2025_English.pdf | industry body | 2026-06-09 | High (primary industry) | mtn-airtel-momo.md |
| cm-mtn-020 | DRC accuses MTN of illegal operations (MTN not licensed in DRC) — Ecofin Agency | https://www.ecofinagency.com/news-digital/1202-52842-drc-accuses-mtn-of-illegal-operations-spotlighting-border-frequency-issues | press | 2026-06-09 | Medium | mtn-airtel-momo.md |
| cm-mtn-021 | RDC-Analyse — Feasibility analysis of Mobile Money in eastern DRC (Aug 2025) | https://rdc-analyse.org/files/2025/08/2025_08_CAT_Feasibility-analysis-of-Mobile-Money-in-eastern-DRC_VE-1.pdf | NGO/research | 2026-06-09 | Medium (regional) | mtn-airtel-momo.md |
| cm-mtn-022 | MTN & Airtel Money boost mobile money in Congo — Communications Africa | https://communicationsafrica.com/mobile/mtn-airtel-money-boost-mobile-money-in-congo | press | 2026-06-09 | Low-Medium | mtn-airtel-momo.md |
