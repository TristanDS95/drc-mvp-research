# Offline / no-internet version feasibility (client-side USSD automation)

- **Topic / entity:** an offline, no-backend version of the app that works without internet — "do what pawaPay does on the customer's phone … no recording/ledger/data transfer"
- **Question it addresses:** Is it feasible to build a basic-functionality version that moves money without an internet connection and without our own backend/ledger?
- **Date researched:** 2026-06-09
- **Overall confidence:** Medium-High on the technical verdict; Medium on DRC-operator specifics; Low-Medium on effort estimate.
- **Claim type:** mix — verified facts (platform capabilities, USSD-over-signaling), inference (architecture conclusions, licensing), company self-reported (Hover precedent).

## Bottom line
**Feasible, but not as described.** An offline, no-backend version is buildable **only as an Android-only "USSD automation" app that drives the mobile money operators' own menus** — it cannot be a phone-side version of pawaPay. The single thing pawaPay does (bridge networks by momentarily *holding* funds) is inherently a networked, money-custody, server-side function and **cannot run on a handset, online or off**. The reason the idea is still partly viable: **the DRC operators already perform cross-network sends themselves over USSD** (e.g., M-Pesa "Mikili" via `*555#`), with no internet, so an offline app doesn't need to bridge — it can ride the operators' rails with a nicer face. The strategic catch is decisive: because the operator (not us) moves the money, **we cannot capture a per-transaction fee, keep a ledger, or issue our own receipts** — exactly the "no recording/ledger/data transfer" the question describes, which also means no payments-business model on this path alone.

---

## 1. The conceptual core — why a phone can't "be pawaPay"

**A phone never moves money. The operator's servers do** — triggered one of two ways:

| Trigger | Internet needed? | Who actually moves the money |
|---|---|---|
| **API call** (our current MVP via pawaPay) | Yes (data) | Operator servers, via the aggregator |
| **USSD** (dial code → menu → PIN) | **No** — runs on the GSM signaling channel | Operator servers, directly |

