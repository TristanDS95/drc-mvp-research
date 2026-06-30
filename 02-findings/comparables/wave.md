# Finding: Wave (Senegal / Côte d'Ivoire) — low-fee disruptor comparable

- **Topic / entity:** Wave Mobile Money — comparable for our DRC consumer cross-network pass-through MVP
- **Question it addresses:** How to structure our app, how to monetise, and how to sequence feature rollout — using the closest market + model analog (francophone West Africa, low-fee disruptor on an agent + app + QR model)
- **Date researched:** 2026-06-09
- **Confidence:** Medium-High (well-corroborated on structure/pricing/funding; Lower on internal economics, which are largely self-reported or inferred)
- **Claim type:** mixed — labelled inline

> **Lens framework:** (1) product structure & architecture, (2) monetisation & value capture, (3) feature rollout & sequencing. Ends with **"What it means for us."**

---

## ⚠️ Critical correction to the brief's premise (pricing is INVERTED)

The research brief stated "free P2P send; ~1% cash-out fee." **Every source contradicts this.** Wave's actual model, consistently and as currently marketed (2024–2025), is:

- **Sending money (P2P): ~1% fee** (flat, paid by sender).
- **Deposits (cash-in) and withdrawals (cash-out): FREE.**
- **Bill pay: free to the user** (Wave absorbs / pushes cost to the biller-merchant).

