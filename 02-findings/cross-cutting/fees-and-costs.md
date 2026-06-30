# Fees & costs (cross-cutting)

- **Topic / entity:** unit economics of the two-leg MVP transaction (collect + payout) in DRC
- **Question it addresses:** what does it cost the business per cross-network payment under each candidate path?
- **Date researched:** 2026-06-04
- **Overall confidence:** Medium — pawaPay's per-leg fees are published and verifiable; DPO/MOKO/Onafriq DRC pricing is not public; operator-direct API pricing is not public ("M-Pesa Business team will inform you of the M-Pesa charging model" pattern).

## Bottom line
The only path with **publicly verifiable per-leg pricing** in DRC today is pawaPay. Direct operator APIs do not publish API-driven fees in DRC, and the other aggregators (DPO, MOKO, Onafriq) require sales contact for DRC rate cards. For modelling, **use pawaPay's published rates as the anchor**, treat consumer tariffs as a ceiling reference, and flag the others as "requires sales contact" until a real quote is in hand.

---

## Path A: pawaPay (rented rails, mobile-money-only) — published rates

Source: [pawaPay Fees page](https://www.pawapay.io/fees) and [pawaPay Plans page](https://www.pawapay.io/plans). All rates **company-published, Medium-High confidence**, exclusive of taxes.

| Direction | Operator | Total fee | Breakdown |
|---|---|---|---|
| Collection (deposit) | Vodacom M-Pesa DRC | **2.5%** | 1.5% MMO + 1% pawaPay |
| Collection (deposit) | Airtel DRC | **3.0%** | 2.0% MMO + 1% pawaPay |
| Collection (deposit) | Orange DRC | **3.0%** | 2.0% MMO + 1% pawaPay |
| Disbursement (payout) | Vodacom M-Pesa DRC | **2.0%** | MMO + pawaPay (composition not itemised) |
| Disbursement (payout) | Airtel DRC | **2.0%** | MMO + pawaPay |
| Disbursement (payout) | Orange DRC | **1.0%** | MMO + pawaPay |

**End-to-end two-leg cost (collect + payout), DRC, via pawaPay:**

| Collect from → Pay out to | Total | Notes |
|---|---|---|
| M-Pesa → M-Pesa (on-net) | **4.5%** | 2.5% + 2.0% |
| M-Pesa → Airtel | **4.5%** | 2.5% + 2.0% |
| M-Pesa → Orange | **3.5%** | 2.5% + 1.0% (Orange payout is the cheapest leg) |
| Airtel → M-Pesa | **5.0%** | 3.0% + 2.0% |
| Airtel → Airtel | **5.0%** | 3.0% + 2.0% |
| Airtel → Orange | **4.0%** | 3.0% + 1.0% |
| Orange → M-Pesa | **5.0%** | 3.0% + 2.0% |
| Orange → Airtel | **5.0%** | 3.0% + 2.0% |
| Orange → Orange | **4.0%** | 3.0% + 1.0% |

**Range: ~3.5%–5.0%** of transaction value per round-trip. **Confidence: Medium-High** for the inputs; **inference** for the round-trip composition (pawaPay charges separately per leg, the two add).

**Coverage gap:** Afrimoney **not supported** by pawaPay → cannot reach Africell users at all on this path.

---

## Path B: Direct operator APIs

Source: operator profiles in `02-findings/operators/`.

**Fees publicly disclosed by operator for API users:**
- **M-Pesa (Vodacom DRC)**: **not public** — "the M-Pesa Business team will review the application and inform you of the M-Pesa charging model" ([business.m-pesa.com DRC onboarding page](https://business.m-pesa.com/vodacom-drc/onboarding-your-business-in-drc/)). The consumer tariff is published but is not what an API merchant pays.
- **Orange Money DRC**: API merchant fees **not public**. Consumer tariff is published ([orange.cd tarifications](https://www.orange.cd/fr/orange-money/tarifications-orange-money.html)): off-net consumer transfer is 1%, withdrawals are tiered 9% → 1%, etc. These are end-user fees, not merchant API fees.
- **Airtel Money DRC**: API merchant fees **not public**. Consumer tariff page exists at airtel.cd/airtel_money/charges but did not extract (JS-rendered).
- **Afrimoney DRC**: API merchant fees **not public**; consumer tariff "displayed at agent and partner points" but not on the public web ([africell.cd Afrimoney service page](https://www.africell.cd/afrimoney/)).

**Implication:** Path B unit economics are **not knowable without a partner contact** in DRC. As a benchmark, pawaPay's "MMO portion" (1.5%–2% for collection, varying for payout) is a useful proxy for what the operators currently charge an aggregator and therefore an indicative floor on what a direct partner would pay. Whether a direct partner pays *less* than pawaPay's pass-through MMO rate, or *more*, depends on negotiated rates and volume. **Confidence: Low — not found in public docs.**

---

## Path C: Other aggregators (DPO, MOKO, Onafriq)

- **DPO Pay DRC**: per-transaction fees **not public**. PayAtlas summary cites general DRC PSP norms ("2.5–4.0% for card payments; 1,000–3,000 CDF per payout; 2–5% FX markup; ~5,000–10,000 CDF chargeback fee") but those are **market-wide, not DPO-specific** ([payatlas.com DRC PSPs page](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd)). DPO is also card-and-collection-first; real-time per-transaction payout pricing is opaque.
- **MOKO Afrika DRC**: per-transaction fees **not public**; "Tarification" page referenced but content not extracted.
- **Onafriq DRC**: per-transaction fees **not public**.

For all three, **rate cards require sales contact**. Use pawaPay's published numbers as the working anchor and assume Path C is **at least competitive with pawaPay** (any vendor charging materially more loses the deal) — but this is an **inference**, not a verified fact.

---

## Other cost components to factor

1. **FX cost CDF ↔ USD.** DRC's mobile money runs in both CDF and USD; payers and payees may prefer different currencies. PayAtlas-DRC cites typical PSP FX markup of **2–5% above interbank** as a market norm ([payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd)). pawaPay's plan materials mention "competitive FX rates" without naming a margin. **Treat 2–5% as a planning range; verify with the chosen aggregator. Confidence: Medium.**
2. **Settlement / payout-to-bank.** Settlement to a merchant bank account is implicit in the aggregator model; cadence is **2–5 business days** as a DRC PSP norm per PayAtlas, with longer holds above $10,000 USD per BCC review ([payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd)). Per-payout fees vary; PayAtlas cites **1,000–3,000 CDF** ($0.35–$1.10) per payout as a market norm. pawaPay does not list settlement fees in the materials reviewed. **Confidence: Medium for market norm; Low for vendor-specific.**
3. **Setup and monthly fees.** pawaPay Standard: **none** ([pawaPay Plans](https://www.pawapay.io/plans)). DPO/MOKO/Onafriq: not public; expect bilateral commercial agreements.
4. **Compliance / KYC costs.** Each aggregator runs merchant KYC at signup; this is non-recurring but adds 1–4 weeks elapsed onboarding on Standard plans (longer on Enterprise) — **inference, Medium confidence**.
5. **Tax withholding on payments to foreign merchants.** PayAtlas cites **2–5%** as a DRC market norm for foreign-merchant withholding ([payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd)). Relevant if our entity is foreign; treat as flag-for-investigation.

---

## Refund-leg fees (added 2026-06-17)

Relevant because our pass-through **auto-refunds the customer** when a payout fails after collection
succeeded — so the **per-transaction cost of a refunded payment** is a real loss line, not zero.

- **A refund is a billable transaction.** pawaPay's Plans page lists the fee as **"1% + MMO fee for
  Disbursements, Refunds\* & Remittances"** — a refund is billed like a disbursement (pawaPay 1% + the
  MMO fee). The refund disburses back to the **payer's** wallet, so the rate is the disbursement rate on
  the **payer's** operator. **Company-published (pricing page), Medium-High confidence.** ⚠️ The **`*`
  after "Refunds"** flags an unread footnote/caveat — confirm its terms. Source: pp-004.
- **Is the original collection fee reversed on a refund? Not found.** The refunds guide (pp-017), the
  fees page (pp-003), and the Plans page (pp-004) are all **silent** on whether the collection fee
  already charged is credited back. Payments-industry default is that it is **sunk** (not reversed) — but
  this is **unverified for pawaPay**; flag for sales/support. **Confidence: Low — not found.**
- **The exact fee isn't machine-readable.** The refund object/callback exposes **no fee field** (only
  `amount`/`status`), same as deposits and payouts (pp-018) — so a booked refund cost is an **estimate**
  from the published rate, to be reconciled against pawaPay's settlement statements.
- **Partial refunds are supported** ("you can initiate multiple partial refunds") (pp-017).

**Unit-economics implication.** A refunded (failed-payout) transaction earns **zero MDR** but still costs
the **collection fee** (assumed sunk) **plus a refund fee** (≈ the disbursement rate on the payer's
operator) — a **real per-transaction loss** of roughly `collect% + refund%` of the amount (e.g. a Vodacom
payer ≈ 2.5% + 2.0% = ~4.5%). Keep failed-payout rates low; this is a direct loss line.

---

## What this means for the MVP

Take a representative example: a $20 round-trip cross-network payment (collect from one wallet, pay out to another).

- **pawaPay, mid-range path (e.g., Airtel → M-Pesa):** 5.0% → $1.00 per $20 round-trip.
- **pawaPay, cheapest path (e.g., M-Pesa → Orange):** 3.5% → $0.70 per $20.
- **pawaPay, most expensive path (Airtel/Orange → M-Pesa/Airtel):** 5.0% → $1.00 per $20.
- **Plus FX (if currency switch): another ~2–5%**.

Even on the cheapest path, per-transaction cost is non-trivial; for low-value payments (under ~$10) the percentage fees are crushing. For the MVP target (urban smartphone merchant pay) this is acceptable for $20+ tickets but probably uneconomic for $5 corner-shop payments unless absorbed by the merchant.

## Confidence summary
- **High:** pawaPay's published DRC fee table.
- **Medium:** the 3.5%–5.0% round-trip envelope is a fair planning anchor.
- **Low:** DPO/MOKO/Onafriq DRC rates; operator-direct API rates; settlement-to-bank cost; FX markup specifics — all require vendor contact to verify.

## Sources
- pp-003, pp-004, pp-017, pp-018 (pawaPay published fees / plans / refunds guide / refund object)
- mp-001, mp-002, mp-006 (M-Pesa DRC onboarding and consumer tariffs)
- om-001, om-002 (Orange Money API + DRC tariff)
- am-005 (airtel.cd Airtel Money charges page — JS-opaque)
- af-002 (africell.cd Afrimoney page)
- xo-005 (PayAtlas DRC PSPs)

[[mpesa]] · [[orange-money]] · [[airtel-money]] · [[afrimoney]] · [[pawapay]] · [[dpo]] · [[moko]] · [[onafriq]]
