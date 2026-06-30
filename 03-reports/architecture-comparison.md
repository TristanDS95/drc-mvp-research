# Architecture comparison — merchant-acquiring QR (now chosen) vs a consumer cross-network pass-through (original spec, superseded)

- **Date:** 2026-06-09 — **reframed 2026-06-11 after the team pivot (see reversal note below)**
- **Author:** Claude Code (agentic research) — produced via a 24-agent verification workflow (each dimension analysed then adversarially verified against the spec files, prior findings, and live web sources)
- **Built from:** [05-product-spec/](../05-product-spec/) (the original consumer architecture), an external "DRC Payment Aggregator — Solutions Architecture Brief" supplied for review (the merchant-acquiring direction), and [02-findings/](../02-findings/) + [03-reports/recommendation.md](recommendation.md)

> ## ⚠️ STATUS — read first (DECISION REVERSED, 2026-06-11)
> **This report originally argued to KEEP the consumer cross-network pass-through and AGAINST the merchant-acquiring QR brief (verdict 7 "ours" / 3 "mixed" / 0 "theirs"). After the team pivot on 2026-06-11, that decision is reversed: the merchant-acquiring / QR direction the brief described is now substantially what we are building** (see [`00-overview/product-summary.md`](../00-overview/product-summary.md)). What the original analysis faulted as *anti-patterns* — merchant onboarding, a merchant dashboard, a merchant QR — are now **in-scope MVP features**. The body below has been reframed so that **"Ours (chosen)" = the merchant-acquiring direction** and **"Original spec (superseded)" = the consumer pass-through in [`05-product-spec/`](../05-product-spec/)** (which now needs rework to match the pivot). The *technical* corrections this review produced still hold regardless of audience and are preserved verbatim (pawaPay = RFC-9421 not HMAC; AWS App Runner absent in af-south-1; the Kinshasa latency figure; Flutterwave does not serve DRC mobile money). **This is not a rewrite of history — it records that we considered merchant-acquiring, initially rejected it, and then chose it after the pivot, and why the original objections no longer bind.**

## Bottom line
**The merchant-acquiring / QR direction is now our chosen MVP.** The original report's tally (7 "ours [consumer]," 3 "mixed," 0 "theirs [merchant]") was scored against the *old* consumer charter and is **superseded** — most of the dimensions it scored "ours" hinged on fixed decisions ("no merchant onboarding," "one consumer app," "pay any number") that the pivot **deliberately reversed**. The substantive product point survives but inverts: the two are genuinely *different products*, and we have now chosen the **merchant-acquiring** one (merchants onboard, show a QR / till short-code, customer pays by USSD or the merchant app, money settles to the merchant; the customer needs no app). The external brief is still optimised for a fast investor *demo* rather than a launchable system, so we adopt its **direction** while keeping our own production-grade engineering (two-leg collect→settle, ledger, reconciliation) and our researched aggregator (**pawaPay**) — *not* its single-leg/no-payout schema or its aggregator pick (one of which — Flutterwave — does not even serve DRC mobile money).

---

## The key framing — are these even the same product?

**No — and after the pivot we have chosen the merchant-acquiring one.** The original report used this same framing to *keep* the consumer product; post-pivot the framing stands but the choice flips.

| | **Ours (chosen, post-pivot): merchant-acquiring** | **Original spec (superseded): consumer pass-through** |
|---|---|---|
| What it is | **Merchant-acquiring** QR / till point-of-sale | Consumer cross-network **pass-through** |
| Who the user is | A **merchant** accepting payment from customers | A payer paying **any** mobile-money number |
| Money legs | **Two** — collect from the customer **+ settle** to the merchant's wallet (we keep the two-leg engine; the customer pays the exact sticker price) | **Two** — collect from payer **+ payout** to payee's wallet |
| Merchant onboarding | **In scope** — register the merchant, settlement details, a till/short-code (merchant KYC flagged) | **None** (the old v1 decision the pivot reversed) |
| How the customer pays | **Scan the merchant's QR (a `tel:` USSD dial-through) or dial the till `*123*1001#`** — no customer app; PIN via the operator's own prompt. Fallback: merchant-initiated "charge by customer number" | Payer keys the **MSISDN**, operator inferred from prefix |
| Apps | **A merchant app** (web dashboard + mobile); **customers need no app** | **One** consumer app |
| Fee direction | **Merchant absorbs the fee (MDR)**; customer pays sticker price | Implicit (consumer-side) |
| Custody | None (instant pass-through settlement; never holds funds) | None (pure pass-through) |

