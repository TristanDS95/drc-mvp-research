# Product summary

A **merchant-facing** mobile-money interoperability app for the DRC: merchants accept
payments from customers on **any** mobile money network (Vodacom M-Pesa, Airtel, Orange),
with cross-network bridging handled behind the scenes, plus clean dashboards to see and
reconcile what comes in. **Customers do not need the app** — they pay via the merchant's
app or by **USSD** from any phone. A consumer-facing version may follow later.

> **Direction change (2026-06-11):** the MVP pivoted from a *consumer* "pay any number"
> app to a *merchant-acquiring* app. The underlying money movement is unchanged (collect
> from a customer, settle to a merchant); what changed is who the product is *for* (the
> merchant) and that **USSD is now an MVP-critical channel**, not a later phase.

## MVP shape (decisions so far)
- **Who it's for:** **merchants** — starting with retail like fuel and small shops. The
  app user is the merchant; the customer is served *through* the merchant (app or USSD).
- **Launch wedge:** ~**40 gas stations + pop-up stores** already lined up — merchant
  acquisition (usually the hardest part of a new acquiring product) is substantially
  de-risked for v1.
- **Channels:** a **merchant app** (web dashboard + mobile) **and** a first-class
  **USSD** channel so customers on feature phones (or without the app) can pay. USSD is
  **in the MVP**, not phased after — its per-operator shortcode lead time means we plan it
  deliberately. See `02-findings/cross-cutting/offline-ussd-feasibility.md`.
- **Settlement:** **instant pass-through** to the merchant's mobile-money account per
  transaction (keeps us a pure pass-through that never holds funds). Batched settlement is
  a later option with float/licensing implications.
- **Pricing:** the **merchant absorbs the fee (MDR)** — the customer pays the sticker
  price; the merchant nets price − fee. (1% placeholder.)
- **Merchant onboarding is now in scope** (it's a merchant app): register merchants,
  their settlement details, and a till/short code; KYC is a flagged item.
- **Rented rails for the MVP** (an aggregator's infrastructure — pawaPay) rather than
  direct operator integration. Owning the rails is a long-term aim, not the MVP.
- **Out of scope for v1:** bank-account settlement, rewards/loyalty, bill pay, a separate
  consumer app.

## The money-movement engine (why the APIs matter)
Each payment is two legs on the underlying rails: **collect** funds from the customer's
mobile money, and **settle** to the merchant's mobile-money account, bridging networks
when they differ. Feasibility, cost, and payment UX all turn on what the operator APIs and
aggregators permit for these two legs — and on what the **USSD** path allows for
customer-present, feature-phone payments.