- **Verified fact:** USSD operates over the GSM signaling channel and requires no data connectivity; M-Pesa, Airtel Money, and others were built on it and still run most transactions through it ([arkesel — USSD financial services in Africa](https://arkesel.com/ussd-financial-services-africa-mobile-money/)). **High confidence.**
- **Inference (High confidence):** pawaPay's value is to **receive** funds on Network A into its settlement account and **disburse** on Network B. That is a money-holding networked intermediary. A handset cannot be that intermediary — it cannot turn Vodacom money into Airtel money. Only a server with accounts at both operators (pawaPay / our own rails) **or the operators' own interoperability** can bridge. This is consistent with the build-vs-rent analysis in [own-aggregator.md](own-aggregator.md).

**The reframe that rescues the idea:** the DRC operators already expose cross-network send *in their own USSD menus*. M-Pesa "Mikili": dial `*555#` → "Send money" → **"To another operator"** to reach Orange Money or Airtel ([iambeezy — M-Pesa DRC guide, 2026](https://blog.iambeezy.app/fr/envoyer-argent-m-pesa-kinshasa-2026/); corroborates the Mikili page already logged as [xo-013]). Off-net interop fees ("slightly higher") are borne by the user and set by the operator. So an offline app's job is **not to bridge** — it's to **drive the operator's existing USSD flow** with a clean interface. **Confidence: Medium-High** that the path exists; **Medium** on how completely it covers all three operator pairs (see Open questions).

---

## 2. What IS feasible offline — the USSD-automation approach

An Android app that automates the operator's USSD menus on the user's own phone:

- **No internet** — USSD uses the signaling channel. ✓ (matches the ask)
- **No backend, no ledger, no server-side data transfer** — we are not in the money path. ✓ (matches the ask)
- **Cross-network capable** — by riding the operators' own interop (Mikili etc.), not by bridging ourselves.
- **Precedent exists (company self-reported, Medium-High):** [**Hover**](https://www.usehover.com/) is a commercial Android SDK purpose-built to automate existing USSD sessions in the background; its customers "built apps that let people manage multiple mobile money accounts from a single interface, pay merchants via QR codes" — almost exactly this use case ([Hover — Automate any USSD Service on Android](https://medium.com/use-hover/automate-any-ussd-service-on-android-45aa9dd9dfa); [docs.usehover.com](http://docs.usehover.com/)).

How it works in practice: app collects recipient + amount + (inferred) operator → launches the operator's USSD send-money sequence → automates menu navigation → **hands the PIN step to the operator's own prompt** (the app must never capture the PIN) → reads the confirmation text for a local-only record.

---

## 3. Hard constraints (why it isn't a slam dunk)

| Constraint | Detail | Confidence / source |
|---|---|---|
| **Android only** | iOS blocks programmatic USSD entirely — the Phone app won't dial codes containing `*`/`#`, and apps can't read USSD responses. | High — [Apple Developer Forums](https://developer.apple.com/forums/thread/14766) |
| **Android is itself limited** | Native `sendUssdRequest` (API 26+) does not handle multi-step interactive sessions well; robust automation needs an accessibility-service approach (what Hover does). | Med-High — [Hover/Android USSD](https://medium.com/use-hover/automate-any-ussd-service-on-android-45aa9dd9dfa); [multi-step USSD limitation note](https://hemant9807.blogspot.com/2018/05/interactive-ussd-sessionmulti-step-does.html) |
| **Fragile** | Operator menu changes silently break the automation; session timeouts (<~1 min); per-operator menu trees must be scripted and re-tested. Ongoing maintenance burden. | Med-High — same sources |
| **Play Store policy risk** | Accessibility-service use is permitted only for "deterministic, rule-based" scripts and is under tightening scrutiny (Oct 2025 update prohibits autonomous action). USSD automation likely qualifies but is a real watch item. | Med — [Google Play AccessibilityService policy](https://support.google.com/googleplay/android-developer/answer/10964491) |
| **PIN / security** | The app must never capture the mobile money PIN — hand that step to the operator's own prompt. Non-negotiable. | High (inference / standard practice) |
| **No fee capture** | Operator moves the money; we are not in the path → no per-transaction fee, no server ledger, no own receipts. | High (inference) |

**Mitigant on the Android-only limit:** roughly ~95% of DRC smartphones are Android (Transsion brands — Tecno/Infinix/Itel — dominate; iOS share is small), so Android-only hurts far less here than in a Western market ([network-and-devices.md](../app-development/network-and-devices.md)). **Medium confidence.**

---

## 4. The two architectures, side by side

| | **A. Online app + aggregator (current MVP)** | **B. Offline USSD-automation (this idea)** |
|---|---|---|
| Internet required | Yes (small data for orchestration; PIN approval itself is over cellular) | **No** |
| Our backend / ledger | Yes | **No** |
| Who moves the money | pawaPay via operator APIs | The operator, via its own USSD |
| Cross-network bridging | pawaPay does it | The operator's own interop does it |
| In the money path? | Yes → can charge a fee, keep history, issue receipts | **No** → cannot charge a fee or keep a server ledger |
| Platforms | iPhone + Android | **Android only** |
| Reliability | Controlled (API + reconciliation) | Fragile (USSD menu scraping) |
| Licensing exposure | Heavier (BCC EMI/PSP question open) | **Likely lighter** — flag for counsel |

---

## 5. Strategic trade-off (the real decision)

- **Upside of B:** much smaller build (no backend, ledger, reconciliation, or pawaPay integration); genuinely offline; and **probably lighter on licensing** — if we never touch the money, the BCC EMI/PSP question that shadows the main MVP ([kyc-and-onboarding.md](kyc-and-onboarding.md), [own-aggregator.md](own-aggregator.md)) may largely fall away. **Inference, Medium confidence — confirm with counsel; facilitating payments may still draw scrutiny.**
- **Downside of B:** we become a **convenience layer, not a payments business.** No transaction-fee revenue, no transaction data, no relationship with the money flow. Monetization must come from elsewhere (subscription, airtime margin, lead-gen) — weak. This is the decisive limitation.

---

## 6. Effort & cost (rough — Low-Medium confidence)

Android-only offline app using Hover: **~6–10 weeks, one Android engineer**, plus **Hover SDK licensing** (commercial; pricing unconfirmed — flag). At the $80/hr, 10–20 hrs/week model that's roughly **$6,000–$13,000** in labor. The fragile, time-consuming part is scripting and testing each operator's on-net **and** off-net USSD flows against real DRC SIMs (≈6 flows across Vodacom/Airtel/Orange).

⚠️ **Do not conflate with the "USSD v2" item in [functionality.md](../../05-product-spec/functionality.md).** That is the *opposite* architecture: a **server-side shortcode** (e.g., `*123#`) we would rent from each operator (3–9 month contracts; our backend very much online) to serve **feature-phone** users — now researched in [ussd-gateway-providers.md](ussd-gateway-providers.md), and the channel our app's `ussd/` module implements. This offline idea needs **no shortcode and no backend** — it automates the operator's *existing* USSD on the user's own smartphone.

---

## 7. Options / recommendation

1. **Offline fallback inside the main app** *(recommended if pursued at all):* online-first via pawaPay (capture fee + ledger); when there's no data, fall back to driving the operator's USSD so the user can still pay (no fee that round). Best resilience; inherits both architectures' fragilities and doubles some work.
2. **Standalone offline-only app:** the version as described. Viable as a lightweight Android utility / market wedge, but **no revenue path on its own**.
3. **Skip it:** keep the data-light online MVP. The PIN approval already works over cellular; only the *trigger* needs a little data, so the practical "works on bad networks" need is largely met without going fully offline.

**Leaning:** treat B as a **possible v2 wedge or resilience feature, not a v1 path** — its lack of a money-path (no fee, no data) makes it a feature, not the business. Validate the two dependencies below before committing.

---

## 8. Open questions / validation needed
- **Does Hover reliably support the three DRC operators' USSD flows** (Vodacom/Airtel/Orange), including off-net send? Requires testing with real DRC SIMs. *(Highest-priority unknown.)*
- **Are the operators' off-net USSD menus stable enough to automate** without constant breakage?
- **Hover SDK commercial terms / pricing** for DRC volume — not found publicly.
- **Counsel view on licensing** for a pure USSD-automation tool that never holds funds — is it genuinely lighter than the pass-through MVP, or does "facilitating payments" still trigger BCC/PSP requirements?
- **Off-net interop fee levels** charged by each operator (borne by the user) — affects whether the UX is attractive vs. just using the operator app.
- **Google Play review outcome** for an accessibility-service USSD-automation app in this category (policy is tightening).

## Sources
- xo-028 — arkesel, "USSD Banking in Africa: Mobile Money Without Apps" (USSD over signaling, no data)
- xo-029 — Hover, "Automate any USSD Service on Android" (Medium) + Android multi-step limitation
- xo-030 — Hover product / docs (usehover.com, docs.usehover.com) — company self-reported precedent
- xo-031 — Apple Developer Forums — programmatic USSD restrictions on iOS
- xo-032 — Google Play Console Help — AccessibilityService API policy
- xo-033 — iambeezy — M-Pesa DRC / Mikili `*555#` off-net guide (2026)
- Existing: xo-013 (M-Pesa Mikili, vodacom.cd), am-004 (Airtel DRC interop page), om-002 (Orange DRC tariff, off-net 1%), ad-024/ad-027 (DRC Android/Transsion dominance), own-aggregator.md, kyc-and-onboarding.md, functionality.md

[[ussd-gateway-providers]] · [[own-aggregator]] · [[payee-identification]] · [[kyc-and-onboarding]] · [[fees-and-costs]]