The deeper point the original review made is still correct and now works *for* us: a merchant-acquiring system and a consumer pass-through are different products, so we should build the one we actually want. The pivot makes that the merchant-acquiring one. **What we keep from our own engineering (and the brief lacks): the second, settlement leg and a state in which *collect-succeeded-but-settle-failed* can be represented** — the brief's `transactions` table has one `aggregator_ref`, no payout id, and fires `COMPLETED` on **collection** success, so it still needs the payout/settlement leg and ledger our spec already designed. We adopt the merchant-acquiring *direction* on top of our two-leg, reconcilable engine — not the brief's single-leg schema. (And the QR that accepts customers from any of the three networks *is* exactly the acceptance-side interoperability we now want: one acceptance point, many source networks, settling to the merchant.)

---

## Verdict by dimension

> **Reframed post-pivot.** The original "Better for our MVP" column was scored against the *consumer* charter. Several rows scored "consumer" **only because** of decisions the pivot reversed (no merchant onboarding, one consumer app, MSISDN-not-QR) — those are marked **[reversed]** and now favour the merchant-acquiring direction or are moot. The rows that turned on *engineering quality* (ledger, reconciliation, aggregator pick, two-leg settlement) still favour our own build and are unchanged — we carry that engineering into the merchant-acquiring product.

| Dimension | Direction now chosen | Confidence | One-line reason (post-pivot) |
|---|---|---|---|
| Product model & scope | **Merchant-acquiring** **[reversed]** | High | Merchant onboarding + acceptance is now the MVP; the original "no onboarding / pay-any-number" charter it was scored against is superseded |
| Mobile framework | **Merchant app (+ no customer app)** **[reversed]** | Medium | We now need a *merchant* app (dashboard + mobile); customers pay by USSD with no app — the "one consumer app" basis is gone. RN+Expo still fits the one-engineer constraint |
| Backend & hosting | Mixed (keep ours) | High | Original-spec stack (AWS Cape Town) wins production/latency/residency; brief's Supabase/Railway wins demo speed only |
| Auth & onboarding | Merchant onboarding now in scope **[reversed]** | Medium | We now *do* onboard merchants (settlement account, till/short-code, KYC flagged); customer leg stays PIN-via-operator. Managed auth for the merchant console is now worth it (AWS Cognito, in-region) |
| Queue, webhooks & reliability | **Keep ours** | Medium | Ours adds idempotency + reconciliation + status-poll fallback the brief lacks — still required for the settlement leg |
| Database & data model | **Keep ours** | High | Ours has a double-entry ledger + 10-state machine; the brief is single-leg, no ledger — we keep ours and add merchant entities |
| Aggregator choice | **Keep ours (pawaPay)** | High | pawaPay (3 DRC operators, published pricing) vs the brief's CinetPay/**Flutterwave** (latter doesn't serve DRC) — unchanged by the pivot |
| Payee identification & QR | **Merchant QR / till now in scope** **[reversed]** | High | A merchant QR/till is now the primary customer flow; still no DRC *national* QR standard, so we run our own merchant QR (a `tel:` USSD dial-through), accepting from all three networks |
| Payout / settlement model | **Keep ours (two-leg)** | High | The settle-to-merchant leg is essential to the merchant-acquiring model too; the brief still omits it |
| Demo-readiness & timeline | Mixed (adopt direction, keep rigor) | High | The brief is tuned to win a room in 8 weeks; it excludes the legal + post-sandbox unknowns that decide a real launch |

---

## Issues with the brief's *engineering* (still valid post-pivot)

These critiques were about how the brief is *built*, not about the merchant-acquiring direction — so they survive the pivot. We adopt the direction but **not** these implementation choices:

