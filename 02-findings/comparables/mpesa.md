# Comparable profile: M-Pesa (Safaricom, Kenya)

- **Topic / entity:** M-Pesa — Safaricom (Kenya), with Vodacom group context (Tanzania, DRC, etc.)
- **Question it addresses:** How a comparable fintech is structured, monetised, and rolled out — to inform our DRC consumer cross-network pass-through MVP and our phased feature-phone/USSD channel.
- **Date researched:** 2026-06-09
- **Confidence:** High on the broad arc and most figures; Medium on a few self-reported metrics and the exact USSD-vs-app channel split (flagged inline).
- **Claim type:** mix — labelled inline as *[verified]*, *[self-reported]*, or *[inference]*.

M-Pesa is the canonical operator-led mobile-money story: a USSD/SIM-toolkit P2P transfer service (2007) that grew into a merchant-acceptance network, a savings/lending platform, and now a super-app. It is the single most relevant precedent for "money tied to a phone number, no bank account," and it usefully contrasts with our model on the two dimensions we care most about: **it holds funds (a wallet) and earns on cash-out**, whereas we are **pure pass-through on rented rails**.

---

## Lens 1 — Product structure & architecture

**Origins: USSD / SIM-toolkit, not an app.** M-Pesa launched commercially **6 March 2007** *[verified — [Wikipedia: M-Pesa](https://en.wikipedia.org/wiki/M-Pesa); [World Bank case study](https://documents1.worldbank.org/curated/en/638851468048259219/pdf/543380WP0M1PES1BOX0349405B01PUBLIC1.pdf)]*. It was conceived (2003, via a £1m DFID matching grant to Vodafone exec Nick Hughes) as a way for microfinance customers to repay loans by phone; market testing **pivoted the core use case to P2P transfers** to friends and family before launch *[verified — [Wikipedia: M-Pesa](https://en.wikipedia.org/wiki/M-Pesa)]*. Crucially, it required **no smartphone and no data** — it ran on SMS + a USSD/SIM-toolkit menu (today `*334#` in Kenya, originally `*140#`/`*400#`-era codes) accessible on any handset *[verified — [Wikipedia: M-Pesa](https://en.wikipedia.org/wiki/M-Pesa); [paybillke USSD](https://paybillke.com/ussd/safaricom-mpesa-main)]*. This is the most transferable structural fact for us: **the franchise was built on the lowest-common-denominator channel first**, and the app came 14 years later.

**The agent network is the physical layer.** M-Pesa's defining infrastructure is the cash-in/cash-out (CICO) agent network — small shops where users convert physical cash to/from e-money. By December 2024 Safaricom reported **~381,000 agents** in Kenya managing ~82 million accounts *[self-reported via press — [Capital FM](https://www.capitalfm.co.ke/business/2026/03/m-pesa-adds-6mn-new-users-to-reach-40mn/); footprint larger than all Kenyan banks combined per [Telecompaper/secondary]]*. Agents buy **e-float** from Safaricom/super-agents and earn commission on each deposit/withdrawal; the agent's float is debited on cash-out and credited on cash-in *[verified mechanics — [KopaCash](https://kopacash.com/blog/how-do-mpesa-agents-make-money/); [Safaricom agents page](https://www.safaricom.co.ke/main-mpesa/for-your-business/m-pesa-agents-and-dealers)]*. **Relevance to us:** we are pass-through and ride pawaPay; we own **no agents and no float**. The CICO layer is exactly the heavy, capital-intensive thing rented rails let us skip — but it's also why M-Pesa can monetise (see Lens 2). We inherit operators' existing CICO indirectly, not as our own asset.

**Merchant acceptance came as distinct products, layered on top of P2P:**
- **Pay Bill** — a biller/aggregator collection rail (utilities, schools, etc.), where customers enter a business number + account ref. Long-standing (rolled out ~2009-era) *[Medium/secondary; exact date not confirmed in primary source — see "open questions"]*.
- **Lipa Na M-Pesa "Buy Goods" (Till Number)** — branded merchant acceptance, **launched 2013**, the "pay the merchant" product analogous to our wedge *[verified arc — [Wikipedia: M-Pesa](https://en.wikipedia.org/wiki/M-Pesa); secondary timelines]*.
- **Pochi la Biashara** — a lightweight product letting **informal micro-merchants** (food vendors, kiosks, boda-boda riders) receive payments **on their personal M-Pesa number** while separating business from personal funds *[verified — [Safaricom Pochi FAQ](https://www.safaricom.co.ke/media-center-landing/frequently-asked-questions/pochi-la-biashara)]*. **This is the closest analog to our v1**: no formal merchant onboarding, pay an existing phone number. M-Pesa eventually built it as a *product* precisely because "pay a person's number" was already how informal commerce worked.

**The super-app came late and on top of everything.** The **M-PESA Super App launched June 2021** (consumer) alongside an **M-PESA for Business app**, ~14 years after launch *[verified — [TechTrends KE](https://techtrendske.co.ke/2021/06/23/safaricom-m-pesa-super-app-is-now-official/)]*. It added spending insights, statements, **"Offline Mode"** (transactions without data), and **Mini-Apps** — third-party services (SGR train tickets, BuuPass buses, insurance via eBima, gas delivery) embedded inside the app *[verified — [TechTrends KE](https://techtrendske.co.ke/2021/06/23/safaricom-m-pesa-super-app-is-now-official/); [Potentash](https://potentash.com/2021/06/24/mpesa-super-app-new-features/)]*. In **April 2026** Safaricom merged M-PESA and MySafaricom into a single AI-assisted **"My OneApp"** *[verified event — [The Star](https://www.the-star.co.ke/news/2026-04-01-safaricom-merges-m-pesa-and-mysafaricom-under-one-roof); execution criticised by [Moses Kemibaro](https://moseskemibaro.com/2026/04/07/safaricoms-m-pesa-my-oneapp-has-arrived-a-brilliant-strategy-undermined-by-a-poor-execution/)]*.

**Channels — USSD still carries the base, app is a thinner slice.** USSD historically initiated the large majority of mobile-money transactions in Kenya (one secondary source cites **>90%** in the early growth period) *[secondary/historical — [hSenid Mobile](https://hsenidmobile.com/ussd-vs-mobile-apps-for-financial-services-what-scales-better-in-emerging-markets/)]*. As of the 2026 OneApp coverage, the **standalone M-PESA App had ~6.7M users** vs ~40M total M-Pesa customers — i.e., the **app is a minority surface; USSD + SIM-toolkit remain the universal default** *[self-reported user counts via [Moses Kemibaro](https://moseskemibaro.com/2026/04/07/safaricoms-m-pesa-my-oneapp-has-arrived-a-brilliant-strategy-undermined-by-a-poor-execution/); 40M total via [techweez](https://techweez.com/2026/03/06/safaricom-40-million-m-pesa-customers/)]*. **An exact current USSD-vs-app transaction split is not publicly disclosed** *[Not found — searched Safaricom press, Statista listings, Wikipedia]*. **Relevance:** even the continent's most app-capable wallet still runs the bulk of activity on USSD — strong support for treating our USSD channel as first-class, not an afterthought.

**Held balance vs pass-through.** M-Pesa is a **stored-value wallet** (regulated e-money; float held in trust). That stored balance is what enables withdrawal fees, savings, and overdraft. **We are pass-through and never hold funds** — so several of M-Pesa's richest monetisation lines (cash-out fees, float, lending off the balance) are *structurally unavailable to us in v1*. Note this is the key non-transferable difference.

---

## Lens 2 — Monetisation & value capture

**Scale of the prize.** M-Pesa contributed **KES 139.9bn (~42% of Safaricom service revenue) in FY2024**, growing to **KES 161.1bn (~44%) in FY2025** *[self-reported via results press — [Kenyan Wall Street](https://kenyanwallstreet.com/m-pesa-boosts-safaricoms-full-year-2024-earnings-despite-ethiopia-losses); [Safaricom press release, May 2025](https://www.safaricom.co.ke/media-center-landing/press-releases/safaricoms-revenue-tops-3-billion-dollars-a-first-in-the-region-as-the-transition-to-become-a-purpose-led-technology-pays-off)]*. Mobile money is no longer a feature — it's nearly half the company.

**Where the money actually comes from:**

| Line | Who pays | Mechanic | Notes for us |
|---|---|---|---|
| **Cash-out / withdrawal** | Consumer | Tiered fee, **~KES 11–309** at agents (KES 50–250,000 bands); ATM ~KES 35–203 | *[verified bands — [Safaricom tariffs (consumer)](https://www.safaricom.co.ke/main-mpesa/m-pesa-for-you/tariffs-limits/consumer-tariffs-limits); [BitValve summary](https://www.bitvalve.com/blog/mpesa-charges-2024/)]*. **Historically the core revenue engine.** Requires holding the balance — **N/A to pass-through.** |
| **P2P send** | Sender | **Free KES 1–100** to registered users; tiered above, **capped ~KES 108** (bands to KES 250,000) | *[verified — [PesaTrail](https://pesatrail.com/mpesa-charges-2026.html); [Safaricom tariffs](https://www.safaricom.co.ke/main-mpesa/m-pesa-for-you/tariffs-limits/consumer-tariffs-limits)]*. Sends to **unregistered** users were **discontinued 5 Feb 2024** (fraud/AML) *[verified — [BitValve](https://www.bitvalve.com/blog/mpesa-charges-2024/)]*. |
| **Lipa Na M-Pesa Buy Goods (merchant)** | **Merchant** (customer free) | Merchant pays **max 0.5%, capped KES 200/txn**; collections ≤KES 200 free; customer pays **nothing** | *[verified — [mpesa.or.ke Buy Goods charges](https://mpesa.or.ke/guides/lipa-na-mpesa-buy-goods-business-till-charges/)]*. Fee was **cut from 1% → 0.5%** to drive adoption *[verified intent — same source]*. **Fuel stations are an exception — customer pays.** |
| **Pay Bill (billers)** | Mixed (often biller; some pass to customer) | Aggregator/biller collection fees | *[verified product, mixed fee model — [mpesa.or.ke](https://mpesa.or.ke/guides/lipa-na-mpesa/)]* |
| **Lending — Fuliza (overdraft)** | Borrower | 1% one-off access fee + **daily fee KES ~5–30** by band; underwritten by **NCBA & KCB** | *[verified — [Standard Media](https://www.standardmedia.co.ke/business/financial-standard/article/2001430559/move-over-m-shwari-and-kcb-m-pesa-fuliza-now-the-new-lending-king); [Safaricom/NCBA/KCB press](https://www.safaricom.co.ke/media-center-landing/press-releases/safaricom-ncba-and-kcb-restructure-fuliza-with-free-daily-fees)]*. |
| **Savings + micro-loans — M-Shwari** | Saver/borrower | Save from KES 1 at ~2–5% p.a.; loans KES 100–20,000, 30 days, **7.5% facilitation fee**; underwritten by **NCBA (ex-CBA)** | *[verified — [CGAP "How M-Shwari Works"](https://www.cgap.org/sites/default/files/Forum-How-M-Shwari-Works-Apr-2015.pdf); [NCBA M-Shwari](https://ke.ncbagroup.com/loan/m-shwari/)]*. |
| **KCB M-Pesa** | Saver/borrower | Bank-partner savings/loan product, same in-menu pattern | *[verified existence — [Safaricom credit & savings](https://www.safaricom.co.ke/main-mpesa/m-pesa-services/credit-and-savings)]* |

**The strategic pricing move worth copying-in-spirit:** M-Pesa makes **customers pay to send and to cash out, but lets them pay merchants for free**, putting the merchant-acceptance fee on the merchant. This deliberately nudges users toward staying *inside* e-money (digital merchant payment) and away from cash-out — keeping float in the system and growing the acceptance network *[verified pricing structure + stated intent — [mpesa.or.ke Buy Goods](https://mpesa.or.ke/guides/lipa-na-mpesa-buy-goods-business-till-charges/); [CGAP on Lipa charges](https://www.cgap.org/blog/fixing-hidden-charges-in-lipa-na-m-pesa)]*.

**Lending is now a major, fast-growing line.** **Fuliza** became the dominant digital-lending product: it transacts **>KES 1.2bn/day**, recorded **~KES 4.5bn revenue (+61% YoY)** in a recent year with **~1.4M daily active users**, and cumulative disbursements reported at **~KES 1.47tn** *[self-reported / press — [Standard Media](https://www.standardmedia.co.ke/business/financial-standard/article/2001430559/move-over-m-shwari-and-kcb-m-pesa-fuliza-now-the-new-lending-king); [People Daily](https://peopledaily.digital/business/fuliza-powers-safaricoms-lending-growth-as-disbursements-hit-ksh1-47t)]*. **Key structural point:** Safaricom does **not** balance-sheet the credit risk — **banks (NCBA, KCB) underwrite**, Safaricom contributes the distribution, data, and rails and shares revenue *[verified partnership structure — [Standard Media](https://www.standardmedia.co.ke/business/financial-standard/article/2001430559/move-over-m-shwari-and-kcb-m-pesa-fuliza-now-the-new-lending-king)]*. The credit *score* is built from Safaricom airtime/M-Pesa usage history *[verified — [CGAP](https://www.cgap.org/sites/default/files/Forum-How-M-Shwari-Works-Apr-2015.pdf)]*.

---

## Lens 3 — Feature rollout & sequencing

**The product line, in order** *[arc verified across [Wikipedia: M-Pesa](https://en.wikipedia.org/wiki/M-Pesa) + dated press below]*:

1. **2007 — P2P send + cash-in/cash-out** on USSD/SIM-toolkit. The wedge was "send money home." Year-1 plan was 350k customers; it hit **~1.2M** *[verified — [World Bank case study](https://documents1.worldbank.org/curated/en/638851468048259219/pdf/543380WP0M1PES1BOX0349405B01PUBLIC1.pdf)]*.
2. **~2009 — Pay Bill** (bill payment / biller collections) — broadened use beyond person-to-person *[date approximate; secondary]*.
3. **2012 (Nov) — M-Shwari** (savings + micro-loans, with CBA/now-NCBA). First move into financial services *on top of* the payment rail *[verified — [AFI](https://afi-global.org/news/safaricom-cba-launch-groundbreaking-mobile-banking-service-m-shwari/); [CGAP](https://www.cgap.org/sites/default/files/Forum-How-M-Shwari-Works-Apr-2015.pdf)]*.
4. **2013 — Lipa Na M-Pesa Buy Goods** (branded merchant acceptance / tills) *[verified arc — [Wikipedia](https://en.wikipedia.org/wiki/M-Pesa)]*.
5. **2019 (8 Jan) — Fuliza** (overdraft) — let any transaction complete on a zero balance, deepening lending *[verified — [Wikipedia](https://en.wikipedia.org/wiki/M-Pesa)]*.
6. **2020 — Pochi la Biashara** (informal-merchant receive-on-personal-number) *[verified product — [Safaricom FAQ](https://www.safaricom.co.ke/media-center-landing/frequently-asked-questions/pochi-la-biashara)]*.
7. **2021 (Jun) — M-PESA Super App + Mini-Apps**; **2026 (Apr) — "My OneApp"** consolidation *[verified — [TechTrends KE](https://techtrendske.co.ke/2021/06/23/safaricom-m-pesa-super-app-is-now-official/); [The Star](https://www.the-star.co.ke/news/2026-04-01-safaricom-merges-m-pesa-and-mysafaricom-under-one-roof)]*.

**What unlocked each step (inference, grounded in the sources):**
- P2P → wide adoption only because the **agent CICO network + USSD ubiquity** reached unbanked, feature-phone users.
- The **transaction-history data** generated by P2P/bill-pay is what made **algorithmic credit scoring** (M-Shwari, Fuliza) possible — *the data exhaust of payments funded the lending business* *[inference from [CGAP](https://www.cgap.org/sites/default/files/Forum-How-M-Shwari-Works-Apr-2015.pdf)]*.
- Merchant acceptance (Lipa/Pochi) and the **free-to-customer Buy Goods** pricing kept money digital, which **grows float and acceptance density** — the precondition for a super-app worth opening daily.

**Geographic sequencing.** Vodacom replicated the model country-by-country: **Tanzania 2008** (8.2M active by 2018; >22M more recently), **DRC 2012** (reported ~6M customers), plus Lesotho, Mozambique, Ghana, Egypt; Vodacom group financial-services customers crossed **~103M (Mar 2026)** *[self-reported / press — [WeeTracker](https://weetracker.com/2025/11/28/vodacom-tanzania-launches-m-pesa-global/); [Mobile Europe (DRC)](https://www.mobileeurope.co.uk/vodacom-launches-m-pesa-rallonge-in-democratic-republic-of-congo/); [tech.africa Vodacom FY2026](https://tech.africa/vodacom-5g-mpesa-fy2026/)]*. **DRC relevance:** Vodacom M-Pesa is **already a live network in our market** — it is one of the wallets our pass-through app must reach, not a competitor we displace. (Detailed DRC M-Pesa API/availability work belongs in the operator findings, not here.)

---

## What it means for us

Concrete, transferable lessons for the DRC pass-through MVP and the phased USSD channel:

1. **USSD-first is validated at the highest level.** Even the most app-mature wallet on the continent still runs the *majority* of activity on USSD/SIM-toolkit, with the app a minority surface (~6.7M of ~40M users). **Treat our feature-phone/USSD channel as first-class, and don't assume the smartphone app is where the volume will live** *[inference grounded in channel data above]*.

2. **"Pay an existing phone number, no merchant onboarding" is a proven entry pattern — Pochi la Biashara is the precedent.** M-Pesa built a dedicated product around exactly our v1 mechanic (receive on a personal number, no formal merchant signup). It validates the wedge *and* signals the later upgrade path (separate-the-business-funds, then formal tills) *[verified — [Safaricom Pochi FAQ](https://www.safaricom.co.ke/media-center-landing/frequently-asked-questions/pochi-la-biashara)]*.

3. **Our biggest monetisation lever is structurally different — plan accordingly.** M-Pesa's richest lines (cash-out fees, float, balance-based lending) **require holding funds**, which **pass-through on pawaPay does not do**. We cannot copy the cash-out-fee engine. Our realistic v1 levers are a **fee/markup on the transaction itself** (the spread between collect and payout legs / a service fee) and possibly **FX or cross-network bridging margin** — *not* withdrawal fees *[inference; verify our actual pawaPay cost stack in the aggregator findings before pricing]*.

4. **Put the fee where it least suppresses usage — and consider "free to pay" as growth fuel.** M-Pesa deliberately makes **merchant payment free to the customer** (merchant bears ≤0.5%) and even cut that fee to drive adoption. For us, the lesson is to think hard about **who bears our fee**: a payer-pays model taxes exactly the action we want to grow. If we can structure the take on a less elastic party (or absorb it early to build habit), adoption compounds faster *[verified pricing logic — [mpesa.or.ke](https://mpesa.or.ke/guides/lipa-na-mpesa-buy-goods-business-till-charges/); [CGAP](https://www.cgap.org/blog/fixing-hidden-charges-in-lipa-na-m-pesa)]*. (Caveat: M-Pesa could afford "free" because it earned on cash-out elsewhere; we have no such cross-subsidy in v1.)

5. **Payments generate the data that funds the next business.** M-Shwari/Fuliza lending was built on the **transaction-history exhaust** of payments, with **banks carrying the credit risk** and M-Pesa monetising distribution + data. **For us:** even as pure pass-through, the transaction graph we accumulate is a future asset (credit, insurance, premium features) — and the **partner-the-balance-sheet** structure (let a licensed partner underwrite, we provide rails + data) is a capital-light path that fits a non-bank like us *[inference grounded in [CGAP](https://www.cgap.org/sites/default/files/Forum-How-M-Shwari-Works-Apr-2015.pdf) + Fuliza partnership structure]*.

6. **Sequence narrowly; let the surface grow only after the rail is dense.** M-Pesa shipped one thing (P2P) for years before bill-pay, then merchant, then lending, and the **super-app came 14 years in**. Resist super-app temptation: nail cross-network pay-a-number, get density, *then* layer.

**Flagged as Kenya-specific / may not transfer:**
- **Near-monopoly distribution.** Safaricom's ~65% mobile market share and a CICO network larger than all banks combined gave M-Pesa network effects we will not have — **the DRC is multi-operator and we are explicitly cross-network**, so our value is *interoperability*, not owning the dominant wallet *[market-share context — secondary]*.
- **Regulatory latitude.** Kenya's regulator allowed a telco to run e-money and (via bank partners) lending with relatively light early friction; **DRC licensing is an open item to investigate, not a given** (per workspace guardrails).
- **Owning the rails & float.** M-Pesa *is* the rail and holds the float; **we rent rails and hold nothing** — most of its monetisation playbook assumes the opposite posture.
- **Single-currency, single-country base.** Our cross-network/cross-wallet bridging is inherently more complex than M-Pesa's in-network Kenyan P2P.

---

## Notes / caveats / contradictions
- Several headline figures (Fuliza revenue, agent counts, user totals, Vodacom 103M) are **company-/press-reported**, not independently audited — labelled *[self-reported]*. Treat as directional.
- **Pay Bill and Lipa Na M-Pesa exact launch dates** are from secondary timelines; I did **not** find a primary Safaricom page stating the precise year — flagged as approximate.
- **Exact current USSD-vs-app transaction split: Not found** in primary sources (Safaricom press, Statista listings, Wikipedia). The >90% USSD figure is historical/secondary.
- Fees and limits are **time-sensitive** (Kenya tariffs change; some sources cite 2024, others 2026) — figures here reflect the 2024–2026 range as accessed 2026-06-09; re-verify before any quantitative use.
- Scope: deep DRC-specific M-Pesa API/availability detail intentionally **deferred to the operator findings**, not duplicated here.

---

## Sources

| ID | Title | URL | Type | Date | Reliability |
|----|-------|-----|------|------|-------------|
| cm-mp-001 | M-Pesa (Wikipedia) | https://en.wikipedia.org/wiki/M-Pesa | secondary | 2026-06-09 | med |
| cm-mp-002 | Mobile Payments go Viral: M-PESA in Kenya (World Bank) | https://documents1.worldbank.org/curated/en/638851468048259219/pdf/543380WP0M1PES1BOX0349405B01PUBLIC1.pdf | primary | 2026-06-09 | high |
| cm-mp-003 | M-PESA Consumer Tariffs & Limits (Safaricom) | https://www.safaricom.co.ke/main-mpesa/m-pesa-for-you/tariffs-limits/consumer-tariffs-limits | primary | 2026-06-09 | high |
| cm-mp-004 | M-PESA Charges 2026 full tariff table (PesaTrail) | https://pesatrail.com/mpesa-charges-2026.html | secondary | 2026-06-09 | med |
| cm-mp-006 | Lipa na M-Pesa Buy Goods (Till) Charges & Limits (mpesa.or.ke) | https://mpesa.or.ke/guides/lipa-na-mpesa-buy-goods-business-till-charges/ | secondary | 2026-06-09 | med |
| cm-mp-007 | Fixing the Hidden Charges in Lipa na M-Pesa (CGAP) | https://www.cgap.org/blog/fixing-hidden-charges-in-lipa-na-m-pesa | primary/research | 2026-06-09 | high |
| cm-mp-008 | Pochi la Biashara FAQ (Safaricom) | https://www.safaricom.co.ke/media-center-landing/frequently-asked-questions/pochi-la-biashara | primary | 2026-06-09 | high |
| cm-mp-011 | Safaricom revenue tops $3bn, FY2025 results (Safaricom) | https://www.safaricom.co.ke/media-center-landing/press-releases/safaricoms-revenue-tops-3-billion-dollars-a-first-in-the-region | self-reported | 2026-06-09 | med |
| cm-mp-013 | Fuliza the new lending king (Standard Media) | https://www.standardmedia.co.ke/business/financial-standard/article/2001430559/move-over-m-shwari-and-kcb-m-pesa-fuliza-now-the-new-lending-king | secondary | 2026-06-09 | med |
| cm-mp-014 | Safaricom, NCBA and KCB Restructure Fuliza (Safaricom) | https://www.safaricom.co.ke/media-center-landing/press-releases/safaricom-ncba-and-kcb-restructure-fuliza-with-free-daily-fees | self-reported | 2026-06-09 | med |
| cm-mp-016 | How M-Shwari Works (CGAP) | https://www.cgap.org/sites/default/files/Forum-How-M-Shwari-Works-Apr-2015.pdf | primary/research | 2026-06-09 | high |
| cm-mp-017 | M-Shwari (NCBA Group) | https://ke.ncbagroup.com/loan/m-shwari/ | primary | 2026-06-09 | high |
| cm-mp-020 | M-PESA Agents and Dealers (Safaricom) | https://www.safaricom.co.ke/main-mpesa/for-your-business/m-pesa-agents-and-dealers | primary | 2026-06-09 | high |
| cm-mp-021 | M-Pesa adds 6mn users to reach 40mn (Capital FM) | https://www.capitalfm.co.ke/business/2026/03/m-pesa-adds-6mn-new-users-to-reach-40mn/ | secondary | 2026-06-09 | med |
| cm-mp-028 | Vodacom launches M-Pesa Rallonge in DRC (Mobile Europe) | https://www.mobileeurope.co.uk/vodacom-launches-m-pesa-rallonge-in-democratic-republic-of-congo/ | secondary | 2026-06-09 | med |
| cm-mp-030 | M-PESA credit & savings (Safaricom) | https://www.safaricom.co.ke/main-mpesa/m-pesa-services/credit-and-savings | primary | 2026-06-09 | high |

_(Abridged to the 16 load-bearing sources; the agent's full 30-row registry — incl. additional secondary/low-reliability corroboration — is preserved in the run transcript.)_
