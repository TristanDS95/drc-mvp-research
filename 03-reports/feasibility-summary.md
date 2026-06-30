# Feasibility summary — DRC payment-aggregator MVP on rented rails

- **Date:** 2026-06-04
- **Author:** Claude Code (agentic research)
- **Built from:** all of `02-findings/` and the two prior comparison reports ([operator-api-comparison.md](operator-api-comparison.md), [aggregator-comparison.md](aggregator-comparison.md)).

> **Reframed 2026-06-11 (team pivot to a merchant-facing MVP).** The feasibility verdict and economics below are unchanged — the same two-leg engine underlies the merchant product. What changed is the *framing*: a **customer pays a merchant**, the **merchant absorbs the fee (MDR)** while the customer pays the exact sticker price, the **primary customer action is USSD** (scan the merchant's QR / dial the till — no customer app), and **merchant onboarding is now in scope** (the old "no merchant onboarding in v1" decision is reversed). Technical facts and the cost table are unchanged. See [`00-overview/product-summary.md`](../00-overview/product-summary.md).

## Bottom line
**The MVP is technically feasible on rented rails today.** A **merchant-acceptance** app — letting a merchant accept payment from a customer on any of DRC's three biggest networks (Vodacom M-Pesa, Airtel, Orange — covering ~99% of DRC mobile money subscribers), with the customer paying by USSD and needing no app — can be built using a single aggregator integration. The MVP's two-leg flow (collect from the customer's wallet → settle to the merchant's wallet, bridging when networks differ) is supported end-to-end with publicly verifiable pricing on **pawaPay**. Per-round-trip cost is in the **3.5%–5.0% range** before FX — this is the **cost the merchant absorbs as MDR** (the 1% placeholder is below it; real MDR is an open pricing decision). Sandbox is open and self-serve. **The main constraint is unit economics on low-value tickets** (sub-$10 round-trips are uneconomic at 4–5%) and **the Afrimoney coverage gap** (<1% market share, but a hard zero on pawaPay; only MOKO Afrika among researched aggregators covers Afrimoney). **The main flag for separate investigation is legal/licensing** — operating a payments app on top of DRC's regulated rails without a BCC EMI/PSP licence likely requires a licensed-local-partner arrangement, which the BCC framework appears to require for foreign PSPs ([payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd) — secondary); merchant onboarding adds merchant KYC to that track.

## Detail

### What is achievable today

1. **The money-movement engine works.** Every aggregator researched (pawaPay, DPO, MOKO, Onafriq) implements both collection (debit the customer's mobile money wallet) and disbursement (credit the merchant's mobile money wallet), and bridges between networks at the settlement layer (collect from any supported network → balance held with aggregator → disburse to any supported network). The DRC operators support this on the API side: Vodacom M-Pesa has C2B + B2C + status APIs ([business.m-pesa.com Developers page](https://business.m-pesa.com/developers/)); Airtel Money has Collection + Disbursement via the unified Airtel Africa portal ([tech-ish.com Airtel developer portal article, 2021-11-19](https://tech-ish.com/2021/11/19/developer-portal-airtel-money-api/)); Orange Money DRC has a public Web Payment / M Payment API for collection ([developer.orange.com om-webpay](https://developer.orange.com/apis/om-webpay)); Afrimoney has announced GSMA-spec merchant APIs in Dec 2023 ([africell.com 2023-12-07](https://www.africell.com/story/afrimoney-adopts-mobile-money-specification/)).
2. **Merchant settlement is addressed by MSISDN; the customer pays by USSD.** Every aggregator addresses payouts by phone number (`+243`-prefixed), so settling to the merchant's wallet is solved (operator inferred from prefix; manual override for edge cases). On the customer side the pay action is **USSD** — scanning the merchant's QR (a `tel:` USSD dial-through) or dialing the till short-code — so the customer needs no app. No DRC *national* QR standard exists (operator/PSP QR is trialled in Kinshasa but not interoperable), which is exactly why we use a `tel:` dial-through rather than a proprietary scanner. See [payee-identification.md](../02-findings/cross-cutting/payee-identification.md) and [offline-ussd-feasibility.md](../02-findings/cross-cutting/offline-ussd-feasibility.md).
3. **Customer KYC rides existing SIM registration.** Every DRC mobile-money wallet is bound to a KYC'd SIM. The customer leg can stay ID-light because the MVP passes through to operator wallets rather than holding balances. App-side ID verification (Smile ID etc.) is available later if a held-balance model is added. **Merchant onboarding is now in scope**, so merchant KYC is a flagged item (pawaPay's sub-merchant questionnaire applies; any additional merchant KYC is on the legal track). See [kyc-and-onboarding.md](../02-findings/cross-cutting/kyc-and-onboarding.md).
4. **Sandbox to live in days–weeks** if pawaPay is the chosen aggregator: self-serve sandbox, KYC questionnaire, go-live on Standard plan with published rates ([pawaPay Plans page](https://www.pawapay.io/plans)).

### What the user actually experiences in the MVP

When a customer pays a merchant (primary flow — customer-initiated USSD, no customer app):

1. The customer **scans the merchant's QR** (a `tel:` USSD dial-through, e.g. `tel:*123*1001%23`) or **dials the till short-code** (`*123*1001#`) and enters the amount. They pay the **exact sticker price**.
2. Our backend calls the aggregator's collection API → aggregator triggers a **PIN prompt on the customer's phone in their operator's UX** (M-Pesa PIN prompt, Airtel Money PIN prompt, etc.). Customer enters PIN.
3. Aggregator credits our pawaPay balance.
4. Aggregator's payout API fires to **settle to the merchant** — instant pass-through, we never hold funds. The merchant receives a **normal incoming transfer on their operator wallet** plus the operator's confirmation SMS, and the sale appears reconciled in the merchant dashboard. The merchant nets sticker price − MDR.
5. Both legs typically complete in seconds (operator-side); optional aggregator-to-bank settlement (if the merchant opts in) is **2–5 business days** per DRC PSP norm.

**Fallback flow:** the merchant keys the customer's number + amount into the merchant app ("charge by customer number"), firing the same two legs; the customer still approves via their operator's PIN prompt.

**Merchant onboarding is now in scope** (the prior "no merchant onboarding in v1" decision is reversed): the merchant registers a settlement mobile-money account and is issued a till / short-code for the QR + USSD flow. The customer side stays zero-onboarding — any SIM-registered phone can pay.

### What is NOT achievable / what hurts

- **Afrimoney coverage is patchy** through the leading aggregators. pawaPay does not list Afrimoney; Onafriq's DRC Visa Pay launch named only the three big operators; DPO has not confirmed Afrimoney. **Only MOKO Afrika explicitly lists Africell** ([moko-africa-documentation.vercel.app](https://moko-africa-documentation.vercel.app/)). Given Afrimoney's <1% mobile money market share ([rdc-analyse.org feasibility analysis](https://rdc-analyse.org/files/2025/08/2025_08_CAT_Feasibility-analysis-of-Mobile-Money-in-eastern-DRC_VE-1.pdf)), the trade-off is real but small.
- **Unit economics on small tickets.** At 3.5%–5.0% round-trip + 2–5% FX (if currency switch), a $5 payment costs $0.30–$0.50 in fees. For target use cases (corner-shop spend), this is steep. Acceptable for $20+ tickets — moderate for $10–$20 — punishing below $10.
- **Operator-direct API per-leg fees are not publicly disclosed in DRC.** Even M-Pesa's onboarding doc explicitly defers pricing: "the M-Pesa Business team will review the application and inform you of the M-Pesa charging model." This means **direct integration unit economics cannot be modelled without a partner conversation**. pawaPay's pass-through MMO fees (1.5%–2%) are the best available proxy for what the operators charge an aggregator. See [fees-and-costs.md](../02-findings/cross-cutting/fees-and-costs.md).
- **Customer approval flow stays in the operator's USSD UX.** None of the aggregators offers a "smoother approve" path — the customer always sees the M-Pesa / Airtel Money / Orange Money PIN prompt. This is fine for the MVP (and is *why* no customer app is needed — the operator's own prompt secures the payment); deeper UX would need operator partnerships later.
- **Bilateral interop is real at the consumer level (M-Pesa DRC ↔ Orange Money is live; Airtel claims tri-operator interop in DRC), but with friction (extra cost). The aggregator path bypasses this friction by handling the bridge in its own settlement layer.

### Licensing and legal posture (flag for investigation)

- The **BCC** (Banque Centrale du Congo) supervises EMIs, PSPs, banks, and FX in DRC ([generisonline.com regulatory framework](https://generisonline.com/understanding-the-regulatory-framework-for-digital-payments-and-fintech-companies-in-the-democratic-republic-of-the-congo/)).
- A third-party PSP guide states that **"Foreign PSPs must partner with licensed local entities or obtain BCC authorisation; independent operation is prohibited"** ([payatlas.com](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd) — **secondary, Medium confidence**, **not verified against BCC primary docs in this research**).
- **The MVP shape matters:**
  - If the MVP is a **thin wrapper / pass-through** (no held balance, no end-user wallet), it likely operates under the aggregator's BCC posture (the aggregator carries the licensing weight). pawaPay being already-operational in DRC suggests this works.
  - If the MVP introduces a **held-balance product** (top-up, custodial wallet), it likely becomes an EMI/PSP candidate in its own right, with the full BCC framework attached.
- **Legal/licensing is a separate track per `00-overview/research-goals.md` and `01-questions/kyc-and-onboarding.md`** — flag to investigate before live launch, not before MVP build.

## Comparison / key data

### Path matrix (collect → bridge → payout)

| Path | Coverage of DRC MNOs | Fees public? | Sandbox today | Onboarding | Recommended? |
|---|---|---|---|---|---|
| pawaPay (sole) | 3 of 4 (no Afrimoney) | **Yes** (3.5%–5.0% round-trip) | Self-serve | Days–weeks | **Lead recommendation** |
| MOKO Afrika (sole) | **4 of 4** (incl. Afrimoney) | No | Self-serve | Days–weeks | **Secondary** (for Afrimoney) |
| DPO Pay (sole) | 3 of 4 likely | No | Yes | Weeks | Possible — multi-rail (cards) but heavier shape |
| Onafriq (sole) | 3 of 4 likely | No | Yes | Weeks–months | Enterprise-shaped; over-engineered for MVP |
| Operator-direct (M-Pesa + Airtel + Orange + Afrimoney parallel) | 4 of 4 (max) | No | Mixed (Airtel/M-Pesa easiest) | Months across 4 contracts | **Not recommended for MVP** — wrong altitude |
| Hybrid: pawaPay + MOKO for Afrimoney | 4 of 4 | Partly | Yes | Days–weeks | Good if Afrimoney coverage matters |

### Decision-relevant per-transaction economics (round-trip), pawaPay

| Round-trip path | Total fee |
|---|---|
| M-Pesa → Orange | 3.5% (cheapest) |
| M-Pesa → M-Pesa | 4.5% |
| Airtel → Orange | 4.0% |
| Airtel → M-Pesa or Airtel | 5.0% (most expensive) |
| Orange → Orange | 4.0% |
| + FX (currency switch) | +2–5% |
| + Settlement to bank (per DRC PSP norm, 1k–3k CDF) | $0.35–$1.10 per payout |

## Confidence & open questions

- **Confident about:**
  - The two-leg flow (collect → bridge → payout) is technically feasible end-to-end via at least one aggregator (pawaPay) with publicly verifiable pricing.
  - pawaPay covers the three big DRC operators; MOKO is the only researched aggregator covering all four including Afrimoney.
  - Customer KYC rides existing SIM registration (customer leg stays ID-light, no held balance); merchant onboarding is now in scope, with merchant KYC a flagged item (pawaPay's sub-merchant questionnaire applies).
  - DRC supports both CDF and USD at every operator and aggregator level.
  - Operator-direct API per-leg pricing is not public; aggregator pass-through is the best public proxy.

- **Uncertain / unverified:**
  - The **MDR / pricing decision** — the merchant absorbs the fee, but the 1% placeholder is below the 3.5–5% cost; the viable MDR (~5–7%+) is an open pricing decision, not a research finding.
  - Whether a **`tel:` USSD dial-through QR / dialable till** behaves consistently across all three operators, plus per-operator shortcode provisioning/lead times (see [offline-ussd-feasibility.md](../02-findings/cross-cutting/offline-ussd-feasibility.md)).
  - DRC-specific rate cards from DPO, MOKO, Onafriq.
  - Settlement cadence per aggregator.
  - Whether DPO and Onafriq cover Afrimoney in DRC.
  - The 2025/2026 BCC stance on cross-network interoperability (only 2014 source located).
  - Whether the MVP will be required to partner-with-a-licensed-local-entity, and what **merchant onboarding/KYC** is required (legal track).

- **Still missing (and how to get it):**
  - Sales-quoted rate cards from DPO/MOKO/Onafriq for DRC.
  - Verified current BCC guidance and licensing brackets — BCC primary docs and counsel.
  - Pilot run with pawaPay sandbox + 1–2 real DRC SIMs to time end-to-end UX, success rates, error paths.
  - Confirmation of whether pawaPay's "settle where and when you want" includes a DRC merchant bank account, foreign account, or both.
  - Afrimoney rollout status of the Dec 2023 GSMA-spec merchant APIs.

## Sources
By ID, from `04-sources/sources.md`:
- Operators: mp-*; om-*; am-*; af-*
- Aggregators: pp-*; dp-*; mk-*; on-*
- Cross-cutting: xo-001 through xo-018