1. **No settlement leg — the bridge fails silently.** Only `/collect` into a merchant balance; no disbursement call, no payout reference. Even in the merchant-acquiring model the money must *land in the merchant's wallet* (instant pass-through settlement). The settle call isn't optional — it is how the merchant gets paid. We keep our two-leg engine.
2. **The QR is closed-loop in the brief.** Verified negative finding: **there is no DRC *national* QR standard.** The brief's QR is scannable only between its own two apps. Our merchant QR is instead a **`tel:` USSD dial-through** (e.g. `tel:*123*1001%23`) that any phone's dialer can run — so it works from a feature phone with no customer app, across all three networks, rather than depending on a proprietary scanner.
3. **Aggregator recommendation is partly wrong and omits our lead.** **Flutterwave does not support DRC mobile money at all** (its mobile-money list covers Ghana/Kenya/Senegal/etc., not DRC; "Congo" = Congo-Brazzaville/XAF). The brief's "Flutterwave: M-Pesa Yes, Airtel Yes" cell is **false** — the exact DRC-vs-Congo-Brazzaville confusion the brief itself warns about — making its "whichever returns a sandbox first wins" strategy actively risky. **CinetPay** does check out (BCC-listed, Kinshasa). **pawaPay** — our researched lead — isn't mentioned.
4. **"Never drop a webhook" is overstated.** BullMQ protects state *after* receipt, but `webhook_logs` has no idempotency key (double-processing risk) and there is **no reconciliation/status-poll** — a dropped or mis-signed callback leaves a transaction stuck in `PROCESSING` forever, recovery push-only. We keep idempotency + reconciliation.
5. **Legal/regulatory explicitly excluded** while branding the audit table "regulatory evidence" — defers the single biggest unknown (BCC EMI / licensed-local-partner question), so a polished demo can imply a launch path that may not legally exist. (Unchanged by the pivot; merchant onboarding adds merchant KYC on top.)

## Things the original review faulted that are now in-scope FEATURES (post-pivot)

The original report listed these as reasons the brief's product was wrong for us. **After the pivot they are exactly what we are building** — recorded here so the reversal is explicit:

1. **Merchant onboarding** — originally "dead scope" against the *no-onboarding* charter. **Now in scope:** registering a merchant, their settlement account, and a till/short-code is the core of a merchant-acquiring MVP (merchant KYC flagged). The launch wedge is ~40 gas stations + pop-ups, so onboarding is the deliberate first step, not avoided.
2. **A merchant app / dashboard** — originally "two apps roughly double the build." **Now in scope:** we ship a *merchant* app (web dashboard + mobile) for acceptance, history, and reconciliation; **customers need no app at all** (they pay by USSD), so this is one app, not two.
3. **A merchant QR / till** — originally a "closed-loop, cold-start" anti-pattern. **Now the primary customer flow:** scan the merchant's QR (a `tel:` USSD dial-through) or dial the till `*123*1001#`; the merchant-initiated "charge by customer number" push is the fallback.
4. **Merchant-facing reconciliation** — originally "a gap a merchant-acquiring competitor would attack, but one we exclude." **Now a headline feature:** clean dashboards to see and reconcile inbound payments are part of the merchant value proposition.

## Top issues with OUR engineering carried into the merchant product (honest)

1. **Materially more to build** — two legs + auto-refund/recovery, double-entry ledger with reconciliation, a 10-state machine, Hakikisha name-preview, **plus** the new merchant-onboarding + dashboard surface. Larger than a single-leg happy-path, but the merchant tooling is now required, not gold-plating.
2. **Pricing is an open decision and the placeholder is below cost.** The merchant absorbs the fee (MDR). The pawaPay round-trip cost is **3.5–5%** (see [recommendation.md](recommendation.md)), so the **1% MDR placeholder is below cost**; real MDR (~5–7%+) is an open pricing decision that must be resolved before launch. (Cost facts unchanged by the pivot; only who bears them — the merchant — is now settled.)
3. **Single-vendor concentration on pawaPay** — 3 of 4 operators, hard zero on Afrimoney, no integrated fallback (MOKO reserve-only).
4. **RN+Expo trade-off is real** — worse cold-start/jank than Flutter's Dart-AOT on low-end Transsion devices; native (Kotlin/Swift) is the flagship pattern (firmly for Wave + OPay; Kuda's stack is unpublished). A deliberate velocity choice for the merchant app, but a genuine one.
5. **Two foundational items still open** — backend language (Python vs Node) deferred to the hired engineer; the schedule assumes the current team executes.

