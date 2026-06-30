# USSD gateway / shortcode integration (server-side channel)

- **Topic / entity:** the **server-side USSD channel** — renting a shortcode + USSD
  gateway so customers dial **our** menu (e.g. `*XYZ#`) on any phone and pay a merchant.
  This is the channel our app's `ussd/` module implements.
- **Question it addresses:** What providers exist (serving DRC)? How do they compare?
  What does it cost? Can we implement it ourselves without an aggregator? How does it fit
  our architecture?
- **Date researched:** 2026-06-11
- **Overall confidence:** **High** on the integration model + provider landscape;
  **Medium** on DRC-specific pricing (best figures are from a reseller, not the source);
  **Medium** on the DRC self-build regulatory specifics.
- **Claim type:** mix — verified facts (provider coverage, callback model), company
  self-reported (pricing, pawaPay USSD), inference (recommendation, self-build verdict).

> ⚠️ **Not the same as [offline-ussd-feasibility.md](offline-ussd-feasibility.md).** That
> file covers the *opposite* architecture — a smartphone app that automates the
> *operator's own* USSD menus client-side (Hover-style, no shortcode, no backend, no fee
> capture). **This** file covers the **server-side shortcode** model where the customer
> dials *our* code, our backend serves the menu, and we stay in the money path (fee +
> ledger). This is the MVP-critical channel for the merchant pivot.

---

## Bottom line
**Rent the USSD *bearer* from an aggregator; we already own the USSD *application*.**
Our `ussd/` channel already implements the de-facto standard (the Africa's Talking model:
`POST` with `sessionId/phoneNumber/serviceCode/text` → reply `CON`/`END`). Going live in
the DRC means signing with a USSD aggregator that covers DRC, provisioning a shortcode,
and writing a **thin adapter** at our `/ussd` boundary — **no domain or orchestrator
change**. **Africa's Talking** is the front-runner: it explicitly offers **dedicated USSD
codes in the DRC** (its own help centre lists DRC) and uses the exact callback model our
code already speaks. **Infobip** (directly, and via **Telerivet**) is the main
alternative. **Self-hosting our own USSD gateway is not worth it for the MVP** — it
requires a per-operator VAS/WASP agreement + an ARPTC-allocated shortcode + a gateway link
to each MNO, i.e. the **same per-operator-contract barrier** that makes building our own
*payment* aggregator impractical ([own-aggregator.md](own-aggregator.md)). Note the USSD
provider is only the **menu transport** — the actual money still moves through **pawaPay**
(its USSD *payments* product exists but is **Nigeria-only** today, not DRC), so USSD-channel
cost is **additive** to pawaPay's payment fees.

---

## 1. What we're researching (and why our code already fits)
A server-side USSD payment has two phone interactions, both over the GSM signalling
channel (no internet):
1. **Our menu** — customer dials our shortcode → the aggregator relays each step to our
   `/ussd` endpoint → we return the next prompt (till? amount? confirm?).
