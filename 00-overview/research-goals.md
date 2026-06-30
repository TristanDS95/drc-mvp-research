# Research goals

This workspace began by investigating the technical feasibility of standing up the MVP on rented rails. It has since broadened to also cover **how comparable fintech products are structured, monetised, and rolled out**, and — after the 2026-06 pivot to a **merchant-facing** product — to treat **customer-initiated USSD as the primary, MVP-critical channel**.

## Audience / product scope (merchant-facing)
The product is **merchant-facing**: merchants accept cross-network mobile-money payments; **customers need no app or internet** — they pay by scanning the merchant's QR or dialing a USSD till. So the primary channel is **customer-initiated USSD**, not a smartphone app, and **merchant onboarding is in scope**. A consumer-facing version is a much-later possibility, not the MVP. Research should centre the merchant and the offline customer.

## What we are investigating
1. **Individual mobile money operator APIs** — M-Pesa (Vodacom), Orange Money, Airtel Money, Afrimoney (Africell).
2. **Merchant-side interoperability / aggregator platforms** — pawaPay, DPO, MOKO, Onafriq, and others.
3. **Payee identification** — how a customer designates the **merchant** to pay: our merchant **till / short-code** (dialed, or carried in a QR `tel:` dial-through). Merchant onboarding **is** in scope; a customer keying an arbitrary number is the old consumer model.
4. **KYC / onboarding** — identity requirements and how they can be met.
5. **USSD as the primary customer channel** — feasibility, providers, and design of the customer-initiated USSD/QR flow (a first-class, **MVP-critical** channel — not phased). See `02-findings/cross-cutting/ussd-gateway-providers.md` (the server-side gateway we use) and `offline-ussd-feasibility.md` (the client-side automation alternative we did not take).
6. **Comparable fintech products / companies** — African leaders plus select global analogs (e.g., M-Pesa, Wave, OPay/PalmPay, MTN MoMo, Chipper, Flutterwave, Onafriq; bKash, GCash, Paytm, Mercado Pago, WeChat Pay). Studied through three lenses: **(a) product structure & architecture, (b) how they make money (monetisation & value capture), (c) feature rollout & sequencing.** Goal: learn how to structure our app, how to derive value, and how to control development and feature rollout. Recorded in `02-findings/comparables/`.

## What we want to get out of it
For each operator and aggregator, a clear, sourced picture of:
- whether it supports **collecting** from the customer's wallet and **settling** to the merchant's wallet, across networks;
- the **per-transaction cost** (both legs) — borne by the merchant as the MDR;
- the **payment flow the customer actually experiences** (e.g., dialing a till / scanning a QR, then approving in their operator's own menu);
- **availability in the DRC**, developer access / sandbox, and onboarding terms.

These feed one decision: can we build the MVP on these rails, by which path, at what cost, and with what UX — captured in `03-reports/`.

## Scope notes
- **Comparative product-strategy research is now in scope** (how comparable fintechs structure, monetise, and sequence their products) — added deliberately to inform our own structure, value model, and rollout.
- **Still secondary:** quantitative demand forecasting / market sizing (whether/how many DRC users will adopt) — we may touch adoption qualitatively via comparables, but rigorous demand modelling remains a separate track.
- **Legal / licensing** is flagged as an item to investigate, not a defined requirement.
- **Accuracy standards in `CLAUDE.md` still apply to all of the above** — source every claim, label fact vs self-reported vs inference, record confidence and date.