So the fee sits on **transfers**, and **cash-out is the free hook**, not the revenue source. This is the opposite of the brief and is decision-relevant — flagging prominently. *(Verified fact — corroborated across Wave's own site, Rest of World, TechCabal, Quartz, TriplePundit; [wave.com](https://www.wave.com/en/), [Rest of World](https://restofworld.org/2022/how-wave-is-disrupting-francophone-africas-mobile-money-market/), [TechCabal — debt raise](https://techcabal.com/2025/06/30/wave-raises-137-milion/).)*

---

## 1. Product structure & architecture

**Single-purpose consumer app + free QR card + dense human-agent network.** Wave is deliberately **app-first and app-centric**, not USSD-first — explicitly the inverse of the telco incumbents (Orange Money, Free/Expresso), which lead with USSD menus. The app's design philosophy is radical minimalism: a visual, icon-driven UI built around QR rather than dial-codes/SMS, intended to be usable by first-time digital-finance users. *(Verified fact / company-positioned; [Rest of World](https://restofworld.org/2022/how-wave-is-disrupting-francophone-africas-mobile-money-market/), [businessmodelcanvas (derivative)](https://businessmodelcanvastemplate.com/blogs/how-it-works/wave-mobile-money-how-it-works).)*

**The agent network is the core physical layer.** Wave runs a network of **~150,000 agents** (small local shops) who act as human ATMs — using their own cash float to handle cash-in/cash-out for users. Reported agent counts vary by date (150,000–200,000+); 150,000 is the figure Wave/RMB used in the mid-2025 debt-raise materials. Agents are independent businesses, not employees — an **asset-light** expansion model. *(Verified fact / company-self-reported; [TechCabal 2025](https://techcabal.com/2025/06/30/wave-raises-137-milion/), [TriplePundit](https://triplepundit.com/2025/wave-mobile-money-cote-divoire/).)*

**Merchant QR acceptance is a first-class surface.** Merchants accept payment by displaying a QR (or scanning the customer), receive into a Wave business account, and withdraw for free (to a Wave wallet, any agent, or by wire). There is a separate **Wave Business** app. Merchant onboarding clearly exists — unlike our v1 — so this is the "later, deliberate expansion" path our own product summary anticipates. *(Verified fact / company-positioned; [Wave Business listing](https://spark.mwm.ai/us/apps/wave-business/1623089106), [Rest of World](https://restofworld.org/2022/how-wave-is-disrupting-francophone-africas-mobile-money-market/).)*

**Feature-phone / no-smartphone path — important nuance for us:**
- Multiple sources state **Wave does NOT use USSD** in its core (Senegal) markets; it is "solely app-based." *(Verified fact, with caveats below; [Rest of World](https://restofworld.org/2022/how-wave-is-disrupting-francophone-africas-mobile-money-market/), [Techjaja — Uganda](https://techjaja.com/wave-mobile-money-in-uganda-explained-servies-charges-and-agents/).)*
- Its substitute for non-smartphone users is a **free physical QR card** (bank-card-sized). The user without a smartphone goes to an **agent**, who scans the card to deposit/withdraw/transact on the user's behalf. The card "supports all services," but only **via an agent** — i.e., not a self-service offline channel. *(Verified fact; [Techjaja](https://techjaja.com/wave-mobile-money-in-uganda-explained-servies-charges-and-agents/), [Rest of World](https://restofworld.org/2022/how-wave-is-disrupting-francophone-africas-mobile-money-market/).)*
- One secondary source claims Wave built "a streamlined USSD interface" alongside the app; this is **unconfirmed and contradicts the dominant "no USSD" reporting.** Flagging as a **contradiction** — likely market-specific or imprecise. **Not treated as established.** *(Contradiction; [Techjaja](https://techjaja.com/wave-mobile-money-in-uganda-explained-servies-charges-and-agents/).)*

→ **Takeaway for our USSD track:** Wave is *not* a precedent for a self-service USSD channel. It chose **agent-mediated QR cards** as its low-end inclusion mechanism instead. That is a genuine architectural alternative to building USSD ourselves — but it presupposes owning an agent network, which we (pure pass-through on pawaPay, no agents) do not have.

**Held balance vs pass-through (a key divergence from us).** Wave is a **stored-value wallet** — users hold a Wave balance; agents move cash in/out of it. This is **not** the pure pass-through model we're pursuing. Holding balances is precisely what required Wave to obtain an e-money licence (below). *(Inference from EMI requirement + wallet/withdraw-balance descriptions; [Wave EMI blog](https://www.wave.com/en/blog/sn-emi/).)*

**EMI / e-money licence (WAEMU) — what it unlocked.** On **14 April 2022**, Wave (via "Wave Digital Finance") became the **first non-bank, non-telecom fintech operating across multiple WAEMU countries to receive an Electronic Money Institution (EME) licence from the BCEAO** (the regional central bank for the 8-country West African Monetary Union). Before this, Wave relied on partner banks (UBA, Ecobank) to issue e-money on its behalf. The licence let Wave **issue e-money directly** and expand into merchant payments, savings, credit, and remittances without a bank intermediary — i.e., regulatory sovereignty over its own rails. *(Verified fact / company + press; [Wave EMI blog](https://www.wave.com/en/blog/sn-emi/), [TechCabal 2022](https://techcabal.com/2022/04/21/wave-gets-e-money-license/), [Global Finance](https://www.gfmag.com/magazine/may-2022/first-e-money-license-issued-west-african-fintech).)*

---

## 2. Monetisation & value capture

**Headline:** Wave makes money on the **~1% P2P transfer fee**, deliberately giving away cash-in, cash-out, and bill-pay to win volume. Wave's pitch is volume-over-margin: *"One percent of all of these payments flowing through an entire economy is not an unprofitable business."* *(Company self-reported; [Quartz](https://qz.com/africa/2189528/how-wave-rose-to-become-francophone-africas-first-unicorn).)*

**Revenue stack (most → least, as reported):**
| Stream | Mechanism | Note |
|---|---|---|
| **P2P transfer fee (~1%)** | Flat 1% on send, paid by sender | The dominant driver. One (derivative) source estimates ~65% of revenue. *(Company-self-reported / derivative inference.)* |
| **B2B / B2G services** | Bulk payroll disbursements, salary/wage payouts, utility & **tax** collections | Reported as a growing, higher-value line. *(Self-reported.)* |
| **Bill pay** | Free to consumer; cost **shifted to the biller/merchant** | So bill pay is a merchant-side fee, not a giveaway. *([TechCabal 2025](https://techcabal.com/2025/06/30/wave-raises-137-milion/).)* |
| **Merchant payments (MDR)** | Fees on larger retail integrations | Growing post-EMI. *(Self-reported / inference.)* |
| **Credit / value-added (since ~2024)** | "Wave Credit" micro-loans (interest share / lead fees); insurance ambitions | The intended margin-expansion layer. *(Self-reported.)* |

**How a near-free model makes money — honest economics:**
- **The 1% transfer fee is the engine.** It only works at enormous volume and with **very low cost-to-serve**, which Wave engineers via: (a) app + QR instead of expensive USSD session fees and SMS; (b) **independent agents who self-fund their cash float** (Wave pays agents a commission but doesn't carry ATM capex); (c) **in-house tech stack** keeping per-transaction cost down. *(Inference + [Rest of World](https://restofworld.org/2022/how-wave-is-disrupting-francophone-africas-mobile-money-market/).)*
- **Float:** As a stored-value issuer, Wave holds customer e-money balances. Under BCEAO EME rules, such float is typically required to be held in safeguarded/segregated bank accounts, which **constrains** how freely Wave can earn interest on it. I found **no source confirming float interest is a material Wave revenue line** — treat float income as **unconfirmed / likely minor**, not a pillar. *("Not found" — flagged; general EME-rule inference.)*
- **Is it sustainable / VC-subsidised?** Mixed, and partly subsidised:
  - Founder Lincoln Quirk admitted (≈2020–22) the company **was not profitable and was "likely years away."** *(Self-reported via [Quartz](https://qz.com/africa/2189528/how-wave-rose-to-become-francophone-africas-first-unicorn).)*
  - Wave **laid off ~15% of staff (~300 people)** in June 2022, mostly in newer operations (Mali, Burkina Faso, Uganda) — i.e., the **disruption is cheap in mature markets but expensive to stand up in new ones.** *([TechCabal 2022](https://techcabal.com/2022/08/05/how-wave-is-navigating-the-economic-downturn/).)*
  - **Uganda** specifically: "Wave Transfer Limited" reported a **~$3.78M net loss for FY2024** — a concrete data point that new-market entry runs at a loss. *(Reported figure; [businessmodelcanvas (derivative) — treat with caution](https://businessmodelcanvastemplate.com/blogs/how-it-works/wave-mobile-money-how-it-works).)*
  - **Counter-signal:** Wave was the **only African company on Y Combinator's "top revenue-generating startups" list in both 2023 and 2024**, and **self-reported >$100M revenue in 2022** — so at least the **mature markets (Senegal especially) appear genuinely revenue-generating**, even if the consolidated group reinvests into expansion. *(Self-reported / YC list; [TechCabal 2024](https://techcabal.com/2024/05/02/wave-only-african-startup-yc-top-revenue-list/).)*
  - **Net read (inference):** Senegal/CI look like a real business on 1% volume; the **group is VC/debt-financed for land-grab**, accepting losses in frontier markets to buy share. The model is plausibly sustainable *in a mature, dense market* but **not** as a turn-key money-printer on day one. *(Inference.)*

> ⚠️ **Suspect metric, not used:** one summary claimed "12 billion transactions in Senegal alone in 2022." Against a ~17M population this is implausible (~700+ txns/person/yr) and uncorroborated — **excluded as likely misreport.**

---

## 3. Feature rollout & sequencing

**Timeline (verified facts unless noted):**
| When | Move | Source |
|---|---|---|
| 2016–2018 | Origins: Wave grew out of **Sendwave** (US→Africa remittances) co-founders Drew Durbin & Lincoln Quirk; Wave Mobile Money launched ~**2018** in **Senegal**. | [Quartz](https://qz.com/africa/2189528/how-wave-rose-to-become-francophone-africas-first-unicorn) |
| ~**2021 (April)** | Entered **Côte d'Ivoire** — first market beyond Senegal. ⚠️ **Contradiction:** some sources say "began operating in CI in **2019**." Both recorded; **April 2021** is the more commonly cited *commercial* launch. | [Quartz](https://qz.com/africa/2189528/how-wave-rose-to-become-francophone-africas-first-unicorn), [The Africa Report](https://www.theafricareport.com/97171/senegal-cote-divoire-wave-the-fintech-thats-shaking-up-the-mobile-money-industry/) |
| **Sep 2021** | **$200M Series A at $1.7B valuation** — **largest-ever Series A in Africa**; made Wave **Francophone Africa's first unicorn.** Led by **Sequoia Heritage, Founders Fund, Stripe, Ribbit Capital** (+ Partech, Sam Altman). | [TechCrunch](https://techcrunch.com/2021/09/06/sequoia-heritage-stripe-and-others-invest-200m-in-african-fintech-wave-at-1-7b-valuation/), [Finextra](https://www.finextra.com/newsarticle/38780/wave-closes-largest-series-a-round-for-an-african-fintech-with-200-million) |
| Jun 2022 | **~15% layoffs (~300)**, concentrated in newer markets — cost discipline during downturn. | [TechCabal 2022](https://techcabal.com/2022/08/05/how-wave-is-navigating-the-economic-downturn/) |
| Apr 2022 | **EME licence from BCEAO** (see §1) — unlocked direct issuance + the value-added roadmap. | [Wave EMI blog](https://www.wave.com/en/blog/sn-emi/) |
| 2023–2024 | Expanded into **Mali, Burkina Faso, Uganda, The Gambia**; launched **Wave Credit** (micro-loans). | [TechCabal 2024](https://techcabal.com/2024/05/02/wave-only-african-startup-yc-top-revenue-list/), [Dabafinance](https://www.dabafinance.com/en/news/wave-raises-137m-debt-round-mobile-money-expansion-africa-2025) |
| Jun 2025 | **$137M (~€117M) debt round** led by **Rand Merchant Bank**, with DFIs **BII, Finnfund, Norfund** — working capital + expansion. Now **8 West African countries, 20M+ monthly users, ~150k agents, ~3,000 staff.** Recent entry: **Cameroon** (via Commercial Bank Cameroon). | [TechCabal 2025](https://techcabal.com/2025/06/30/wave-raises-137-milion/), [Finnfund](https://www.finnfund.fi/en/news-and-publications/news/wave-raises-eur-117-million-to-accelerate-financial-inclusion-across-the-african-continent/) |

⚠️ **Unverified — "Wave Bank":** Two lower-tier sources claim Wave launched a **commercial bank in Côte d'Ivoire ("Wave Bank Africa S.A.")**. **Not corroborated by a primary/top-tier source as of this date — recorded as a flag to verify, not as fact.** *([Ecofin Agency](https://www.ecofinagency.com/news-finances/2610-49842-wave-launches-commercial-bank-in-cote-d-ivoire), [Tech In Africa](https://www.techinafrica.com/wave-mobile-becomes-a-bank-with-launch-of-wave-bank-africa-s-a/).)*

**Sequencing logic (the playbook):**
1. **Lead with cash-out price disruption.** Wave entered at **~1% transfer + free cash-out vs incumbents' 5–10% on transfers**, i.e., **~70% below market**. Cash-out being *free* is the visible, viral wedge (incumbents charged heavily to withdraw). *(Verified fact; [Rest of World](https://restofworld.org/2022/how-wave-is-disrupting-francophone-africas-mobile-money-market/), [TechCabal 2025](https://techcabal.com/2025/06/30/wave-raises-137-milion/).)*
2. **Win consumer P2P + agent density**, then **layer merchant QR payments**, then **bill pay**, then **B2B/B2G payroll & disbursements**, then **credit**. Surface grew from a single send-money primitive outward. *(Inference from revenue-stack + timeline.)*
3. **Geographic:** **Senegal (anchor) → Côte d'Ivoire → Mali / Burkina Faso / The Gambia / Uganda → Cameroon**, prioritising "**regulatory-friendly**" markets and WAEMU's shared-currency, shared-regulator footprint (one EME licence → multiple countries). *(Verified fact; [TechCrunch](https://techcrunch.com/2021/09/06/sequoia-heritage-stripe-and-others-invest-200m-in-african-fintech-wave-at-1-7b-valuation/), [Dabafinance](https://www.dabafinance.com/en/news/wave-raises-137m-debt-round-mobile-money-expansion-africa-2025).)*

**Competing with the operators it rides — the most relevant fight for us.** In Senegal, **Orange Money (Sonatel)** retaliated on **two fronts**:
- **Price:** Orange cut bill-pay to 1% and dropped transfer rates toward Wave's level (reported ~0.8% at one point), while restructuring incremental P2P fees (some up to ~10% on larger amounts). The Orange exec line: *"With Wave, the price balance has been broken brutally."* *(Verified fact / quoted; [The Africa Report](https://www.theafricareport.com/156585/tech-with-wave-the-price-balance-has-been-broken-brutally-cheikh-tidiane-sarr-orange/).)*
- **Access denial:** **Sonatel blocked Wave from selling Orange airtime** for over a year (Wave could sell Free/Expresso credit). Wave escalated to the **telecoms regulator**. This shows an incumbent **weaponising network/distribution access against a fintech riding its ecosystem.** *(Verified fact; [The Africa Report](https://www.theafricareport.com/97171/senegal-cote-divoire-wave-the-fintech-thats-shaking-up-the-mobile-money-industry/), [in-24](https://new.in-24.com/News/186.html).)*

---

## What it means for us

**Transferable lessons:**

1. **The low-fee wedge works — but Wave's wedge is "free cash-out," not "free send."** Their fee sits on the *transfer*. For us (pure pass-through, payee = MSISDN), our analog of the wedge is **a radically lower all-in price to move money across networks than the incumbents' cross-net + cash-out stack.** The lesson is *pick one fee point, make everything else free, and undercut by ~70%.* Don't copy "free P2P" — that was never Wave's model.

2. **Minimal, single-purpose, QR-first app beats USSD-menu UX for smartphone users.** Wave's win is substantially a **design/UX** win over clunky telco USSD. Validates our smartphone-app-first beachhead and a deliberately bare app surface.

3. **The agent network is the moat we won't have.** Free cash-out + 150k agents is what made Wave physical and trusted — and it's **exactly what pure pass-through on pawaPay does not give us.** Wave's whole cash-in/out economy lives *outside* the telco rails on its own agents. We're betting on the operators' existing cash-in/out instead. **This is the single biggest non-transferable piece:** much of Wave's value capture (and its independence) flows from owning agents + a wallet, which our model deliberately forgoes.

4. **Held-balance + EMI licence unlocked the upside — pass-through caps ours.** Wave's expansion into merchant pay, payroll, and credit was **enabled by issuing e-money under an EME licence.** Our pass-through/no-balance design avoids that regulatory burden for v1 (good for speed) but **structurally limits later monetisation** (no float, no on-us volume, harder to layer credit). Worth deciding *consciously* that we're trading upside for v1 simplicity, and flagging the **EME/e-money licence as the gating step** if we ever want Wave-style breadth. *(Legal/licensing = flag-for-investigation per CLAUDE.md.)*

5. **Expect the operators to fight back — on price AND on access.** Orange both **slashed prices** and **cut off Wave's airtime access**, forcing regulator involvement. **We are even more exposed:** we *depend on* operator rails (via pawaPay) for both legs of every transaction. An incumbent that resents our cross-net bridging could **degrade or deny API/rail access** the way Sonatel denied airtime. **Action:** treat operator-relationship/regulatory risk as first-order; understand what protections (if any) BCC/ARPTC-equivalent and our aggregator contracts give us in DRC. This is the closest, most cautionary parallel in the whole comparable.

6. **Sequencing to borrow:** disrupt on price → win P2P + density → add merchant acceptance → bill pay → B2B disbursements → credit; expand into regulatory-friendly neighbours under a shared licence umbrella. For us, "merchant QR acceptance" is explicitly the *post-v1* expansion our product summary already names — Wave confirms it's the natural step 2.

**Non-transferable / caution flags:**
- **Pricing direction** (Wave = fee-on-send, free-cash-out) is the inverse of the brief — don't anchor on the brief's version.
- **Agent network + stored-value wallet** underpin most of Wave's economics and inclusion story; **our pass-through model has neither.**
- **WAEMU single-licence/single-currency** geography made multi-country scale cheap; **DRC (CDF, separate regulator, separate central bank)** offers no equivalent regional shortcut.
- **Profitability is unproven group-wide** and frontier-market entry runs at a loss — the model is **VC/DFI-financed land-grab**, not a self-funding template. Senegal-mature ≠ day-one DRC.
- **USSD:** Wave is **not** a precedent for self-service USSD; if our phased USSD channel matters for inclusion, we're on our own (or must build the agent-card alternative, which needs agents).

---

## Source(s)
See registry block below. Primary/high-reliability: Wave.com, Wave blog, TechCrunch, Rest of World, Quartz, TechCabal, The Africa Report, Finnfund, Global Finance. Lower-reliability / derivative (used only for non-load-bearing context, flagged inline): businessmodelcanvas*, Techjaja, Ecofin/TechInAfrica (the unverified "Wave Bank" claim), Dabafinance.

## Notes / caveats / contradictions (consolidated)
- **Pricing inverted vs brief** (fee-on-send, free cash-out) — high confidence.
- **CI launch year:** 2019 vs April 2021 — both recorded; April 2021 favoured.
- **"Wave Bank Africa S.A." in CI** — unverified by top-tier source; flagged, not asserted.
- **USSD interface claim** (one source) contradicts dominant "no USSD" reporting — not treated as fact.
- **Float as revenue** — not found / likely minor under EME safeguarding rules.
- **"12B transactions in Senegal 2022"** — implausible, excluded.
- **Profitability** — self-reported "not yet profitable, years away" (founder, ~2020–22); Uganda FY2024 loss ~$3.78M (derivative source); offset by YC top-revenue listing + self-reported >$100M revenue (2022). Internal unit economics remain **Low confidence**.
- Agent count (150k–200k+) and user count (20M+ monthly across 8 markets; ~8M Senegal) are **company-self-reported**, date-sensitive (mid-2025).

---

SOURCES_FOR_REGISTRY:
| cm-wv-001 | Wave (official site) — pricing & product | https://www.wave.com/en/ | company-primary | 2026-06-09 | high | wave.md |
| cm-wv-002 | How Wave is using Amazon's playbook to crush the competition (Rest of World) | https://restofworld.org/2022/how-wave-is-disrupting-francophone-africas-mobile-money-market/ | press-primary | 2026-06-09 | high | wave.md |
| cm-wv-003 | How Wave rose to become Francophone Africa's first unicorn (Quartz Africa) | https://qz.com/africa/2189528/how-wave-rose-to-become-francophone-africas-first-unicorn | press | 2026-06-09 | high | wave.md |
| cm-wv-004 | Wave becomes first multi-WAEMU fintech to get E-money licence (Wave blog) | https://www.wave.com/en/blog/sn-emi/ | company-primary | 2026-06-09 | high | wave.md |
| cm-wv-005 | Wave gets e-money license in Senegal (TechCabal) | https://techcabal.com/2022/04/21/wave-gets-e-money-license/ | press | 2026-06-09 | high | wave.md |
| cm-wv-006 | First E-Money License Issued To West African Fintech (Global Finance) | https://www.gfmag.com/magazine/may-2022/first-e-money-license-issued-west-african-fintech | press | 2026-06-09 | high | wave.md |
| cm-wv-007 | Sequoia Heritage, Stripe and others invest $200M in Wave at $1.7B (TechCrunch) | https://techcrunch.com/2021/09/06/sequoia-heritage-stripe-and-others-invest-200m-in-african-fintech-wave-at-1-7b-valuation/ | press | 2026-06-09 | high | wave.md |
| cm-wv-008 | Wave closes largest Series A for an African fintech ($200M) (Finextra) | https://www.finextra.com/newsarticle/38780/wave-closes-largest-series-a-round-for-an-african-fintech-with-200-million | press | 2026-06-09 | medium | wave.md |
| cm-wv-009 | How Wave's radical business model is surviving the downturn / layoffs (TechCabal) | https://techcabal.com/2022/08/05/how-wave-is-navigating-the-economic-downturn/ | press | 2026-06-09 | high | wave.md |
| cm-wv-010 | Wave only African startup on YC top-earning list 2024 (TechCabal) | https://techcabal.com/2024/05/02/wave-only-african-startup-yc-top-revenue-list/ | press | 2026-06-09 | medium-high | wave.md |
| cm-wv-011 | Wave raises $137M debt to expand mobile money (TechCabal, 2025) | https://techcabal.com/2025/06/30/wave-raises-137-milion/ | press | 2026-06-09 | high | wave.md |
| cm-wv-012 | Wave raises EUR 117M for financial inclusion (Finnfund) | https://www.finnfund.fi/en/news-and-publications/news/wave-raises-eur-117-million-to-accelerate-financial-inclusion-across-the-african-continent/ | DFI-primary | 2026-06-09 | high | wave.md |
| cm-wv-013 | "With Wave, the price balance has been broken brutally" — Orange (The Africa Report) | https://www.theafricareport.com/156585/tech-with-wave-the-price-balance-has-been-broken-brutally-cheikh-tidiane-sarr-orange/ | press | 2026-06-09 | medium-high | wave.md |
| cm-wv-014 | Senegal/CI: Wave, the fintech shaking up mobile money (The Africa Report) | https://www.theafricareport.com/97171/senegal-cote-divoire-wave-the-fintech-thats-shaking-up-the-mobile-money-industry/ | press | 2026-06-09 | medium-high | wave.md |
| cm-wv-015 | The price war rages between Orange Money and Wave in Senegal (in-24) | https://new.in-24.com/News/186.html | press | 2026-06-09 | low-medium | wave.md |
| cm-wv-016 | Wave raises $137M debt for expansion across Africa (Dabafinance) | https://www.dabafinance.com/en/news/wave-raises-137m-debt-round-mobile-money-expansion-africa-2025 | press | 2026-06-09 | medium | wave.md |
| cm-wv-017 | A Zero-Fee Mobile Banking Model in Côte d'Ivoire (TriplePundit) | https://triplepundit.com/2025/wave-mobile-money-cote-divoire/ | press | 2026-06-09 | medium | wave.md |
| cm-wv-018 | Wave Mobile Money in Uganda: services, charges, agents (Techjaja) | https://techjaja.com/wave-mobile-money-in-uganda-explained-servies-charges-and-agents/ | press | 2026-06-09 | low-medium | wave.md |
| cm-wv-019 | How Does Wave Mobile Money Work (businessmodelcanvas — derivative) | https://businessmodelcanvastemplate.com/blogs/how-it-works/wave-mobile-money-how-it-works | derivative | 2026-06-09 | low | wave.md |
| cm-wv-020 | Wave Launches Commercial Bank in Côte d'Ivoire (Ecofin Agency) — UNVERIFIED claim | https://www.ecofinagency.com/news-finances/2610-49842-wave-launches-commercial-bank-in-cote-d-ivoire | press | 2026-06-09 | low | wave.md |
