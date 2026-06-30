# Comparable profile: bKash (Bangladesh)

- **Topic / entity:** bKash Limited — Bangladesh's dominant mobile financial service (MFS); agent-led, USSD (`*247#`) + smartphone app.
- **Question it addresses:** How to structure, monetise, and phase a service that runs the **same offering across feature phones (USSD) and smartphones (app)** at scale, agent-dependent — directly informing our phased feature-phone/USSD channel.
- **Date researched:** 2026-06-09
- **Confidence:** Medium–High on structure/sequencing/ownership (multiple converging sources incl. CGAP/World Bank, Wikipedia, partner-bank press); Medium on current pricing (fees change; some figures from third-party calculators corroborated by bKash pages but not all fetched directly).
- **Claim types labelled inline:** [VF] verified fact · [SR] company self-reported/marketing · [INF] inference.

> Note on method: WebFetch was blocked for several target domains this session, so primary-page figures (bkash.com cash-out page, Future Startup history) are cited from search-engine extracts of those pages rather than direct fetches. Treated as Medium confidence and flagged where it matters.

Why bKash is our sharpest comparable: it is the clearest large-scale case of **one wallet, two access channels** — a `*247#` USSD menu for feature phones and a smartphone app — built **USSD/agent-first, app-second**, with cash-out fees as the revenue engine. That is almost exactly the channel architecture and sequencing we are contemplating, just with a **held-balance wallet** rather than our **pass-through** model (a key divergence, see "What it means for us").

---

## 1. Product structure & architecture