---

## What to adopt from the brief (concrete, prioritised — post-pivot)

> The big change post-pivot: items the original report banked as "explicit v2 — do not build now" (merchant onboarding, merchant dashboard, merchant QR) are now **MVP scope**, because the merchant-acquiring direction *is* the product. They are pulled up to Tier 1.

**Tier 1 — adopt now (now core MVP, not optional):**
1. **The merchant-acquiring model itself** — merchant onboarding (settlement account + till/short-code, merchant KYC flagged), a merchant QR/till as the customer's pay action, and a merchant dashboard for acceptance + reconciliation. This was the brief's whole point and is now ours; build it on top of our two-leg, reconcilable engine rather than the brief's single-leg schema.
2. **Merchant-facing reconciliation / dashboard** — the one place the brief's model genuinely beats a bare P2P transfer. Now a headline merchant feature (see-and-reconcile inbound payments), not a deferred nicety.
3. **Split the webhook receiver into its own always-on, stateless service** (we currently describe it as an endpoint, not a deployed process) — smaller failure/attack surface, independent scaling/redeploy, pairs with our idempotency + reconciliation.
4. **Add a first-class `mno_status_codes` lookup table** (`operator, raw_code, meaning, is_terminal`) to normalise raw operator/pawaPay result codes into our `failure_reason` and sharpen reconciliation's terminal-state decisions.
5. **Adopt their 5-question aggregator-vetting checklist as a formal go-live gate** for pawaPay during the sandbox pilot, and **get one real price-check quote from CinetPay** (BCC-listed for DRC — *not* Flutterwave) to confirm pawaPay's published rates.

**Tier 2 — adopt for the investor demo (treat the throwaway parts as throwaway):**
6. **Define 2–3 rehearsed "milestone demo moments" against our real system** — a live/sandbox **customer-USSD-pays-a-merchant** flow (scan QR / dial till), the merchant dashboard showing the inbound payment reconciled, and a *settle-fails-then-recovers* moment showing signed `operator_event` logs — so there's a fundable artefact before code freeze.
7. **Consider Supabase/PaaS for the demo loop** to move fast — but build the real ledger on **in-region AWS (ECS Fargate)** and avoid Supabase Auth/Realtime/RLS lock-in. Carry over **Sentry + Logtail** monitoring. (Note: Supabase "zero-config phone OTP" works only with its native SMS providers — Twilio/MessageBird/Vonage — **not Africa's Talking**, our chosen provider, so our SMS plumbing stays custom. For the **merchant console** — now in scope — **AWS Cognito** (already in-region) is the cleaner managed-auth choice.)

**Tier 3 — the customer-side QR/USSD detail (now MVP-critical, design carefully):**
8. **Customer pay-action = a `tel:` USSD dial-through QR + a dialable till short-code**, not a server-signed `merchant_id` scanned by a proprietary app. This gives scan-to-pay (and dial-to-pay from a feature phone) **without** depending on a custom scanner, and resolves to the right merchant till on our side. This is the primary customer flow, with merchant-initiated "charge by customer number" as the fallback.

**Explicitly do NOT adopt** (engineering, not direction): the brief's **single-leg / no-settlement** schema (we keep the settle-to-merchant leg + ledger); the CinetPay/**Flutterwave** "first sandbox wins" aggregator selection (Flutterwave doesn't serve DRC mobile money); and the *proprietary-scanner* QR premise (we use a `tel:` USSD dial-through that any dialer runs). The merchant-onboarding and merchant-dashboard ideas that this line *previously* told us to avoid are now adopted.

---

## Factual corrections the review produced (applied where they touch our own files)