2. **The MNO PIN prompt** — on "confirm", *we* call pawaPay to initiate the collection;
   pawaPay/the MNO then pushes the customer a PIN prompt on their own handset to authorise.
   We never see the PIN. (This matches our handler's terminal message, "Approve on your
   phone.")

So the aggregator transports the *menu*; pawaPay moves the *money*. They are separate
vendors with separate costs.

---

## 2. Providers that exist (and DRC coverage)

| Provider | DRC USSD? | Integration model | Pricing transparency | Notes |
|---|---|---|---|---|
| **Africa's Talking** | **Yes** — dedicated USSD codes in DRC (their help centre lists "…Zambia DRC, Ghana…") | HTTP callback: `sessionId/phoneNumber/serviceCode/text` → `CON`/`END` | Model public; **DRC rate card on request** | The de-facto African USSD aggregator; model **already matches our `/ussd`** |
| **Infobip** | **Yes** — provides USSD access codes in DRC (per Telerivet's provider list) | HTTP / 2-way, enterprise | **Opaque** — custom quote via account manager | Global CPaaS; heavier enterprise sales motion |
| **Telerivet** | **Yes — but indirectly** | A platform that *connects third-party codes* (Infobip / Africa's Talking); visual Rules Engine or JS "Cloud Script API" | Pro plan+; **each session = 1 message** toward plan | Convenience/low-code layer over the above, not a direct telco link |
| **Beem Africa** | **Unconfirmed for DRC** | HTTP callback "USSD menu API" across Africa | n/a | Markets pan-African USSD; DRC coverage not verified |
| **pawaPay** (our payment rail) | **No (USSD)** — USSD **collections live in Nigeria only** | Its own USSD collections product | n/a | Strategic watch: if pawaPay extends USSD to DRC, we'd get payments **and** channel from one vendor |
| **Direct MNO** (Vodacom/Airtel/Orange/Africell) | n/a — *you become the integrator* | Per-operator USSD-gateway link + ARPTC shortcode + VAS/WASP | n/a | The self-build path — see §4 |
| **USSD-gateway software** (hSenid, Omobio, Panacea Mobile) | n/a | On-prem/operator-side gateway products | n/a | Doesn't remove the need for operator USSD connectivity; for those who *have* it |

**Verified fact (High):** Africa's Talking lists DRC among the countries where **dedicated
USSD codes** are available [xo-036]. **Verified fact (Medium-High):** Telerivet states
Infobip "provides USSD access codes in … Democratic Republic of the Congo …" [xo-039].
**Self-reported (High):** pawaPay's own Q3 round-up says USSD collections were added "in
Nigeria" and lists no other country [xo-037].

---

## 3. Cost (DRC-specific, plus the general model)

**DRC figures — secondary source (helloduty, a CPaaS reseller/aggregator-comparison site;
treat as indicative, confirm with the provider). Confidence: Medium.** [xo-035]

| Item | DRC figure (CDF) | ≈ USD¹ | Note |
|---|---|---|---|
| Dedicated USSD code **setup** (one-time) | 2,846,000 | ~**$1,000** | |
| Monthly code **rental** | 570,000 **per telco / month** | ~**$200**/telco → ~**$600/mo** for 3 operators | per-operator |
| **Per dial session** | 97 | ~**$0.034** | charged per USSD session; **additive to pawaPay payment fees** |

¹ at ≈2,850 CDF/USD (2026 approx.). The CDF amounts back-convert to round USD ($1,000 /
$200 / $0.034), suggesting the prices are USD-pegged and CDF-displayed.

**General model — primary (Africa's Talking help centre). Confidence: High for the
*structure*.** [xo-036] Dedicated USSD pricing is **postpaid**: a **per-network setup
fee + monthly maintenance + per-session charge**, billed per operator. Published Kenya
proxy (regional, not DRC): Safaricom ~KES 70,000/mo maintenance (setup KES 17.4k–145k),
Airtel ~KES 46,400/mo, etc. DRC's own rate card is **"contact us"** (not public) — so the
helloduty figures above are the only DRC-specific numbers found, at Medium confidence.

**Cost insight:** the USSD **channel** cost (setup + per-telco monthly + per-session) is
**separate from and on top of** pawaPay's **payment** fees (≈3.5–5% round-trip envelope —
[fees-and-costs.md](fees-and-costs.md)). The per-session fee (~3.4¢) is small per
transaction but recurs on **every** session including abandoned ones.

---

## 4. Can we implement it ourselves (no aggregator)?
**Two layers — we already own one, and renting the other is right for the MVP:**

- **The USSD *application* (menu logic): yes, ours — already built** (`ussd/session.py`).
- **The USSD *bearer* (getting the dial from the handset to our app): not practical to
  self-host for the MVP.** To connect directly to the operators you must, **per operator**:
  - sign a **VAS / WASP agreement** with the MNO and connect to its USSD gateway;
  - obtain a **shortcode allocated by the regulator (ARPTC)** — shortcodes are
    regulator-gated in the DRC, and premium/VAS services need a licence; [xo-040][xo-041]
  - integrate + test against each MNO's gateway (regional lead time **6–12 weeks+** per
    operator for direct integration). [xo-040]

This is the **same per-operator-contract, regulator-gated barrier** that made building our
own *payment* aggregator impractical for the MVP ([own-aggregator.md](own-aggregator.md)) —
3–4 bilateral telco relationships, opaque terms, slow. **Inference (High):** rent the
bearer now; reconsider a direct/self-hosted gateway only at scale (volume that justifies
cutting the per-session aggregator margin), exactly as we concluded for payment rails.

---

## 5. Architecture fit — our `ussd/` channel is already the right shape
**Verified fact (High) [xo-038]:** the Africa's Talking model — which Infobip and most
African USSD aggregators mirror — POSTs four fields per step: `sessionId`, `phoneNumber`,
`serviceCode`, `text`; the app replies with a body prefixed **`CON`** (keep the session
open) or **`END`** (terminate). Each HTTP request is **stateless**; `text` carries the
user's accumulated inputs.

Our implementation (`http/ussd_routes.py` → `ussd/session.py`) already exposes
`POST /ussd` taking `{session_id, msisdn, text}` and returns `CON`/`END`. **The fit is
direct.** The only work to go live is a **thin adapter at the `/ussd` boundary** for the
chosen aggregator's wire specifics — **no domain/orchestrator change**:

| Aggregator reality | Our scaffold today | Adapter work |
|---|---|---|
| `text` accumulates **all** inputs, `*`-joined (e.g. `1001*10*1`) | expects the **latest** input + tracks state server-side | split `text` on `*`, take the last segment (or replay full text) — **the one real delta**, already flagged in code |
| Posts **form-encoded** fields (`sessionId`, `phoneNumber`, …) | accepts JSON `{session_id, msisdn, text}` | map field names + content-type |
| Requires a **public HTTPS** endpoint, **200** reply, **fast** (~≤10 s) response | already synchronous + fast | deploy behind HTTPS |

Money path unchanged: on confirm we still call `application.start_merchant_payment` →
pawaPay deposit → MNO PIN push. The aggregator is pure menu transport.

---

## 6. Options / recommendation
1. **Rent Africa's Talking for the USSD bearer (recommended).** DRC-confirmed dedicated
   codes; the callback model our code already speaks; semi-transparent pricing. Lowest
   integration risk. Validate per-operator DRC coverage + a real rate card on contact.
2. **Infobip (directly or via Telerivet) as the alternative** — DRC-capable, but
   enterprise sales + opaque pricing; Telerivet adds a low-code layer (and a markup).
3. **Keep pawaPay as the payment rail**, and **track pawaPay extending USSD to DRC** — if
   it does, a single vendor covers both payments and the USSD channel (simplest of all).
4. **Do not self-host a USSD gateway for the MVP** — per-operator VAS/WASP + ARPTC
   shortcodes = the own-aggregator barrier. Revisit only at scale.

**Leaning:** MVP = **Africa's Talking (or Infobip) bearer + our existing `ussd/` app +
pawaPay for money**, with a thin `/ussd` adapter. This keeps the channel rented and
swappable, matching the rest of our rented-rails MVP posture.

---

## 7. Open questions / validation needed
- **DRC rate card from Africa's Talking / Infobip** (the helloduty figures are secondary)
  — requires contacting them (**team action**; we don't sign up).
- **Per-operator DRC coverage:** does the aggregator's DRC USSD reach **all** of
  Vodacom/Airtel/Orange (and Africell), or a subset? Coverage gaps would fragment the UX.
- **Shortcode provisioning lead time in the DRC** (weeks/months) — not found; affects
  launch timing. (offline-ussd-feasibility.md cited a "3–9 month" shortcode lead time for
  the *direct* route — verify for the *aggregator* route, which should be faster.)
- **Dedicated vs shared code** for a payment flow — dedicated (`*XYZ#`, own brand) vs
  shared (cheaper, menu-prefix UX). Which the operators/ARPTC permit for payments.
- **DRC VAS/WASP licensing** to host a payment USSD menu — counsel + a primary ARPTC source
  (the regulatory points here are Medium confidence, search-aggregated).
- **Can the USSD provider also settle the payment** in DRC (avoiding the pawaPay double-hop)?
  Unlikely for cross-network mobile money, but worth a direct question.
- **pawaPay DRC USSD roadmap** — would collapse two vendors into one.

## Sources
- xo-035 — helloduty, "DRC's USSD, SMS, WhatsApp & Call Services: Pricing & Setup Guide" (secondary; CPaaS reseller — DRC USSD figures)
- xo-036 — Africa's Talking Help Centre, "How much is a dedicated USSD code" (primary; lists DRC, pricing model)
- xo-037 — pawaPay, "Q3 round-up" blog (self-reported; USSD collections Nigeria only)
- xo-038 — BFA Global, "Serverless USSD with Africa's Talking — Part 1" (secondary; callback model `sessionId/phoneNumber/serviceCode/text` → CON/END)
- xo-039 — Telerivet, USSD product page (self-reported/primary; platform reselling Infobip/AT; Infobip lists DRC)
- xo-040 — Arkesel, "How to Create a USSD Code: Developer Guide for Africa" (secondary; direct-MNO vs aggregator, VAS/WASP, lead times) — via search index
- xo-041 — ARPTC, Autorité de Régulation des Postes et Télécommunications du Congo (arptc.gouv.cd) (primary; DRC shortcode/numbering authority — role) — via search index

[[offline-ussd-feasibility]] · [[own-aggregator]] · [[fees-and-costs]] · [[pawapay-api-deep-dive]]
