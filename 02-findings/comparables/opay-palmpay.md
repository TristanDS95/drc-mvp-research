# Finding: OPay & PalmPay (Nigeria) — agent-network super-apps built on incentives

- **Topic / entity:** OPay and PalmPay — Nigerian fintech "super-apps"
- **Question it addresses:** How do these two companies structure their product, monetise, and sequence feature rollout? What lessons (and Nigeria-specific caveats) apply to our DRC pass-through interoperability app?
- **Date researched:** 2026-06-09
- **Confidence:** Medium overall (user/agent counts are largely company self-reported; revenue/profit are reported by media and may be partial; strategy narrative is well-corroborated)
- **Claim type:** mix — flagged inline per claim

---

## Why these two are relevant to us

OPay and PalmPay are the clearest modern example of fintechs that **won a mass market fast on incentives + agent distribution**, then **accreted a super-app** on top. They are useful contrast cases to our deliberately *focused* pass-through model (one job: let a merchant accept payment from a customer on any network). They also illustrate how a "free transfer" wedge gets funded — and what happens when the subsidy era ends and regulators tighten. Two important differences up front: both are **licensed deposit-taking institutions** (CBN Mobile Money Operator / microfinance-bank licences), not pure pass-through aggregators like our model on pawaPay; and both rely on a **bank-failure tailwind** that may not have a DRC analog.

---

## 1. Product structure & architecture