The adversarial pass also caught genuine errors in **our own spec**, now fixed:

| # | Correction | Where | Status |
|---|---|---|---|
| 1 | pawaPay signs callbacks with **RFC-9421 HTTP Message Signatures (public-key, ECDSA-P256)** — **not HMAC** (verified against pawaPay's own docs). | `api-contracts.md`, `data-model.md` | **Fixed in spec** |
| 2 | **AWS App Runner is not available in af-south-1** (Cape Town; open roadmap request since 2022) — use **ECS Fargate**. | `tech-stack.md` | **Fixed in spec** |
| 3 | Latency to Kinshasa is **~80–100 ms (inference, not measured)**, and in-country last-mile (>150 ms) likely dominates — not the "~50–80 ms" stated. | `tech-stack.md` | **Fixed in spec** |
| 4 | Our state machine has **10 states**, not "8" (verified vs the code). The data model is now the **merchant model** — 3 built (`merchant` / `transaction` / `ledger_entry`) + 4 planned entities — superseding the old consumer "9". | `README.md` | **Fixed in spec** |
| 5 | **Flutterwave does not serve DRC mobile money** — the brief's coverage claim is false. | (the brief — not our file) | Recorded here |
| 6 | Supabase "zero-config phone OTP" **excludes Africa's Talking** (our chosen provider). | (analysis note) | Recorded here |

---

## Confidence & open questions
- **Confident about:** the two are genuinely different products (High); **the team has chosen the merchant-acquiring direction (decision, 2026-06-11)** — so the original 7–0 tally *for* the consumer product is superseded, and the rows it scored "ours" on the *no-onboarding / one-consumer-app / MSISDN-not-QR* charter no longer bind (High); the *engineering* critiques and corrections still hold regardless of audience (High): Flutterwave does not serve DRC mobile money (High — Flutterwave's own coverage list); pawaPay uses RFC-9421 not HMAC (High — pawaPay docs); App Runner absent from af-south-1 (High — AWS roadmap); the settle-to-merchant leg + ledger are required (High).
- **Uncertain / unverified:** the **MDR / pricing decision** — the merchant absorbs the fee, but the 1% placeholder is below the 3.5–5% pawaPay round-trip cost, so the real MDR is an open decision (see [recommendation.md](recommendation.md)); whether a **`tel:` USSD dial-through QR / dialable till** behaves consistently across all three operators' menus (untested — see [`02-findings/cross-cutting/offline-ussd-feasibility.md`](../02-findings/cross-cutting/offline-ussd-feasibility.md)); merchant-KYC requirements for onboarding; CinetPay's exact per-operator collect-vs-settle matrix for DRC (their API docs returned 403/503 — Medium; regulator-listing + marketing confirm presence); Cellulant/Tingg DRC coverage (unconfirmed); pawaPay settlement-cadence / transaction-feed timing for the ledger reconcile (Low — "not found"); whether RN cold-start on low-end Transsion is a real problem for the merchant app (untested).
- **Still missing (and how to get it):** the resolved MDR (a pricing decision, not research); per-operator USSD/short-code lead times and shortcode provisioning for the till flow; a price-check quote from CinetPay (contact during sandbox pilot); confirmation that pawaPay's name-lookup supports the Hakikisha preview for all three operators; counsel's view on the BCC EMI / partner-PSP question, now including merchant onboarding/KYC (the licensing gate the brief omits entirely).

## Sources
By ID, from [04-sources/sources.md](../04-sources/sources.md): pp-001..pp-008 (pawaPay), on-001..on-007 (Onafriq/Beyonic lineage), xo-005 (PayAtlas DRC), and new entries ac-001..ac-004 (pawaPay RFC-9421 signature docs; Flutterwave DRC coverage; AWS App Runner af-south-1 roadmap; CinetPay BCC-listed). Plus internal: [05-product-spec/](../05-product-spec/) (functionality.md, data-model.md, tech-stack.md, api-contracts.md), [recommendation.md](recommendation.md), [payee-identification.md](../02-findings/cross-cutting/payee-identification.md), [own-aggregator.md](../02-findings/cross-cutting/own-aggregator.md).
