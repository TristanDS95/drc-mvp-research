# On-net direct-operator collection APIs (cross-cutting)

- **Topic:** bypassing pawaPay for **same-network (on-net)** merchant payments by using the operators' own C2B APIs directly.
- **Question it addresses:** can a customer pay a same-network merchant **inside our app**, with the operator moving the money in **one cheap leg** and a **trustworthy automatic confirmation** back to us — and is pawaPay a cheaper shortcut?
- **Date researched:** 2026-06-19
- **Overall confidence:** Medium. The API *mechanisms* are documented (primary/High); DRC-specific activation, the aggregator/multi-merchant model, pricing, and licensing are all **partner-gated** and need direct contact.

## Bottom line
- **pawaPay cannot do it (High confidence).** Every pawaPay **deposit lands in *our* wallet** — the Initiate Deposit and Payment-Page APIs have no recipient/merchant field — so reaching the merchant always needs a **second payout leg**. No single-leg C2B-to-merchant, no on-net discount, no documented aggregator/sub-merchant model. The two-leg cost is the floor via pawaPay: **Airtel 5% · Vodacom 4.5% · Orange 4%** (Orange cheapest — no MMO payout fee). So the on-net saving **requires going direct to the operators**; pawaPay is not a shortcut. (An undocumented Enterprise/aggregator tier can't be ruled out — one sales question would close it.)
- **M-Pesa and Airtel support exactly the model we want:** a third-party-initiated **USSD/STK push** (customer pays **in-app**, enters PIN on their handset), funds routed to a merchant code, plus an **async callback + status-query** for trustworthy auto-confirmation. Moderately complex, partner-gated.
- **Orange does NOT** offer a clean in-app push — `om-webpay` is a **web-redirect + USSD-OTP** flow (customer is sent to Orange's hosted page **and** must dial USSD for a one-time code). It fails the "pay inside our app, one tap" bar.
- **Each operator is a separate integration** (different API, auth, onboarding) — no cross-operator consistency, which is exactly why pawaPay exists. Expect **2–3 distinct integrations + per-operator partner onboarding**.

## Per-operator comparison

| Operator | API / product | DRC available? | In-app push? | Trustworthy auto-confirm? | Onboarding | Pricing | Fit |
|---|---|---|---|---|---|---|---|
| **Vodacom M-Pesa** | C2B Single Payment (OpenAPI portal; legacy SOAP IPG `ipg.m-pesa.vodacom.cd`) | **Yes** (DRC a listed market) | ✅ USSD/STK push | ✅ callback to Response URL + Transaction Status query | Business KYC + M-Pesa Services Agreement + short code; sandbox yes; SOAP path needs **IP whitelisting** | partner-gated (~0.5–1%? *unverified secondary*) | **Good** — aggregator/multi-merchant model unconfirmed |
| **Airtel Money** | Collection API (Airtel Africa Open API) | **Likely** — DRC in the footprint + pawaPay already runs `AIRTEL_COD`; direct API activation portal-login-gated (secondary-only, needs confirm) | ✅ USSD push | ✅ callback + status enquiry (callback+poll double-check) | OAuth2; **self-serve sandbox** (`openapiuat.airtel.africa`) — test today; production = KYC, weeks–months | partner-gated | **Good** — easiest to start testing |
| **Orange Money** | `om-webpay` (Web Payment) | Listed on overview (**FAQ omits DRC** — conflict) | ❌ **web redirect + USSD-OTP** (customer leaves to a hosted page + dials USSD for a one-time code) | ⚠️ `notif_url` webhook + status poll (webhook reliability flagged) | in-store KYA enrollment, RCCM; **sandbox exists** (`webpayment-sb.orange-money.com`) | partner-gated | **Weak** — not a clean in-app push |

## Testing / sandbox availability (how do we test the pipeline?)
- **Our own pipeline first** — a **simulated `DirectCollectRail`** (mirroring the existing pawaPay `SimulatedPaymentRail`) makes the whole on-net flow (router → on-net orchestrator → single ledger entry → confirm) testable **offline**, no operator. Most testing lives here.
- **Airtel — self-serve sandbox:** sign up at `developers.airtel.africa` → `openapiuat.airtel.africa`; testable **today**, no partner agreement (production = KYC). Easiest to start.
- **M-Pesa — sandbox via the OpenAPI portal** (`openapiportal.m-pesa.com`) with test MSISDNs; a production business account / KYC may gate full access — confirm.
- **Orange — sandbox exists** (`webpayment-sb.orange-money.com`) via `developer.orange.com`; but on-net Orange is a redirect/OTP flow, so lower priority.
- **pawaPay — already on its sandbox** (current setup).

## Key open questions (all need direct partner/sales contact)
1. **Aggregator / multi-merchant model — the biggest unknown.** Can ONE platform credential collect to MANY merchants' tills (merchant code per transaction), or does each merchant need its own credential + agreement? **Not publicly confirmed for any of the three, nor pawaPay.** This shapes whether on-net is even operable at our scale (and may be a licensing matter).
2. **Pricing.** Every operator's API merchant rate is partner-gated — we need **quotes** to confirm the saving vs pawaPay's 4–5%. (Secondary hints put M-Pesa merchant fees ~0.5–1%, unverified.)
3. **Licensing / regulatory.** Does intermediating operator payments on behalf of merchants require an **ARPTC / Banque Centrale du Congo** licence (EMI / PSP)? Flagged in the M-Pesa research, unaddressed in public docs. Material — legal track.
4. **pawaPay Enterprise.** One cheap sales question: does pawaPay offer a deposit-direct-to-merchant or sub-merchant/aggregator tier? Would close "is direct integration even necessary?"
5. **Orange in-app gap.** Is there any Orange *push* API for DRC (vs the redirect/OTP `om-webpay`)? If not, on-net Orange likely **stays on pawaPay** or accepts the redirect UX.

## What this means for the build
- On-net direct integration is **real, multi-operator work**, gated by partner onboarding + the questions above — **not a quick config change**, and **not** shortcuttable through pawaPay.
- Smartest first move = the two cheap **sales questions** (pawaPay aggregator/direct-to-merchant; each operator's aggregator model + pricing + DRC activation) — they could materially change the plan.
- **Sequencing:** the operators differ in readiness — M-Pesa & Airtel fit the in-app model, Orange does not. So build the abstraction + **one operator first** (M-Pesa or Airtel), behind a simulator, and let the router **fall back to pawaPay** per-operator until each on-net rail is ready (Orange may stay on pawaPay indefinitely).

## Confidence & sources
- **High:** pawaPay's two-leg model (deposit lands in our wallet, no recipient field) — primary docs, unambiguous.
- **High:** each operator's C2B *mechanism* exists; Orange is redirect/OTP not push.
- **Medium:** DRC-specific production activation for each (Airtel & M-Pesa strongly evidenced; Orange listing conflicts).
- **Low / not found:** all API pricing (partner-gated), the aggregator/multi-merchant model (all vendors), and licensing — every one needs direct contact; do **not** model costs off the unverified secondary hints.

Key primary sources: M-Pesa DRC onboarding (`business.m-pesa.com/vodacom-drc/onboarding-your-business-in-drc/`), M-Pesa "customer pay on 3rd-party app" + developers portal, Airtel Africa Open API (`developers.airtel.africa`) + Collection API per community SDKs, Orange `developer.orange.com/apis/om-webpay` (+ /faq), pawaPay v2 deposits/payouts/payment-page (`docs.pawapay.io/v2`) + `pawapay.io/fees`. (Grounded in four dedicated research runs, 2026-06-19/20 — one per operator plus pawaPay; full per-claim source tables in those runs. The pawaPay verdict is corroborated by our earlier `pawapay-api-deep-dive` and `fees-and-costs` findings.)

[[mpesa]] · [[airtel-money]] · [[orange-money]] · [[pawapay]] · [[fees-and-costs]] · [[own-aggregator]]