**Both are broad super-apps, not focused wallets.** PalmPay describes itself as integrating "money transfers, bill payments, credit services, and savings" plus investment/insurance, building **multiple access points: app, agents, USSD, and cards** ([PalmPay Nigeria site](https://www.palmpay.com/nigeria/); [TechAfrica News, 2024-03-12](https://techafricanews.com/2024/03/12/palmpay-launches-free-transfers-and-target-savings-features-to-drive-financial-inclusion-in-nigeria/)). OPay's surface spans transfers, airtime/data, electricity and bill pay, betting-wallet funding, savings (OWealth), microloans (OKash), and merchant/POS acquiring (*self-reported / corroborated by multiple profiles*, e.g. [Studentsdash history](https://studentsdash.com.ng/opay-from-ride-hailing-to-super-app-the-evolution-of-a-nigerian-fintech-powerhouse/)).

**The agent network is the core distribution asset — arguably more important than the app.** By 2023 OPay reported 500,000+ agents; PalmPay claims a similar or larger network (one source cites "over one million") ([TechCabal, 2025-08-25](https://techcabal.com/2025/08/25/opay-palmpay-lead-20trn-mobile-money-boom/)). Agents are physical cash-in/cash-out points and "the face of these companies in places where they do not have physical locations" ([TechCity explainer](https://www.techcityng.com/mobile-money-explained-how-opay-palmpay-mpesa-and-others-work/)) (*self-reported counts; role description corroborated*). This is the single biggest structural difference from a pure smartphone app: their reach into low-smartphone, cash-heavy areas runs through human agents and POS terminals, not the app alone.

**Three surfaces, not one:** consumer app + agent/POS network + cards. OPay and Moniepoint together distributed ~17 million cards in 2024; PalmPay targeted issuing 5 million cards by end-2025 ([TechCabal, 2025-08-25](https://techcabal.com/2025/08/25/opay-palmpay-lead-20trn-mobile-money-boom/)) (*company-reported targets*). Card issuance, POS terminals and USSD are deliberate channels to reach users who don't live in the app.

**Scale (mostly self-reported):** OPay reports 50M+ registered Nigerian users, ~10M daily active users, and ~100M daily transactions in 2024 ([OPay PRNewswire, May 2024](https://www.prnewswire.com/news-releases/opay-announces-its-first-monthly-profit-with-nearly-10-million-daily-active-trading-users-302142052.html); [TechCabal, 2025-08-25](https://techcabal.com/2025/08/25/opay-palmpay-lead-20trn-mobile-money-boom/)). PalmPay reports 40M users at its 6-year mark and ~15M transactions/day in Q1 2025, with an average ~50 transactions per user per month ([Nairametrics, 2025-09-16](https://nairametrics.com/2025/09/16/palmpay-celebrates-6-years-with-40-million-users/); [Nairametrics, 2025-05-08](https://nairametrics.com/2025/05/08/palmpay-hits-15-million-daily-transactions-in-q1-2025/)) (*company self-reported — treat as marketing-grade, not audited*).

---

## 2. Monetisation & value capture

**The headline consumer product is a loss leader.** PalmPay offers **unlimited free bank transfers** as an explicit acquisition tool ([TechCabal, 2024-03-08](https://techcabal.com/2024/03/08/palmpay-users-to-pay-zero-bank-fees-on-all-transactions-on-the-fintech-app/); [Daily Post, 2024-07-11](https://dailypost.ng/2024/07/11/palmpay-makes-payments-easier-with-zero-transfer-fees-on-all-transactions/)). Cashback is used the same way — e.g. up to ₦60 cashback on first daily airtime top-ups, plus coupons and "free transfer chances" ([PalmPay FAQ via Lendsqr](https://blog.lendsqr.com/frequently-asked-questions-about-palm-pay/)) (*verified from company/app material*).

**How "free" is funded — the key mechanism for us.** Per [Technext, 2025-11-11](https://technext24.com/2025/11/11/real-price-of-free-fintech-transfers/), free/subsidised transfers were "primarily covered by venture capital investors... and, to some extent, traditional banks absorbing the switching costs." In other words the wedge was **VC-subsidised**, not intrinsically profitable; the company then captures value on adjacent, higher-margin products (*media analysis — inference-grade but consistent across sources*).

**Where the money actually comes from:**
- **Float / interest on deposits + savings products.** PalmPay runs interest-bearing wallets (Cashbox, SmartEarn advertised at up to ~20–22% annualised) and reportedly **paid ₦4 billion in interest in 2024**, implying it held well over ₦18 billion in customer deposits on the wealth wallet alone ([TechCabal, 2025-08-25](https://techcabal.com/2025/08/25/opay-palmpay-lead-20trn-mobile-money-boom/)) (*media-reported / inference on deposit size*). Holding deposits is only possible because they hold deposit-taking licences — **we, as a pass-through app on rented rails, would not hold float.**
- **Lending / credit.** OPay (OKash) and PalmPay both offer credit, in PalmPay's case "in partnership with licensed lenders"; PalmPay reports 60% of credit borrowers had no prior financial account ([TechCrunch, 2025-06-05](https://techcrunch.com/2025/06/05/profitable-african-fintech-palmpay-is-in-talks-to-raise-as-much-as-100m/)) (*company-reported*). Lending monetises the user base and the data the wallet generates.
- **Agent / float economics + POS acquiring.** Agents earn commissions on cash-in/cash-out and POS withdrawals; "some agents transact only because commissions exist" ([Legit.ng](https://www.legit.ng/business-economy/industry/1713124-nigerias-banks-fight-fintech-giants-face-tougher-rules-digital-payments-war/)). OPay's 500,000+ agents reportedly moved ~₦1.8 trillion (~$2.4bn) in cash flows in 2025 ([TechCabal, 2025-08-25](https://techcabal.com/2025/08/25/opay-palmpay-lead-20trn-mobile-money-boom/)). POS terminals are increasingly **leased** (company keeps ownership, agent must hit volume targets) as FX/inflation doubled terminal costs 2023–2025 ([Legit.ng, PoS prices](https://www.legit.ng/business-economy/industry/1676888-pos-prices-double-moniepoint-opay-palmpay-banks-raise-cost-terminals-amid-fx-crunch/)) (*media-reported*).
- **Airtime/bill commissions and card issuance** round out the take (*self-reported across profiles*).

**Profitability turn.** OPay announced its **first monthly profit in May 2024** alongside ~10M DAU ([OPay PRNewswire, May 2024](https://www.prnewswire.com/news-releases/opay-announces-its-first-monthly-profit-with-nearly-10-million-daily-active-trading-users-302142052.html)). PalmPay reported **2023 revenue ~$64M** and is described as profitable and in talks to raise up to $100M in 2025 ([TechCrunch, 2025-06-05](https://techcrunch.com/2025/06/05/profitable-african-fintech-palmpay-is-in-talks-to-raise-as-much-as-100m/)) (*media-reported; OPay's full revenue is undisclosed*). Valuations: OPay ~$2.75–3bn by April 2024 and reportedly eyeing a ~$4bn US IPO; PalmPay reportedly ~$1.5bn unicorn by early 2025 ([TechCabal, 2024-04-29](https://techcabal.com/2024/04/29/opay-valuation-nears-3billion/); [PYMNTS, 2026](https://www.pymnts.com/news/ipo/2026/softbank-backed-opay-eyes-4-billion-valuation-in-us-ipo/)) (*media-reported*).

The pattern: **acquire cheaply on free transfers + agent reach, then monetise via float/interest, lending, POS acquiring, and ancillary commissions** — almost none of which a pure pass-through app captures.

---

## 3. Feature rollout & sequencing

**OPay — pivoted *into* fintech, then accreted.** Launched Aug 2018 by Opera (Yahui Zhou) as a payments app (airtime, bills, food). In 2019 it blitz-scaled a **ride-hailing/logistics super-app** — ORide (bikes), OCar, OBus, OTrike, OFood — on heavy subsidy ([Studentsdash](https://studentsdash.com.ng/opay-from-ride-hailing-to-super-app-the-evolution-of-a-nigerian-fintech-powerhouse/); [Afridigest](https://afridigest.substack.com/p/operas-opay-optimizes-its-operations)). In **2020 it shut ORide/OCar/OExpress/OFood** and refocused entirely on payments and merchant acquiring (*corroborated across profiles*). Lesson embedded here: even a well-funded super-app **killed the parts that didn't work** and concentrated on the payments core.

**PalmPay — distribution-first from day one.** Launched Nov 2019 in Lagos on a **$40M seed led by Transsion** (Tecno/Infinix/Itel parent; with NetEase, Mediatek). Its signature growth lever was **pre-installation on Transsion phones** — ~20M handsets in 2020 — giving instant smartphone distribution, layered with a large agent network ([TechCrunch, 2019-11-12](https://techcrunch.com/2019/11/12/palmpay-launches-in-nigeria-on-40m-round-led-by-chinas-transsion/); [TechCabal, 2019-11-15](https://techcabal.com/2019/11/15/40-million-from-tecno-and-a-visa-partnership-palmpays-bold-nigeria-play/)). It sequenced free transfers + target savings (2024) and is now pushing cards and African expansion (Ghana, Kenya, then South Africa, Côte d'Ivoire, Uganda, Tanzania) ([TechCabal, 2025-05-08](https://techcabal.com/2025/05/08/palmpay-expansion/)).

**The decisive tailwind: Nigeria's cash crunch + bank failures.** The CBN's Dec-2022 naira redesign (old notes to be phased out by Jan/Feb 2023) triggered a cash shortage; bank apps crashed and transfers failed, and Nigerians flooded to OPay/PalmPay, which had more reliable rails and agent cash access. By Oct 2023 OPay was Nigeria's most-downloaded app ([Businessday, naira crunch](https://businessday.ng/business-economy/article/nigerians-flock-to-opay-palmpay-others-amid-naira-crunch/); [Ecofin Agency](https://www.ecofinagency.com/insights/0809-48489-in-nigerian-bank-technology-failures-pushed-opay-and-palmpay-to-leadership-in-daily-payments)). Mobile-money value hit ₦20.71tn ($13.49bn) in Q1 2025, up >1,500% from Q1 2021 ([TechCabal, 2025-08-25](https://techcabal.com/2025/08/25/opay-palmpay-lead-20trn-mobile-money-boom/)) (*verified from cited regulator-derived figures, though magnitudes are large*). The growth curve was as much **macro/regulatory accident** as product genius.

**Regulatory context (Nigeria-specific).** Both operate under **CBN licences** (microfinance-bank/MMO; recently upgraded to *national* status, allowing nationwide operation) ([Businessday, licence upgrade](https://businessday.ng/technology/article/cbn-upgrades-licences-of-opay-moniepoint-kuda-other-fintechs-to-national-status/)). The flip side: in **April 2024 the CBN ordered OPay, PalmPay, Kuda, Moniepoint and Paga to halt new-customer onboarding** over KYC/AML and illicit-FX concerns; the freeze was lifted in **June 2024** after compliance reviews, and NIN-linking of accounts was enforced ([TechCabal, 2024-04-29](https://techcabal.com/2024/04/29/exclusive-cbn-directs-four-fintechs-to-stop-onboarding/); [Techpoint, 2024-06-03](https://techpoint.africa/2024/06/03/cbn-lifts-ban-on-new-account-opening/)). Compliance/fines became material (CBN fined Moniepoint and OPay ~$634k in one Q2 action) ([ThePaypers](https://thepaypers.com/digital-identity-security-online-fraud/cbn-fines-moniepoint-and-opay-usd-633990-in-q2--1271423)). And the "free" era is ending: Nigeria extended the electronic transfer levy to fintechs, so from 2026 a ₦50,000 transfer carries a ₦50 levy ([TechCabal, 2025-12-22](https://techcabal.com/2025/12/22/why-your-50000-transfer-will-cost-100-from-2026/)) (*verified, media-reported*).

---

## What it means for us

1. **The agent network is the deepest moat — and the part hardest to copy on rented rails.** OPay/PalmPay's reach into cash-heavy, low-smartphone areas runs through 500k+ human agents and POS terminals, not the app. Our pass-through model has **no agent layer and holds no float**, so we cannot replicate this moat or its float/interest economics. If reaching non-urban/feature-phone DRC users matters (and our scope says it does), the honest read is that **distribution, not the app, is the hard problem** — and our USSD channel is a partial substitute for, not an equal of, a physical agent network.

2. **Incentive-driven growth works but is VC-subsidised and fragile.** Free transfers + cashback demonstrably acquire users fast, but sources are explicit that the cost was carried by investors and bank switching-cost absorption, not unit economics ([Technext](https://technext24.com/2025/11/11/real-price-of-free-fintech-transfers/)). For us — **thinner margins (we pay aggregator fees on both legs) and no float/lending to backfill** — a free-transfer land-grab is far riskier. We should treat any subsidy as a time-boxed acquisition cost with an explicit path to a fee or adjacent revenue, not a permanent feature.

3. **Monetisation lives in the products we *don't* have.** Their profit comes from float/interest, lending, POS acquiring, and card issuance — all gated behind a deposit-taking licence and an agent/merchant network. A pure pass-through wallet's realistic levers are **a transfer/convenience fee, FX/cross-network spread, and possibly later bill-pay/merchant commissions** — much narrower. This argues for **charging modestly from early on** (our model can't subsidise indefinitely) rather than copying the free-transfer playbook.

4. **Super-app accretion vs focus: their own history endorses focus first.** OPay built a sprawling ride-hailing super-app and then **shut most of it** to concentrate on payments; PalmPay won by nailing distribution + a simple free-transfer hook before layering savings/cards. The transferable lesson is **start narrow, prove the core money-movement loop, then accrete** — which aligns with our "focused interoperability wedge, defer bill-pay/rewards" decision.

5. **Sequencing lesson: distribution hacks beat feature breadth early.** PalmPay's single biggest lever was **phone pre-installation** (Transsion), and both leaned on agents. We have no handset-OEM lever, but the principle — find a distribution channel that puts you in front of users cheaply before broadening features — should shape our go-to-market more than feature planning does.

### Nigeria-specific caveats (may not transfer to DRC)
- **The cash-crunch / bank-failure tailwind was a one-off macro event.** Much of the 2023–2025 surge was the naira redesign + failing bank rails, not pure product pull. DRC has no obvious equivalent trigger; we should not assume comparable explosive adoption.
- **Licensing model differs.** OPay/PalmPay are licensed deposit-takers (CBN MMO/MFB). Our model is pass-through on someone else's rails; their float/lending economics are **structurally unavailable** to us. (Our own licensing remains an item flagged for investigation.)
- **Scale and regulatory regime differ.** Nigeria's market size, NIBSS rails, NIN identity backbone, transfer-levy regime, and CBN onboarding freezes are Nigeria-specific. DRC's regulator, identity infrastructure, and operator landscape are different and must be researched separately.
- **Counts are self-reported.** User/agent/transaction figures here are largely company marketing; treat as directional, not audited.

## Notes / caveats / contradictions
- **Agent-count conflict:** PalmPay is cited at both "~500,000" and "over one million" across sources/years; OPay at "500,000+" (2023). Likely reflects growth over time and marketing inflation — flagged, not resolved.
- **OPay revenue undisclosed**; only PalmPay's 2023 figure (~$64M) is in the public record, and both companies' "profitability" claims are self-/media-reported, not independently audited.
- "CrediboPalm/BNPL" (named in the research prompt) was **not found** as a PalmPay product; PalmPay credit appears to run via licensed-lender partnerships. Recorded as not-found rather than guessed.

## Sources

| ID | Title | URL | Type | Date | Reliability |
|----|-------|-----|------|------|-------------|
| cm-op-001 | OPay, PalmPay cash in: Inside ₦20.7tn mobile money rush (TechCabal) | https://techcabal.com/2025/08/25/opay-palmpay-lead-20trn-mobile-money-boom/ | secondary | 2026-06-09 | high |
| cm-op-002 | Bank tech failures pushed OPay & PalmPay to leadership (Ecofin) | https://www.ecofinagency.com/insights/0809-48489-in-nigerian-bank-technology-failures-pushed-opay-and-palmpay-to-leadership-in-daily-payments | secondary | 2026-06-09 | med |
| cm-op-003 | PalmPay launches free transfers & target savings (TechAfrica) | https://techafricanews.com/2024/03/12/palmpay-launches-free-transfers-and-target-savings-features-to-drive-financial-inclusion-in-nigeria/ | secondary | 2026-06-09 | med |
| cm-op-004 | PalmPay users to pay zero bank fees (TechCabal) | https://techcabal.com/2024/03/08/palmpay-users-to-pay-zero-bank-fees-on-all-transactions-on-the-fintech-app/ | secondary | 2026-06-09 | high |
| cm-op-005 | OPay first monthly profit, ~10M daily active users (PR Newswire) | https://www.prnewswire.com/news-releases/opay-announces-its-first-monthly-profit-with-nearly-10-million-daily-active-trading-users-302142052.html | self-reported | 2026-06-09 | med |
| cm-op-006 | OPay valuation nears $3bn (TechCabal) | https://techcabal.com/2024/04/29/opay-valuation-nears-3billion/ | secondary | 2026-06-09 | med |
| cm-op-007 | PalmPay in talks to raise up to $100M (TechCrunch) | https://techcrunch.com/2025/06/05/profitable-african-fintech-palmpay-is-in-talks-to-raise-as-much-as-100m/ | secondary | 2026-06-09 | high |
| cm-op-008 | PalmPay launches on $40M round led by Transsion (TechCrunch) | https://techcrunch.com/2019/11/12/palmpay-launches-in-nigeria-on-40m-round-led-by-chinas-transsion/ | secondary | 2026-06-09 | high |
| cm-op-009 | CBN directs four fintechs to stop onboarding (TechCabal) | https://techcabal.com/2024/04/29/exclusive-cbn-directs-four-fintechs-to-stop-onboarding/ | secondary | 2026-06-09 | high |
| cm-op-010 | CBN lifts ban on new account opening (Techpoint) | https://techpoint.africa/2024/06/03/cbn-lifts-ban-on-new-account-opening/ | secondary | 2026-06-09 | high |
| cm-op-011 | The real price of free transfers (Technext) | https://technext24.com/2025/11/11/real-price-of-free-fintech-transfers/ | secondary | 2026-06-09 | med |
| cm-op-012 | Why your ₦50,000 transfer could cost ₦100 from 2026 (TechCabal) | https://techcabal.com/2025/12/22/why-your-50000-transfer-will-cost-100-from-2026/ | secondary | 2026-06-09 | high |
| cm-op-014 | PalmPay celebrates 6 years with 40M users (Nairametrics) | https://nairametrics.com/2025/09/16/palmpay-celebrates-6-years-with-40-million-users/ | secondary | 2026-06-09 | med |
| cm-op-015 | PalmPay hits 15M daily transactions Q1 2025 (Nairametrics) | https://nairametrics.com/2025/05/08/palmpay-hits-15-million-daily-transactions-in-q1-2025/ | secondary | 2026-06-09 | med |
| cm-op-017 | CBN upgrades licences of OPay, Moniepoint, Kuda to national (BusinessDay) | https://businessday.ng/technology/article/cbn-upgrades-licences-of-opay-moniepoint-kuda-other-fintechs-to-national-status/ | secondary | 2026-06-09 | med |
| cm-op-019 | PalmPay to expand into four African markets (TechCabal) | https://techcabal.com/2025/05/08/palmpay-expansion/ | secondary | 2026-06-09 | high |
| cm-op-021 | OPay eyes $4bn valuation for US IPO (PYMNTS) | https://www.pymnts.com/news/ipo/2026/softbank-backed-opay-eyes-4-billion-valuation-in-us-ipo/ | secondary | 2026-06-09 | med |

_(16 load-bearing rows of the agent's 22-source registry; full set in the run transcript.)_
