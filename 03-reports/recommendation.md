# Recommended integration path for the MVP

- **Date:** 2026-06-04
- **Author:** Claude Code (agentic research)
- **Built from:** [feasibility-summary.md](feasibility-summary.md), [operator-api-comparison.md](operator-api-comparison.md), [aggregator-comparison.md](aggregator-comparison.md), [fees-and-costs.md](../02-findings/cross-cutting/fees-and-costs.md), [payee-identification.md](../02-findings/cross-cutting/payee-identification.md), [kyc-and-onboarding.md](../02-findings/cross-cutting/kyc-and-onboarding.md)

> **Reframed 2026-06-11 (team pivot to a merchant-facing MVP).** The integration path below is unchanged — pawaPay's two-leg collect→settle engine is what a merchant-acquiring product runs on too. What changed is the *audience and flow*: a **customer pays a merchant**, the **merchant absorbs the fee (MDR)**, the customer pays the exact sticker price, settlement is instant pass-through to the merchant, and the **primary customer action is USSD** (scan the merchant's QR / dial the till), not a payer keying a number into a consumer app. **Merchant onboarding is now in scope.** Technical facts and the cost table are unchanged. See [`00-overview/product-summary.md`](../00-overview/product-summary.md).

## Bottom line — the recommended path

**Build the MVP on pawaPay as the single aggregator, settling to merchants by MSISDN (`+243`-prefixed) on Vodacom M-Pesa, Airtel Money, and Orange Money — covering ~99% of DRC mobile money subscribers.** Each sale is a two-leg flow: **collect from the customer → settle to the merchant**, bridging networks when they differ. Defer Afrimoney support to later (and, if it becomes commercially material, evaluate MOKO Afrika as the Afrimoney-only fallback path rather than re-platforming). Do **not** integrate operators directly for the MVP; the direct path takes four contracts and four opaque pricing conversations to replicate something pawaPay already publishes. **Open a separate legal/licensing track immediately** to determine whether the MVP entity needs a BCC-registered partner before live launch (now including merchant onboarding/KYC).

## Detail

### Why pawaPay as the lead