**Held-balance wallet, not pass-through.** bKash is a stored-value e-wallet: users **cash in** (load value via an agent or bank), hold a balance, then **send / pay / cash out** from it ([bKash cash-in, old.bkash.com](http://old.bkash.com/products-services/cash-in); [Antom guide](https://knowledge.antom.com/bkash-payment-guide)). [VF] This is structurally different from our MVP, which moves money leg-to-leg without holding a customer balance.

**The two channels run the same account.** Once registered, a user activates and operates the **same** bKash account either by dialling `*247#` (USSD, no internet) or logging into the app ([Antom guide](https://knowledge.antom.com/bkash-payment-guide); [bKash mobile recharge page](https://www.bkash.com/en/products-services/mobile-recharge)). [VF] The USSD menu exposes the core verbs — Send Money, Cash Out, Mobile Recharge, Payment, Pay Bill — and explicitly works "without the need of an internet connection" ([Antom guide](https://knowledge.antom.com/bkash-payment-guide)). [VF, marketing-sourced but corroborated]

**Feature-phone parity was the *starting point*, not a bolt-on.** bKash launched in 2011 as a USSD/agent service; the smartphone app did not arrive until **May 2018** ([Wikipedia](https://en.wikipedia.org/wiki/BKash); [Future Startup, app registration](https://futurestartup.com/2018/04/21/bkash-opens-registration-for-bkash-app-bkashs-digital-payment-push-comes-full-circle/); [The Daily Star, app launched](https://www.thedailystar.net/business/bkash-app-launched-1576999)). [VF] So for its first ~7 years bKash was effectively a **feature-phone product**. The app then **added surface** (QR payments, voice assistance, payment reminders, in-app savings/loans, bank linkage) on top of the USSD core rather than replacing it ([TBS, bKash app](https://www.tbsnews.net/tech/bkash-app-how-countrys-mfs-giant-changing-lives-665750); [Daily Star, app launched](https://www.thedailystar.net/business/bkash-app-launched-1576999)). [VF] The USSD channel still carries the full transactional core today. [INF — from current bKash pages still documenting `*247#` for send/cash-out/bill]

**The agent network is the physical backbone.** Cash-in and cash-out happen at human agent points. The network scaled from **~500 agents (2011) → ~50,000 (2013) → ~135,000 (2015)** and bKash/press now cite **~300,000 agents** serving 70M+ customers ([Future Startup history extract](https://futurestartup.com/2025/04/22/a-brief-history-of-bkash-trajectory-funding-and-lessons/); [bKash cash-out from agent](https://www.bkash.com/en/products-services/cashout-from-agent)). [VF for trajectory; SR for current 300k] Agents register customers, take physical cash for cash-in, and dispense cash for cash-out — the service does not function without them.

**OTC vs registered wallet — a critical structural nuance.** Early on, most "users" did not have their own wallet: CGAP found that in 2014 **~75%+ of MFS use in Bangladesh was over-the-counter (OTC)** — a customer hands cash to an agent who transacts from the agent's own account — versus <25% true registered-wallet use; **56% of registered users had started as OTC** before opening a wallet ([CGAP, OTC→wallets](https://www.cgap.org/blog/transitions-otc-to-wallets-evidence-bangladesh); [CGAP, mobile wallets transition](https://www.cgap.org/blog/mobile-wallets-is-transition-underway-in-bangladesh)). [VF, 2014 vintage] The agent-assisted, low-literacy-friendly OTC pattern is how bKash bootstrapped before smartphones; the multi-year drift toward registered wallets is what the app accelerated. [INF]

**Interoperability — mostly inbound bank rails, limited wallet-to-wallet.** bKash interconnects with banks: **Add Money** pulls funds in from 50+ banks/cards (free in-app), and **bKash-to-Bank** / NPSB pushes out to bank accounts (for a fee) ([bKash add money](https://www.bkash.com/en/products-services/add-money); [bKash bank-to-bKash](https://www.bkash.com/en/products-services/bank-account)). [VF] It also offers **Send Money to non-bKash numbers** ([bKash page](https://www.bkash.com/en/products-services/send-money-to-non-bkash-user)). [VF — mechanics not fully verified] But true *cross-MFS* interoperability (bKash↔Nagad wallet-to-wallet) is weak: an independent governance report notes only **4.2% of merchant accounts** sit with the #2 player and flags **lack of interoperability** as an industry gap ([TI-Bangladesh MFS governance, 2025](https://www.ti-bangladesh.org/images/2025/report/mfs/Executive-Summary-Mobile-Financial-Services-Sector-En.pdf?v=1)). [VF] Contrast with us: **cross-network bridging is our entire product premise**, whereas bKash grew as a largely closed loop and only later bolted on bank interop.

---

## 2. Monetisation & value capture

**Cash-out fees are the dominant revenue line — by design.** CGAP, describing bKash's early model, states primary revenue was **1.85% of cash-out value plus Tk 5 per wallet-to-wallet transfer, plus interest on the float account** ([CGAP, fast start](https://www.cgap.org/blog/bkash-bangladesh-what-explains-its-fast-start); [CGAP/World Bank 2014 brief](https://www.cgap.org/sites/default/files/Brief_bKash_Bangladesh_July_2014.pdf)). [VF, 2014] That structure persists: current cash-out is cited at **1.85% (Tk 18.50 per Tk 1,000)** via app or `*247#`, with a discounted **Tk 14.90 per Tk 1,000** for ATM and for up to two "Priyo Agent" numbers (capped at Tk 50,000/month) ([cashoutcharge.com, bKash](https://www.cashoutcharge.com/bkash); [bKash cash-out](https://www.bkash.com/en/products-services/cashout); [BSS, low-cost cash-out](https://www.bssnews.net/business/177484)). [VF for the rates; Medium confidence since the bKash page itself wasn't fetched directly]

**The fee structure deliberately steers behaviour:**
| Action | Fee (approx, 2026) | Who pays | Note |
|---|---|---|---|
| Cash-in (agent or bank) | **Free** | — | Loss-leader to load value ([bKash add money](https://www.bkash.com/en/products-services/add-money)) [VF] |
| Cash-out (agent, app/USSD) | **1.85%** (Tk 18.5/1,000) | Customer | Core revenue ([cashoutcharge.com](https://www.cashoutcharge.com/bkash)) [VF] |
| Cash-out (ATM / Priyo Agent) | **Tk 14.9/1,000** | Customer | Discount to push loyalty/ATM ([BSS](https://www.bssnews.net/business/177484)) [VF] |
| Send money (app, P2P) | **Free** | — | App perk vs USSD ([TBS](https://www.tbsnews.net/tech/bkash-app-how-countrys-mfs-giant-changing-lives-665750)) [VF] |
| Send money (USSD `*247#`, P2P) | **Tk 5/txn** | Sender | USSD costs more than app ([TBS](https://www.tbsnews.net/tech/bkash-app-how-countrys-mfs-giant-changing-lives-665750)) [VF] |
| Send to "Priyo" number | **Free** (to limits) | — | Tk 5 over Tk 25k, Tk 10 over Tk 50k/mo ([bKash Priyo](https://www.bkash.com/en/products-services/send-money-to-priyo-number)) [VF] |
| Merchant payment | **1.5%–1.85%** | Merchant (MDR) | ([bKash payment](https://www.bkash.com/en/products-services/payment)) [VF] |
| bKash→Bank transfer | **~Tk 20/1,000** | Customer | ([bdesheba extract](https://bdesheba.com/bkash-all-charges/)) [VF — third-party] |
| Bill pay | First bill/month **free**, then small fee | Customer | ([bKash pay bill](https://www.bkash.com/en/products-services/pay-bill)) [VF] |

Two strategic reads: (a) **the app is cheaper than USSD for P2P (free vs Tk 5)** — bKash uses pricing to *pull* customers toward the app while keeping USSD universal; (b) but **cash-out — the revenue engine — costs the same on both channels (1.85%)**, so monetisation does not depend on the user being on a smartphone. [INF, well-supported by the fee table]

**Float interest is a real second line.** Customer balances sit in a regulated trust/float account; bKash earns interest on it ([CGAP fast start](https://www.cgap.org/blog/bkash-bangladesh-what-explains-its-fast-start)). [VF] This is **only available because bKash holds balances** — a lever a pure pass-through model forgoes. [INF]

**Later lines — merchant MDR, savings, and bank-funded nano-loans:**
- **Merchant payments**: QR + merchant accounts, 1.5–1.85% MDR ([bKash merchant](https://www.bkash.com/en/business/merchant); [bKash payment](https://www.bkash.com/en/products-services/payment)). [VF]
- **Savings (DPS)**: launched 2021; bKash is the *distribution rail*, partner banks/FIs (BRAC Bank, IDLC, Dhaka Bank, Mutual Trust, EBL) hold the deposit and pay ~8.5–10% interest; **5M+ DPS accounts opened in 4 years** ([Daily Star, DPS](https://www.thedailystar.net/supplements/personal-finance-wisdoms/news/save-smarter-live-better-introducing-bkashs-innovative-dps-3665701); [TBS, 5M DPS](https://www.tbsnews.net/economy/corporates/dps-openings-hit-5-million-through-bkash-app-1289266)). [VF/SR]
- **Digital Nano Loan**: piloted 2020, commercial 2021, with **City Bank** as lender (Bangladesh Bank-approved); Tk 500–20,000, 9% p.a., 3-month EMI; ~**Tk 7.5bn disbursed to 250k+ customers (~800k loans)**; "Pay-Later" interest-free variant added 2024 ([City Bank/bKash launch](https://www.citybankplc.com/newsevent/city-bank--bkash-jointly-launch-country%E2%80%99s-first-digital-nano-loan); [TBS, Tk7b disbursed](https://www.tbsnews.net/economy/corporates/digital-loan-amounting-tk70-billion-city-bank-disbursed-thru-bkash-app-812826); [TBS, Pay-Later](https://www.tbsnews.net/economy/banking/how-pay-later-provides-instant-purchasing-power-access-interest-free-loans-820661)). [VF/SR] Crucially, **the bank carries the credit risk and balance-sheet; bKash supplies the rail, data, and distribution** — a partnership pattern forced by Bangladesh's bank-led regulation. [VF/INF]

**Scale of the money machine (context):** bKash reports ~**82–83M verified users** ([Wikipedia](https://en.wikipedia.org/wiki/BKash); Future Startup); 2024 revenue ~**Tk 25bn** (~$210M) and profit Tk 315.77 crore (+67% YoY) per press citing filings ([TBS, MFS market](https://www.tbsnews.net/economy/banking/mfs-market-flourishes-transactions-surge-tk384-lakh-crore-2024-1072141); [Statista, bKash revenue](https://www.statista.com/statistics/1372638/bkash-revenue/)). [VF — press-reported financials, Medium]

---

## 3. Feature rollout & sequencing

**Ownership / funding milestones (each unlocked a phase):**
| Date | Event | What it unlocked |
|---|---|---|
| 2010 | Founded as JV: **Money in Motion LLC** (US; Quadir brothers et al.) + **BRAC Bank** | Bank-led licence + entrepreneurial drive |
| Jul 2011 | **Launch** with Cash In, Cash Out, Send Money (USSD/agent) | The core, feature-phone-first |
| Apr 2013 | **IFC** equity | Agent-network scale-up capital |
| Mar 2014 | **Gates Foundation** equity | Financial-inclusion credibility + capital |
| Apr 2018 | **Ant Financial / Alipay** (~20% stake) | Tech + super-app playbook; app era |
| May 2018 | **Smartphone app** launches | Digital payments, QR, app-only features |
| 2020–21 | **Digital Nano Loan** (pilot→commercial, City Bank) | Credit, on bank balance sheet |
| 2021 | **DPS savings** via partner banks | Deposits-as-distribution |
| Nov 2021 | **SoftBank Vision Fund II** $250M (>10%), first **unicorn** ($1bn+, ~$2bn val.) | Growth capital, super-app expansion |
| 2024 | **Pay-Later** interest-free nano-loan | Embedded credit at checkout |

([Wikipedia](https://en.wikipedia.org/wiki/BKash); [Future Startup history](https://futurestartup.com/2025/04/22/a-brief-history-of-bkash-trajectory-funding-and-lessons/); [Harvard d3 case](https://d3.harvard.edu/platform-rctom/submission/bkash-bracs-venture-into-mobile-banking/); City Bank/TBS cites above) [VF for dates/investors; SR for the $2bn valuation]

**The sequence, distilled:** USSD + agents (cash-in/out/send) → user & agent scale → **then** smartphone app → merchant payments/QR → bill pay → **then** savings & nano-loans via bank partners. Payments came first and built the user base and float; **lending/savings came last, layered on the installed base, and were outsourced to banks** for regulatory and risk reasons. [VF/INF] Geographic sequencing was **deep within one country**, not multi-country — bKash densified Bangladesh rather than expanding abroad early. [INF — no evidence of early foreign expansion found]

---

## Bangladesh-specific factors that may NOT transfer to DRC

- **Bank-led regulation.** Bangladesh Bank mandates a **bank-led MFS model** and has **restricted telcos/NBFIs from holding MFS licences**; bKash exists as a **BRAC Bank subsidiary** ([TBS, MFS regulation](https://www.tbsnews.net/thoughts/mfs-grows-its-regulations-need-change-136069); [Wikipedia](https://en.wikipedia.org/wiki/BKash)). [VF] This shaped *everything* — why savings/loans run through partner banks, why bKash holds a regulated float. DRC's regime differs (operator-led mobile money — Vodacom/Orange/Airtel — dominates); **do not assume the bank-led structure transfers.** [INF — flag for the regulatory track]
- **Extreme population density & near-mono-language.** Bangladesh is one of the densest countries on earth with a relatively homogeneous Bangla-speaking population, which makes a single USSD menu and a dense agent grid economic. The **DRC is vast, low-density, multilingual (French + Lingala/Swahili/Kikongo/Tshiluba), with weaker connectivity** — agent economics and USSD menu localisation are materially harder. [INF — population/connectivity is general knowledge, not separately sourced here; verify before relying on it]
- **OTC dominance was a Bangladesh phenomenon of its era** ([CGAP](https://www.cgap.org/blog/transitions-otc-to-wallets-evidence-bangladesh)); DRC's mix may differ and our pass-through model doesn't host OTC the same way.
- **Currency/inflation**: Tk fee absolutes (Tk 5, Tk 18.5/1,000) reflect Bangladeshi price levels; only the *structure*, not the numbers, transfers.

---

## What it means for us

**On running a bare-bones USSD/feature-phone version alongside the app:**

1. **Channel parity is achievable and proven — design the USSD menu as the *transactional core*, the app as the *richer surface over the same account.*** bKash ran feature-phone-only for ~7 years and still keeps full send/cash-out/bill-pay on `*247#`. For us: a user's identity/account and the **collect→payout** flow should be expressible as a flat USSD menu; the smartphone app adds QR, history, contacts, reminders — but must not be a *prerequisite* for the core verb (pay any number). [INF from bKash structure]

2. **Sequence USSD/app deliberately, but know our economics differ.** bKash launched USSD-first because smartphones were rare; we are launching **app-first** by choice and phasing USSD after. The lesson that transfers is the **layering discipline**: ship the core money-movement first, add surface later — don't gate the core behind app-only features. [INF]

3. **Where USSD economics differ — three hard truths:**
   - **USSD has a per-session/shortcode cost and per-operator lead time** (our own offline-ussd-feasibility finding flags shortcode provisioning as the reason USSD is phased). bKash absorbed USSD costs because cash-out (its revenue) is channel-agnostic. **We have no cash-out revenue line** (pass-through), so USSD session costs hit our unit economics more directly — model this explicitly. [INF — important divergence]
   - bKash **prices the app cheaper than USSD for P2P (free vs Tk 5)** to pull users onto the app. We can copy the *direction* (nudge to app) but our fee on a pass-through payment is structurally thinner than a 1.85% cash-out, leaving less room to subsidise USSD. [INF]
   - USSD UX is text-menu only — **payee = MSISDN keyed in** fits USSD natively (no QR), which actually *suits* our MSISDN-payee design. USSD is a good fit for "type a number, pay it." [INF — positive]

4. **Agent dependence is bKash's superpower and its tax — and we largely sidestep it.** bKash *must* have ~300k agents because it holds balances that need physical cash-in/cash-out. **Our pass-through model collects from and pays to existing operator wallets**, so we do **not** need our own cash-in/cash-out agent army — we ride the operators' existing agent/float networks. This is a real simplification, but it also means **we forgo cash-out fees and float interest, bKash's two biggest revenue lines.** We must monetise the *bridge* (cross-network transfer margin/markup) instead. [INF — central strategic implication]

5. **Monetisation lesson, adapted:** bKash makes money on **cash-out (the exit), not on sending.** The analog for us is to **price the cross-network bridge** (the value is convenience of paying across networks), keep same-network or trivial actions cheap, and treat the fee as borne by the **payer** at the moment of a cross-network payment. Float interest and cash-out are not on our menu by design; **don't build a business plan that assumes them.** [INF]

6. **Defer lending/savings; when you add them, rent a bank's balance sheet.** bKash added nano-loans/savings *last*, on a large installed base, and **outsourced credit risk and deposit-holding to partner banks** (partly regulation-forced). For us this doubles as a regulatory hedge: if/when we add credit or savings, structure them as **bank-partner products on our rail**, not balance-sheet risk we carry. [INF — also a flag for the legal/licensing track]

7. **Watch the regulatory model carefully — bKash's bank-led structure is a Bangladesh artefact.** Our equivalent question (who can hold the licence / the float / extend credit in DRC) is unresolved and is explicitly a **flag-for-investigation** item, not a settled assumption. [INF — defer to regulatory track]

---

## Source(s)
See registry block below; all also cited inline. Primary where possible (bkash.com, City Bank, CGAP/World Bank, Bangladesh Bank-adjacent press, Wikipedia for sourced chronology). Several bkash.com figures are via search-engine extracts (WebFetch blocked) — labelled Medium confidence.

## Notes / caveats / contradictions
- **No direct page fetches** this session (WebFetch denied for bkash.com, futurestartup.com); figures rely on search extracts of those pages. Re-verify cash-out rate and current user/agent counts directly before using in a report.
- **User count varies by source/date** (70M / 80M / 82–83M) — all "verified users," self-reported, growing; treat as ~80M+ (2025–26). [SR]
- **Ant Financial stake** reported as "20%" in some sources, "significant" in others — Medium confidence on the exact %.
- **OTC vs wallet data is 2014-vintage** (CGAP); directionally important for the bootstrapping lesson but not current.
- bKash is a **held-balance wallet**; we are **pass-through** — the single most important structural caveat when borrowing its lessons.

---

SOURCES_FOR_REGISTRY:
| cm-bk-001 | bKash — Wikipedia | https://en.wikipedia.org/wiki/BKash | encyclopedia (sourced) | 2026-06-09 | Medium | bkash.md |
| cm-bk-002 | A Brief History of bKash: Trajectory, Funding, and Lessons — Future Startup | https://futurestartup.com/2025/04/22/a-brief-history-of-bkash-trajectory-funding-and-lessons/ | industry press / history | 2026-06-09 | Medium | bkash.md |
| cm-bk-003 | A Comprehensive Guide to bKash — Antom (Ant) | https://knowledge.antom.com/bkash-payment-guide | vendor guide | 2026-06-09 | Medium | bkash.md |
| cm-bk-004 | Cash Out from the largest agent and ATM network — bKash | https://www.bkash.com/en/products-services/cashout | primary (company) | 2026-06-09 | Medium (via extract) | bkash.md |
| cm-bk-005 | bKash Cash Out Calculator — cashoutcharge.com | https://www.cashoutcharge.com/bkash | third-party calculator | 2026-06-09 | Medium | bkash.md |
| cm-bk-006 | bKash app: How the country's MFS giant is changing lives — The Business Standard | https://www.tbsnews.net/tech/bkash-app-how-countrys-mfs-giant-changing-lives-665750 | reputable press | 2026-06-09 | High | bkash.md |
| cm-bk-007 | bKash app launched — The Daily Star | https://www.thedailystar.net/business/bkash-app-launched-1576999 | reputable press | 2026-06-09 | High | bkash.md |
| cm-bk-008 | bKash opens registration for bKash App — Future Startup | https://futurestartup.com/2018/04/21/bkash-opens-registration-for-bkash-app-bkashs-digital-payment-push-comes-full-circle/ | industry press | 2026-06-09 | Medium | bkash.md |
| cm-bk-009 | bKash Bangladesh: What Explains its Fast Start? — CGAP | https://www.cgap.org/blog/bkash-bangladesh-what-explains-its-fast-start | development institution (primary research) | 2026-06-09 | High | bkash.md |
| cm-bk-010 | Transitions from OTC to Wallets: Evidence from Bangladesh — CGAP | https://www.cgap.org/blog/transitions-otc-to-wallets-evidence-bangladesh | development institution (primary research) | 2026-06-09 | High | bkash.md |
| cm-bk-011 | bKash Bangladesh: A Fast Start for MFS (2014 brief) — CGAP/World Bank | https://www.cgap.org/sites/default/files/Brief_bKash_Bangladesh_July_2014.pdf | development institution (primary research) | 2026-06-09 | High | bkash.md |
| cm-bk-012 | City Bank & bKash jointly launch country's first Digital Nano Loan — City Bank | https://www.citybankplc.com/newsevent/city-bank--bkash-jointly-launch-country%E2%80%99s-first-digital-nano-loan | primary (partner bank) | 2026-06-09 | High | bkash.md |
| cm-bk-013 | Digital loan amounting Tk7.0 billion from City Bank disbursed thru bKash app — The Business Standard | https://www.tbsnews.net/economy/corporates/digital-loan-amounting-tk70-billion-city-bank-disbursed-thru-bkash-app-812826 | reputable press | 2026-06-09 | High | bkash.md |
| cm-bk-014 | City Bank-bKash launch 'Pay-Later' — The Business Standard | https://www.tbsnews.net/economy/banking/how-pay-later-provides-instant-purchasing-power-access-interest-free-loans-820661 | reputable press | 2026-06-09 | High | bkash.md |
| cm-bk-015 | Save smarter, live better: Introducing bKash's DPS — The Daily Star | https://www.thedailystar.net/supplements/personal-finance-wisdoms/news/save-smarter-live-better-introducing-bkashs-innovative-dps-3665701 | reputable press | 2026-06-09 | Medium | bkash.md |
| cm-bk-016 | DPS openings hit 5 million through bKash app — The Business Standard | https://www.tbsnews.net/economy/corporates/dps-openings-hit-5-million-through-bkash-app-1289266 | reputable press | 2026-06-09 | High | bkash.md |
| cm-bk-017 | MFS market flourishes, transactions surge in 2024 — The Business Standard | https://www.tbsnews.net/economy/banking/mfs-market-flourishes-transactions-surge-tk384-lakh-crore-2024-1072141 | reputable press | 2026-06-09 | High | bkash.md |
| cm-bk-018 | Governance Challenges in MFS Sector in Bangladesh (Exec Summary) — Transparency Intl Bangladesh | https://www.ti-bangladesh.org/images/2025/report/mfs/Executive-Summary-Mobile-Financial-Services-Sector-En.pdf?v=1 | NGO research | 2026-06-09 | Medium-High | bkash.md |
| cm-bk-019 | Mobile Financial Services in Bangladesh: regulations need to change — The Business Standard | https://www.tbsnews.net/thoughts/mfs-grows-its-regulations-need-change-136069 | reputable press / opinion | 2026-06-09 | Medium | bkash.md |
| cm-bk-020 | Cash In — Product and Service — bKash (legacy site) | http://old.bkash.com/products-services/cash-in | primary (company) | 2026-06-09 | Medium | bkash.md |
| cm-bk-021 | Add Money to bKash from Your Bank or Card — bKash | https://www.bkash.com/en/products-services/add-money | primary (company) | 2026-06-09 | Medium (via extract) | bkash.md |
| cm-bk-022 | Payment / Merchant — bKash | https://www.bkash.com/en/products-services/payment | primary (company) | 2026-06-09 | Medium (via extract) | bkash.md |
| cm-bk-023 | Send Money to Priyo Number (free) — bKash | https://www.bkash.com/en/products-services/send-money-to-priyo-number | primary (company) | 2026-06-09 | Medium (via extract) | bkash.md |
| cm-bk-024 | bKash further expands low-cost cash out facility — BSS News | https://www.bssnews.net/business/177484 | state news agency | 2026-06-09 | Medium | bkash.md |
| cm-bk-025 | bKash revenue 2024 — Statista | https://www.statista.com/statistics/1372638/bkash-revenue/ | data aggregator | 2026-06-09 | Medium | bkash.md |
| cm-bk-026 | bKash, BRAC's venture into mobile banking — Harvard d3 (TOM) | https://d3.harvard.edu/platform-rctom/submission/bkash-bracs-venture-into-mobile-banking/ | academic case | 2026-06-09 | Medium | bkash.md |
