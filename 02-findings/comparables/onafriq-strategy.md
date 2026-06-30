# Onafriq (formerly MFS Africa) — strategy lens

**What it is to us:** the closest *structural* analog — a cross-network / cross-border interoperability hub ("network of networks") that, like us, does **not own the endpoints** (the wallets) yet built a business on bridging them. This file covers **strategy**: product structure, monetisation/value capture, and feature sequencing. DRC coverage/capability is in `../aggregators/onafriq.md` (not duplicated here).

- **Last updated:** 2026-06-09
- **Tooling note:** WebFetch was permitted only for `onafriq.com`; third-party sources (FT Partners, podcasts, press) were read via WebSearch result summaries. Several primary financial figures are genuinely **not publicly disclosed** — flagged below as "Not found."
- **Overall confidence:** Medium-High for structure & sequencing (well documented); **Low for exact unit economics** (take-rate / bps not public).

---

## Lens 1 — Product structure & architecture (B2B "network of networks")

**The core architecture is hub-and-spoke.** Onafriq sits as a single neutral hub between fragmented, non-interoperable endpoints — mobile money schemes, banks, card networks — and lets any connected party reach any other through **one integration**. It describes the model as "an omnichannel network of networks … with pathways from and to every African and every African business," grounded in **"any-to-any interoperability"** ([rebrand announcement, onafriq.com](https://onafriq.com/press/article/mfs-africa-announces-rebrand-to-onafriq); founder framing in [Goodbye MFS Africa, Hello Onafriq, Medium](https://dokoudjou.medium.com/goodbye-mfs-africa-hello-onafriq-65650e60a4ce)). **Verified fact (self-described model), High confidence.**

**It is explicitly B2B — it serves institutions, not consumers directly.** Connected segments are: mobile network operators; banks and fintechs; money transfer operators; global enterprises; online/offline merchants; development organisations ([rebrand announcement](https://onafriq.com/press/article/mfs-africa-announces-rebrand-to-onafriq)). The end consumer touches a *partner's* product (e.g. a telco wallet, or Visa Pay); Onafriq is the rails underneath. **Verified fact, High confidence.** This is the single biggest divergence from our consumer model (see "What it means for us").

**The product set has been assembled into four pillars**, per the rebrand and homepage:
| Pillar | What it is | How acquired/built |
|---|---|---|
| **Collections & disbursements** (domestic + cross-border) | Pay-ins and payouts across wallets/banks/corridors — the original hub | Built organically; deepened domestically via **Beyonic** |
| **Card issuing & processing** | Issuer-processing + tokenisation linking mobile-money wallets to Visa/Mastercard rails | **GTP** acquisition + **Visa** partnership |
| **Agency banking** (cash-in/cash-out, last mile) | Physical agent network, offline touchpoints | **Baxi** acquisition (Nigeria) |
| **Treasury services** | Multi-currency liquidity / FX across markets | Built; called out as a funded priority in Series C |

([rebrand announcement](https://onafriq.com/press/article/mfs-africa-announces-rebrand-to-onafriq); [Series C, onafriq.com](https://onafriq.com/press/article/mfs-africa-100-million-series-c-fund-raising)). **Verified fact (offerings), High confidence.**

**Architectural insight relevant to us:** the moat is *not* the API — it's the **number of pre-integrated, pre-licensed, pre-settled endpoints** behind the single integration. A partner connects once and reaches ~40+ markets; replicating that web of bilateral integrations, regulatory permissions and settlement relationships is the barrier. **Inference, Medium-High confidence.**

---

## Lens 2 — Monetisation & value capture

This is the lens most directly tied to our **thin-pass-through-margin worry**, and also the one with the **least public disclosure**.

**What is documented (the *where* of revenue):** Onafriq monetises across all four pillars — transaction fees on collections/disbursements across corridors, card issuing/processing fees and **tokenisation** for the mobile-money-to-card bridge, agency-banking transaction revenue, and **treasury/FX** ([rebrand announcement](https://onafriq.com/press/article/mfs-africa-announces-rebrand-to-onafriq); [card-&-mobile interoperability insight, onafriq.com](https://onafriq.com/insights/article/why-card-and-mobile-money-interoperability-are-critical-to-empowering-african-consumers-and-entrepreneurs)). **Verified fact (revenue lines exist), High confidence.**

**What is NOT public (the *how much*):** exact **take-rate, basis points, or per-transaction fee** is **Not found** in any primary or reputable source. The Series C announcement gives **no** revenue or margin figures ([Series C, onafriq.com](https://onafriq.com/press/article/mfs-africa-100-million-series-c-fund-raising)); company/aggregator profiles also lack fee detail ([CB Insights summary](https://www.cbinsights.com/company/mfs-africa/financials)). **"Not found," and we should not infer a number.** For external context only (a *proxy*, not Onafriq's take): the average cost of sending remittances to/within Africa was cited at **~7.43% in 2020** (per search summary citing remittance-cost data) — an end-user *price* benchmark, **not** Onafriq's margin. **Labelled proxy, Low confidence.**

**The strategic logic of value capture (the important part for us):** an interoperability layer that doesn't own endpoints captures value by (a) operating at **volume across many thin corridors**, and (b) **stacking margin pools** so it isn't reliant on the thinnest one:
- **FX is a higher-margin layer than raw transfer.** Cross-border movement lets Onafriq monetise the **currency conversion**, not just the rail — hence treasury/liquidity being a named funded priority. **Inference (consistent with cross-border PSP economics + treasury emphasis), Medium confidence.**
- **Card issuing/BIN sponsorship + tokenisation adds a scheme-economics layer** that pure wallet-to-wallet interop lacks. GTP is "the number one processor for prepaid cards in Africa," with **80+ banks** and connectivity to **Visa, Mastercard, GIM, GIMAC and Verve**, bought for **US$34M** in cash and shares ([GTP deal summary, FT Partners via search](https://www.ftpartners.com/transactions/mfs-africa-gtp); [BusinessDay](https://businessday.ng/companies/article/mfs-africa-acquires-us-based-tech-company-for-34-million/)). Issuer-processing is a recurring, per-card/per-transaction revenue base that is **stickier** than corridor flow. **Verified fact (GTP scale/price); Medium confidence on margin characterisation.**
- **Scale is the answer to thin margins.** The founder is described as "candid that Onafriq has reached a size where it can sustain this indefinitely as a commercial proposition" ([F-Squared Podcast #24 summary, Frontier Fintech](https://frontierfintech.substack.com/p/f-squared-podcast-24-building-the)). Reported cumulative transactional value of **US$1.8bn** over one investor's holding period ([FSD Africa](https://fsdafrica.org/investment/mfs-africa-now-onafriq/)); the network spans **600+ corridors** and (current) **~1bn wallets / 43 markets** ([Series C: 320M wallets, 180+ schemes, 250+ enterprises](https://onafriq.com/press/article/mfs-africa-100-million-series-c-fund-raising); current scale via [search summaries](https://onafriq.com/)). **Self-reported / third-party-reported, Medium confidence.**

**Read-through:** a bridge that captures only a sliver of each transfer *can* be durable — but Onafriq's durability rests on (i) enormous volume and (ii) **adding higher-margin layers on top of the thin transfer fee** (FX, card economics, treasury). A pure single-layer pass-through at small scale is the hardest version of this business. **Inference, Medium confidence — central to our model.**

---

## Lens 3 — Feature rollout & sequencing

Onafriq built the B2B hub over roughly **15 years**, sequencing **hub → omni-channel network**, with growth heavily by **acquisition**:

| ~Date | Move | What it added / sequencing logic |
|---|---|---|
| Founding → 2010s | **Mobile-money interoperability hub** | Core: connect MNO wallets across borders via one API ([FSD Africa](https://fsdafrica.org/investment/mfs-africa-now-onafriq/)) |
| **2019** | **Visa partnership** | Connected the hub to the Visa network "for card issuing at scale" — first card-rail bridge ([card-&-mobile interoperability insight](https://onafriq.com/insights/article/why-card-and-mobile-money-interoperability-are-critical-to-empowering-african-consumers-and-entrepreneurs)) |
| **2021** | **Beyonic** acquired | Domestic **business/SME** collections-disbursements + payroll; bridged "remittance hub" → "serve African entrepreneurs." Beyonic connected 26 MM networks + 20+ banks across 9 countries ([Beyonic announcement, onafriq.com](https://onafriq.com/press/article/mfs-africa-acquires-beyonic-bringing)) |
| **2021–Apr 2022** | **Baxi** (Capricorn Digital) acquired | **Offline / last-mile** node in Nigeria: 90,000+ agents, cash-in/cash-out, bill pay, account opening. Logic: Nigeria is offline-first, so own the physical touchpoint ([TechCabal](https://techcabal.com/2021/10/20/mfs-africa-acquires-baxi/); [Baxi completion, onafriq.com](https://onafriq.com/press/article/mfs-africa-completes-capricorn)) |
| **Jun 2022** | **GTP** acquired (US$34M) | **Card issuer-processing** at scale — 80+ banks, 34 countries, Visa/Mastercard/GIM/GIMAC/Verve. Logic: own the card-issuing layer, link cards to wallets ([GTP, FT Partners summary](https://www.ftpartners.com/transactions/mfs-africa-gtp)) |
| **~2021–22** | **US$100M Series C** | Funded talent, **GRC/compliance**, **treasury & liquidity pool**, further fintech investment; opened Abidjan/Kampala/**Kinshasa**/Nairobi/Lagos offices ([Series C, onafriq.com](https://onafriq.com/press/article/mfs-africa-100-million-series-c-fund-raising)) |
| **Nov 2022** | **Uganda licences** (PSP, PSO, IPI) | Regulatory build-out to operate as issuer/operator, not just via partners ([acquisitions summary via search](https://onafriq.com/)) |
| **Nov 2023** | **Rebrand MFS Africa → Onafriq** | Signalled omni-channel repositioning beyond "mobile financial services"; also the MFS trademark was unusable outside Africa ([rebrand announcement](https://onafriq.com/press/article/mfs-africa-announces-rebrand-to-onafriq); [founder Medium post](https://dokoudjou.medium.com/goodbye-mfs-africa-hello-onafriq-65650e60a4ce)) |
| **Sep 2025** | **Visa Pay launch in DRC** | Bridged Visa cards ↔ M-Pesa/Airtel/Orange wallets in DRC, powered by Onafriq collections/disbursements APIs ([Visa Pay DRC, onafriq.com](https://onafriq.com/press/article/onafriq-and-visa-partner-to-launch-visa-pay-unlocking-interoperability-between-card-and-mobile-money-in-the-drc)) |

**Sequencing pattern:** start with the highest-leverage interoperability primitive (cross-border wallet transfer) → **add adjacent rails one acquisition at a time** (business payments, offline cash, cards) → wrap with treasury + regulatory licences → rebrand once the "single rail" story no longer fits. Each acquisition both **widened the endpoint web** (the moat) and **added a margin pool** (Lens 2). **Verified fact (sequence); inference on pattern, High/Medium confidence.**

---

## What it means for us

**1. Can a cross-network bridge be a durable, monetisable business?** Yes — Onafriq is the existence proof. But its durability is built on three things a thin single-layer pass-through lacks:
- **Scale across many corridors** (volume compensates for thin per-transaction margin) — founder calls the model commercially self-sustaining *at its size*.
- **Stacked margin layers** beyond the transfer fee — especially **FX** and **card/issuer-processing economics**. The raw wallet-to-wallet hop is the *thinnest* layer; Onafriq deliberately doesn't depend on it alone.
- **A web of pre-integrated, pre-licensed, pre-settled endpoints** that is expensive to replicate — the real moat, not the API.

**2. The moat raw operator interop lacks.** Operators interoperating bilaterally (or via a national switch) give users cross-network transfer *without* a third party. Onafriq's defensibility over that is **breadth + neutrality + adjacent rails**: any-to-any across 40+ markets and into card networks, which no single operator or national switch offers. **For us, this is the warning:** if DRC operators (or a BCC-mandated national switch) achieve domestic interop directly, a pure domestic cross-network pass-through has a *weak* moat. Our defensibility would have to come from **UX, channel reach (USSD/feature-phone), and bundling**, not from owning interop itself. **Inference, Medium confidence — flag for the decision.**

**3. B2B vs. our consumer model — the biggest non-transfer.** Onafriq monetises *institutions* (banks/MNOs/fintechs pay for access), so it never fights for consumer trust, CAC, or support at the retail edge — and it earns on **wholesale** rails plus card/treasury layers we won't have. We are **consumer-facing pass-through on rented rails (pawaPay)**: we sit *below* Onafriq's customers, paying an aggregator's take, then trying to add our own thin margin on top of a consumer price. **Implication:** we cannot copy Onafriq's monetisation directly; volume-at-wholesale-scale and card/FX/treasury margin pools are largely **out of reach in v1**. Our realistic value capture is **convenience pricing + future adjacent layers** (e.g. our own future card/bill-pay/agent layer), echoing Onafriq's *sequencing* logic even if not its B2B economics.

**4. Transferable lessons (de-risked):**
- **Sequence like Onafriq:** ship the one high-leverage primitive first (cross-network send to any MSISDN), then add adjacent rails (bill pay, cash-in/out via agents, cards) once volume exists — don't build the omni-channel stack up front.
- **Plan a margin-stacking path early**, because pure pass-through margin is structurally thin (Onafriq's whole history confirms this) — identify which higher-margin layer (FX on USD/CDF, future card issuance, agent cash) we could add.
- **Baxi validates our phased USSD/feature-phone + agent channel:** Onafriq paid real money to *own* the offline/last-mile node because mobile-first doesn't reach everyone — directly supportive of our feature-phone track being first-class, not an afterthought.

**5. Scale/B2B specifics that may NOT transfer:** the ~1bn-wallet network, 600+ corridors, US$34M card-processor acquisition, BIN sponsorship, multi-market treasury pool, and ~US$100M+ funding base are **wholesale/scale assets** unavailable to an MVP. Treat Onafriq's *economics* as aspirational and its *sequencing + moat logic* as the actually-transferable insight. **Inference, Medium-High confidence.**

---

## Confidence & open questions
- **Exact take-rate / per-transaction fee / FX margin:** **Not found** publicly — do not infer a number for our model from Onafriq.
- **Revenue split across the four pillars** (how much is transfer vs. card vs. treasury): **Not found.**
- **Whether Visa Pay in DRC is a product Onafriq *sells* vs. a *demonstration* of its underlying APIs** (carried over from `../aggregators/onafriq.md`) — affects whether the DRC bridge is replicable for us.
- **Profitability / current revenue:** not publicly disclosed; "commercially self-sustaining at scale" is founder commentary (self-reported), not audited.
- Dates for Beyonic vs. Baxi announcements vary slightly across sources (2021 vs. early 2022) — recorded as a range; not material to the strategy read.

[[onafriq]] · [[../reports/comparables-synthesis]] · [[../../01-questions/comparables]]

---

SOURCES_FOR_REGISTRY:
| cm-on-001 | MFS Africa Announces Rebrand to Onafriq | https://onafriq.com/press/article/mfs-africa-announces-rebrand-to-onafriq | company primary (press) | 2026-06-09 | High (primary, self-reported) | onafriq-strategy.md |
| cm-on-002 | Goodbye MFS Africa, Hello Onafriq (Dare Okoudjou) | https://dokoudjou.medium.com/goodbye-mfs-africa-hello-onafriq-65650e60a4ce | founder blog | 2026-06-09 | Medium (founder self-reported) | onafriq-strategy.md |
| cm-on-003 | MFS Africa Acquires Beyonic | https://onafriq.com/press/article/mfs-africa-acquires-beyonic-bringing | company primary (press) | 2026-06-09 | High (primary) | onafriq-strategy.md |
| cm-on-004 | Why Card and Mobile Money Interoperability Are Critical | https://onafriq.com/insights/article/why-card-and-mobile-money-interoperability-are-critical-to-empowering-african-consumers-and-entrepreneurs | company primary (insight) | 2026-06-09 | Medium-High (primary, marketing) | onafriq-strategy.md |
| cm-on-005 | MFS Africa $100M Series C Fund Raising | https://onafriq.com/press/article/mfs-africa-100-million-series-c-fund-raising | company primary (press) | 2026-06-09 | High (primary) | onafriq-strategy.md |
| cm-on-006 | FT Partners Advises Onafriq on Acquisition of GTP | https://www.ftpartners.com/transactions/mfs-africa-gtp | advisor/transaction page | 2026-06-09 | Medium-High (reputable advisor) | onafriq-strategy.md |
| cm-on-007 | MFS Africa acquires US-based tech company for $34 million | https://businessday.ng/companies/article/mfs-africa-acquires-us-based-tech-company-for-34-million/ | press | 2026-06-09 | Medium (reputable press) | onafriq-strategy.md |
| cm-on-008 | Baxi acquisition deal sees MFS Africa expand into Nigeria | https://techcabal.com/2021/10/20/mfs-africa-acquires-baxi/ | press | 2026-06-09 | Medium (reputable press) | onafriq-strategy.md |
| cm-on-009 | MFS Africa completes Capricorn (Baxi) acquisition | https://onafriq.com/press/article/mfs-africa-completes-capricorn | company primary (press) | 2026-06-09 | High (primary) | onafriq-strategy.md |
| cm-on-010 | MFS Africa (now Onafriq) — investment profile | https://fsdafrica.org/investment/mfs-africa-now-onafriq/ | investor/DFI profile | 2026-06-09 | Medium-High (investor-reported) | onafriq-strategy.md |
| cm-on-011 | F-Squared Podcast #24: Building the Rails for African Trade | https://frontierfintech.substack.com/p/f-squared-podcast-24-building-the | podcast/newsletter | 2026-06-09 | Low-Medium (secondary, interview) | onafriq-strategy.md |
| cm-on-012 | Onafriq & Visa Partner to Launch Visa Pay in DRC | https://onafriq.com/press/article/onafriq-and-visa-partner-to-launch-visa-pay-unlocking-interoperability-between-card-and-mobile-money-in-the-drc | company primary (press) | 2026-06-09 | High (primary) | onafriq-strategy.md |