1. **Coverage matches the goal.** Three of four DRC mobile money operators (Vodacom M-Pesa, Airtel, Orange) account for **~99% of DRC mobile money subscribers** per third-party share estimates ([rdc-analyse.org feasibility analysis, 2025-08](https://rdc-analyse.org/files/2025/08/2025_08_CAT_Feasibility-analysis-of-Mobile-Money-in-eastern-DRC_VE-1.pdf)). Afrimoney is a hard zero on pawaPay but the addressable share lost is small.
2. **Publicly published pricing.** pawaPay is the only researched aggregator that publishes per-provider DRC fees ([pawapay.io/fees](https://www.pawapay.io/fees)). This lets us model unit economics today without a sales conversation. DPO, MOKO, and Onafriq don't publish their DRC rates.
3. **Self-serve sandbox and time-to-live.** Sandbox at `dashboard.sandbox.pawapay.io` is open immediately; production via KYC questionnaire and use-case review ([pawaPay Plans page](https://www.pawapay.io/plans)). No setup or monthly fees on Standard. Time-to-live is days–weeks if onboarding goes smoothly.
4. **API surface fits the use case.** Deposits (collection), Payouts (disbursement), Refunds, batch disbursements, transaction status query, dashboard, and treasury services for settlement — all the moving parts the two-leg engine needs ([pawaPay product overview](https://www.pawapay.io/product-overview)).
5. **Risk fit.** DRC providers do **not** require Enterprise qualification on pawaPay (the Enterprise-gated list is Kenya M-Pesa, Mozambique M-Pesa/Movitel, Nigeria Airtel/MTN, Tanzania Vodacom, Benin Celtiis betting, Ivory Coast Orange — not DRC) ([pawaPay Plans page](https://www.pawapay.io/plans)). Standard plan unlocks DRC end-to-end.

### Why not the other aggregators (for MVP)

- **MOKO Afrika** is the **only researched aggregator covering all four DRC MNOs including Afrimoney**, with technical docs that are quite explicit (JWT auth, HMAC-SHA256 callbacks, +243 phone validation built in). It is a serious option, particularly DRC-focused, and listed clients include Equity BCDC and several large betting operators ([mokoafrika.com](https://www.mokoafrika.com/en); [moko-africa-documentation.vercel.app](https://moko-africa-documentation.vercel.app/)). The reason to defer it from the lead spot: **pricing is opaque** (no public tariff) and **some doc sub-pages were inconsistently available** during this research. The risk of a four-MNO integration that we can't model financially upfront, plus less international visibility, makes pawaPay the safer MVP lead. **Keep MOKO Afrika as the v2 escalation if Afrimoney coverage becomes material.**
- **DPO Pay** is real and well-established (Network International parent), but its public shape is **card-first / collection-first / gateway-shaped**: real-time per-transaction mobile-money payout granularity is less clearly documented, with bulk-file payouts being the documented disbursement pattern ([dpogroup.com](https://dpogroup.com/); [docs.dpopay.com llms.txt](https://docs.dpopay.com/llms.txt)). For an MVP whose core flow is per-payment payouts to merchants, this is a worse shape than pawaPay's.
- **Onafriq** is targeted at PSPs and financial institutions (e.g., the Visa Pay partnership with Visa for DRC card-and-wallet interoperability launched 2025-09-11) ([Onafriq Visa Pay DRC press release](https://onafriq.com/press/article/onafriq-and-visa-partner-to-launch-visa-pay-unlocking-interoperability-between-card-and-mobile-money-in-the-drc)). Powerful, but over-engineered for a v1 merchant-acceptance MVP, and its DRC pricing is opaque.

### Why not direct operator integration (for MVP)

- **Four contracts, four KYC packs, four onboarding cycles.** M-Pesa via `business.m-pesa.com/vodacom-drc/onboarding-your-business-in-drc/`; Orange Money via Orange store + `Serviceclients.RDC@orange.com`; Airtel via `developers.airtel.africa/developer` plus per-country contract; Afrimoney via `info@africell.cd`. The Airtel piece is comparatively self-serve, but the others require commercial sales conversations.
- **None publishes DRC API per-leg fees**, so unit economics cannot be modelled without partnership conversations. M-Pesa's onboarding doc states this explicitly.
- **Orange Money DRC does not appear to publish a B2C/payout API** — collection-only via `developer.orange.com/apis/om-webpay`. This forces an aggregator anyway, or an off-channel workaround, for the Orange payout leg.
- **Direct integration is the right v2 play** for cost reduction and bargaining power once volume justifies it, **not the v1 play**.

### The MVP user flow on pawaPay (merchant-facing)

**Primary flow — customer-initiated USSD (no customer app, no internet):**
1. **The customer pays the exact sticker price.** They either **scan the merchant's QR** — a `tel:` USSD dial-through (e.g. `tel:*123*1001%23`) that the phone's dialer runs — or **dial the till short-code** (`*123*1001#`) and enter the amount. No customer app is needed; this works on a feature phone. (Currency is CDF or USD.)
2. **Our backend calls pawaPay collection (deposit) API** for the customer's mobile number on the customer's operator. pawaPay triggers a **PIN prompt on the customer's phone in their operator's own UX** (M-Pesa PIN, Airtel Money PIN, Orange Money PIN). The customer enters their PIN there.
3. **Funds land in our pawaPay balance.**
4. **Our backend calls pawaPay payout (disbursement) API** to settle to the **merchant's** mobile number on the merchant's operator — **instant pass-through, we never hold funds.** The merchant receives a **normal incoming transfer on their existing operator wallet** plus the operator's confirmation SMS, and sees the sale reconciled in the merchant dashboard. The merchant nets sticker price − MDR.
5. **Both legs typically complete in seconds; aggregator-to-bank settlement (if the merchant later opts into bank payout) happens on pawaPay's cadence** (not publicly stated for DRC; 2–5 BD is the DRC PSP norm per [payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd)).

**Fallback flow — merchant-initiated "charge by customer number":** the merchant keys the customer's mobile number and amount into the merchant app, which fires the same collection→settlement two legs (the customer still approves via their operator's PIN prompt). Useful when a customer can't scan/dial.

### Merchant onboarding (now in scope)

The MVP onboards merchants: register the merchant, their **settlement mobile-money account**, and a **till / short-code** for the USSD + QR flow. **Merchant KYC is a flagged item** (pawaPay's own merchant KYC questionnaire applies to us as their sub-merchant; whether we must collect additional merchant KYC is part of the legal/licensing track). The launch wedge is ~40 gas stations + pop-up stores already lined up, which substantially de-risks merchant acquisition for v1.

### Cost picture (per cross-network round-trip, before FX)

| Route | Fee | Source |
|---|---|---|
| M-Pesa → Orange | **3.5%** | pawaPay published |
| Orange → Orange | 4.0% | pawaPay published |
| Airtel → Orange | 4.0% | pawaPay published |
| M-Pesa → M-Pesa or Airtel | 4.5% | pawaPay published |
| Airtel → M-Pesa or Airtel | **5.0%** | pawaPay published |
| Orange → M-Pesa or Airtel | 5.0% | pawaPay published |

- **Add FX 2–5%** for currency switch ([payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd)).
- **Settlement to bank: 1,000–3,000 CDF (~$0.35–$1.10)** per payout per DRC PSP norm — vendor-specific cadence not public.
- **No pawaPay setup or monthly fees on Standard plan.**

**Who bears this — and the pricing gap.** This 3.5–5% round-trip is our **cost of goods**. In the merchant-facing model the **merchant absorbs the fee (MDR)** — the customer always pays the exact sticker price. The current **MDR placeholder is 1%, which is *below* this cost**; a viable MDR is ~5–7%+ to clear the round-trip plus FX and a margin. **The MDR is an open pricing decision** (does the merchant accept a 5–7% take rate, do we subsidise low-value tickets, do we price by route?), and it must be resolved before launch.

**Implication for ticket size:** at a cost-covering MDR, the percentages are comfortably absorbable on $20+ tickets, tight on $10–$20, and punishing below $10 — so very small tickets either carry a higher MDR, get subsidised as merchant-acquisition cost, or are deferred. Gas-station fills (the launch wedge) typically sit in the absorbable band.

### KYC posture

- **Customer KYC** rides their existing SIM/operator wallet KYC (DRC mandates SIM registration; tier-1 wallets allow up to ~$100; tier-2 up to $3,000; daily/monthly limits apply — see [kyc-and-onboarding.md](../02-findings/cross-cutting/kyc-and-onboarding.md)). The customer leg stays ID-light because we never hold their funds.
- **App-side ID** can stay light while the MVP is pure pass-through (no held balance). If we add a custodial wallet later, plug **Smile ID** (or similar) for OCR + selfie verification of the standard DRC ID set (national ID, passport, driver's licence, voter card).
- **Merchant KYC** — now in scope because we onboard merchants. pawaPay applies its own merchant questionnaire + use-case review to us as their sub-merchant; whether we must collect *additional* merchant KYC (and to what tier) is a flagged item on the legal/licensing track.

### Customer pay-action & merchant addressing

- **The customer's pay action is USSD-first:** scan the merchant's **QR (a `tel:` USSD dial-through)** or **dial the till short-code** — both work on a feature phone with no customer app, across all three networks, because the operator's own PIN prompt handles approval. This QR/till is the **primary MVP flow**, not a later add-on.
- **No DRC *national* QR standard exists** ([payee-identification.md](../02-findings/cross-cutting/payee-identification.md)), so we deliberately avoid a proprietary-scanner QR; the `tel:` dial-through sidesteps that (any dialer runs it) while resolving to the merchant's till on our side.
- **Merchants are addressed by MSISDN** on the settlement leg (operator inferred from prefix), and by their assigned **till / short-code** for the customer-facing flow.
- See [payee-identification.md](../02-findings/cross-cutting/payee-identification.md) and [`offline-ussd-feasibility.md`](../02-findings/cross-cutting/offline-ussd-feasibility.md).

### What to investigate in parallel (separate tracks)

1. **Legal/licensing.** Whether the MVP entity needs a BCC-registered partner (or its own EMI/PSP licence) is the single biggest unknown. PayAtlas summarises DRC as "Foreign PSPs must partner with licensed local entities or obtain BCC authorisation; independent operation is prohibited" ([payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd) — secondary). **Engage counsel; verify against BCC primary docs.** Per `00-overview/research-goals.md`, this is a flag-to-investigate item, not a requirement.
2. **Real sandbox pilot.** Stand up the pawaPay sandbox, run end-to-end with 1–2 real DRC SIMs across operators, measure success rates, time-to-final-confirmation, error patterns. This converts "should work" into "does work."
3. **Pricing conversations with MOKO, DPO, Onafriq.** Get real quotes to confirm pawaPay's published rates are competitive (or to identify cheaper paths if Afrimoney coverage matters).
4. **Afrimoney status.** Track whether Afrimoney's Dec 2023 GSMA-spec API rollout is live in DRC and whether pawaPay will add it.
5. **BCC interoperability stance.** Confirm the 2014 "no mandate" position is still current; check for newer guidance.

## Comparison / key data

### Concise vendor recommendation matrix

| Vendor | Use? | Why |
|---|---|---|
| **pawaPay** | **Yes — MVP lead** | Public DRC pricing; self-serve sandbox; 3 of 4 MNOs; no setup/monthly fees |
| **MOKO Afrika** | **Reserve — v2 escalation for Afrimoney** | Only researched aggregator with all 4 MNOs; technical docs explicit; pricing opaque |
| **DPO Pay** | No (for MVP) | Card-/gateway-shaped; bulk-file payouts; opaque pricing; heavier onboarding |
| **Onafriq** | No (for MVP) | PSP-to-PSP / financial-institution shape; over-engineered for v1; opaque pricing |
| **Direct operators** | No (for MVP); **yes for v2** | 4 contracts and opaque pricing across them all; right answer once volume justifies it |

## Confidence & open questions

- **Confident about:**
  - The technical path (pawaPay collection + payout, two-leg) works end-to-end — and is the same engine the merchant-facing model needs (collect from customer → settle to merchant).
  - 3 of 4 DRC MNO coverage on pawaPay is verified by primary docs.
  - Round-trip cost envelope (3.5%–5.0%) is verifiable — and is the merchant-absorbed MDR's cost floor.
  - MOKO Afrika is the right Afrimoney fallback.
  - Direct operator integration is wrong for v1.

- **Uncertain / unverified:**
  - The **MDR / pricing decision** — the merchant absorbs the fee, but the 1% placeholder is below the 3.5–5% cost; the real MDR (~5–7%+) is an open pricing decision, not a research finding.
  - Whether a **`tel:` USSD dial-through QR and dialable till short-code** behave consistently across all three operators' menus, and per-operator shortcode provisioning/lead times (see [`offline-ussd-feasibility.md`](../02-findings/cross-cutting/offline-ussd-feasibility.md)).
  - Whether the MVP entity needs a BCC-registered partner, and what **merchant onboarding/KYC** we must collect (legal track).
  - Settlement cadence to a partner bank account on pawaPay.
  - Whether DPO/MOKO/Onafriq quote significantly cheaper than pawaPay (would change the lead).
  - Real-world success rates and UX timing — must be measured in sandbox.
  - DRC FX markup at pawaPay versus the 2–5% PSP norm.

- **Still missing (and how to get it):**
  - Sandbox-pilot results — sign up at `dashboard.sandbox.pawapay.io` and run real flows.
  - Legal review of BCC posture — counsel + BCC primary docs.
  - Real quotes from DPO, MOKO, Onafriq — sales contacts.
  - Confirmation of Afrimoney API live status in DRC — `info@africell.cd`.
  - Vodacom DRC and Airtel DRC direct-API rate cards (for v2 modelling) — partner contacts.

## Sources
By ID, from `04-sources/sources.md`:
- pp-001..pp-008 (pawaPay — primary)
- mk-001..mk-005 (MOKO Afrika — Afrimoney fallback)
- dp-001..dp-007 (DPO Pay)
- on-001..on-007 (Onafriq)
- mp-001..mp-012; om-001..om-009; am-001..am-009; af-001..af-006 (operators)
- xo-001..xo-018 (cross-cutting: interop, market structure, KYC, regulatory, additional aggregators)
