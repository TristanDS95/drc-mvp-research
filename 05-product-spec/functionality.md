# Functionality (v1 MVP)

What the app does. Decisions captured as they're made; updated when revisited.

- **Last updated:** 2026-06-11
- **Status:** core decisions settled (merchant pivot); MDR pricing is an open decision; minor edges (business-KYC depth, multi-currency on USSD) flagged

> **Direction (2026-06-11): merchant-facing.** This spec was rewritten when the MVP pivoted from a *consumer* "pay any number" send-app to a **merchant-acquiring** app. A **customer pays a merchant**; the customer pays the **exact sticker price** and the **merchant absorbs our fee (MDR)**. The **primary customer flow is customer-initiated USSD** (scan the merchant's QR / dial the till on any phone — no customer app, no internet), with a **merchant-initiated "charge by customer number"** fallback. **USSD is MVP-critical, not v2.** **Merchant onboarding is in scope.** The durable structure is: one merchant-acquiring flow, pure pass-through, merchant absorbs MDR, customer authorises on the operator's own PIN prompt, USSD as an MVP channel. Surface details are most likely to evolve.

## Settled decisions

### 1. Use case — a customer pays a merchant (one flow)
The product accepts a **customer-to-merchant** payment. The **merchant is the app user**; the **customer is served through the merchant** (via USSD/QR, or a merchant-initiated charge) and needs no app of their own. The payee is a **registered merchant** (a `Merchant` record with a settlement number and a till/short-code) — **not** an arbitrary typed phone number. Underlying money movement is two legs: **collect** from the customer's mobile-money wallet, **settle** to the merchant's mobile-money wallet, bridging networks when they differ.

### 2. Wallet model — pure pass-through
We **never hold funds**. Each payment is customer's wallet → pawaPay → merchant's wallet, settled **instantly per transaction**. No in-app balance, no custody, no float. (Batched settlement is a later option with float/licensing implications — out of scope for v1.)

### 3. Fee direction — the **merchant absorbs the MDR**; the customer pays the sticker price
The customer is debited the **exact amount on the sticker / till** — never amount + fee. Our fee (the **Merchant Discount Rate, MDR**) is **deducted from the merchant's settlement**: the merchant nets **amount − fee**. The fee is **booked as revenue only on a successful settlement**; a refund returns the **full amount** to the customer with **no fee**. (This is exactly what the built orchestrator does: `merchant_amount = amount − fee`; `revenue:fees` credited only on `payout_succeeded`.)

> **MDR pricing is an OPEN decision (and 1% is below cost).** The code uses a **1% placeholder** (`pricing.FEE_BASIS_POINTS = 100`). pawaPay's round-trip cost is **≈3.5–5%** ([fees-and-costs.md](../02-findings/cross-cutting/fees-and-costs.md)) and the USSD-bearer per-session fee is **additive** ([ussd-gateway-providers.md](../02-findings/cross-cutting/ussd-gateway-providers.md)), so a sustainable MDR is likely **~5–7%+**. Real pricing (a margin over the all-in rail + channel cost; possibly tiered by merchant/volume; including the CDF↔USD spread) is to be decided. Pricing is isolated in one module so the number can change without touching the money spine.

### 4. Primary customer flow — **customer-initiated USSD** (scan QR or dial the till)
The default, fully offline path. The merchant displays a **static sticker** carrying a **QR** and a printed **dial string**:
- **Scan the QR:** the QR encodes a `tel:` USSD dial-through, e.g. `tel:*123*1001%23`. The customer's camera offers to dial the underlying USSD string `*123*1001#` (Android; **iOS blocks `*`/`#` dialing**, so iPhone customers use the printed string or the merchant-initiated fallback).
- **Dial the till:** the customer simply dials `*123*1001#` on **any phone** — feature phone or smartphone, no app, no internet.

Either way the input lands in **our USSD channel pre-filled with the till**. The customer is then prompted (over USSD) for the **amount** and a **confirm**. On confirm, we initiate the collection via pawaPay, and **the operator pushes a PIN prompt to the customer's own handset** to authorise — **we never see the PIN**. (`*123#` is a placeholder shortcode; the live code is provisioned with the USSD bearer per `01` below and `tech-stack.md`.)

### 5. Fallback customer flow — **merchant-initiated "charge by customer number"**
When the customer can't scan/dial (e.g. an iPhone, a damaged sticker, or a merchant who prefers to drive), the **merchant enters the customer's mobile-money number and the amount in the merchant app**. This calls `POST /transactions {customer_msisdn, merchant_id, amount}`; pawaPay issues a **deposit request** to that number, and the **MNO pushes the PIN prompt to the customer** to approve. Same money path, same settlement, same fee direction — only the *initiation* differs.

### 6. Identity disclosure — operator default both ways
- **To the customer:** the operator's own PIN-prompt / confirmation SMS (whatever Vodacom/Airtel/Orange shows). The USSD confirm step shows the **merchant's name** ("Pay 10.00 USD to Alpha Gas Station?") so the customer knows who they're paying.
- **To the merchant:** the payment appears in the merchant app's live feed with amount, time, state, and the customer's number; no extra customer PII is surfaced.

### 7. Merchant onboarding — **in scope** (it's a merchant app)
Registering a merchant captures: **business name/details**, a **settlement mobile-money number** (+ its operator, resolved if omitted), and a **till / short-code** the customer references. The till drives the merchant's QR/dial string (`merchant_payment_code`). **Business KYC is a flagged item** — the depth and provider (Smile ID, business documents) are to be decided; the MVP record is lightweight and onboarding tightens as volume/regulation requires. Merchant status is `active | suspended`.

> _Build note:_ the domain `Merchant` model and the read/QR endpoints exist; **merchant-create/update writes and merchant auth (phone + OTP + PIN) are planned, not yet built.** In the zero-setup demo, merchants are seeded.

### 8. Currency — USD default, CDF supported; USSD single-currency for now
The platform supports **USD** and **CDF** (the `Money` type carries both, exact integer minor units). The merchant app defaults to **USD** with CDF available. **The built USSD flow is single-currency (USD) for now** — multi-currency on the USSD channel is a flagged follow-up (currency selection over a USSD menu is clunky; revisit).

### 9. Merchant home — QR + till + live payments feed
The merchant's main screen shows their **payment QR** and **printed dial string** (for the customer to scan/dial), their **till/short-code**, and a **live feed of incoming payments** with state chips (pending / settled / failed-refunded). This is the merchant's day-to-day surface — "show the code, watch the money land."

### 10. Receipts / records — in-app payment list + WhatsApp share
The merchant has an in-app **payments list** and a **payment detail** view (amount, net after MDR, customer number, operator, time, state, transaction ID). Each detail has a **"Share via WhatsApp"** button (e.g. to send the customer a confirmation). The customer's own operator confirmation SMS arrives in parallel as an independent signal.

### 11. Merchant auth — phone-OTP first, then PIN (planned)
First sign-in: **phone number + OTP**. Every app open after that, and before sensitive actions (changing the settlement number, charging by number): **PIN**. **Forgot PIN** = OTP re-verification to the registered number → set a new PIN. PINs are **Argon2id-hashed, never logged, never recoverable** (reset only via OTP); lock after repeated failures. _(Auth is specced but not yet built — see `api-contracts.md`.)_

### 12. Failure UX — automatic refund to the customer + merchant sees it
When the collection succeeds but the **settlement to the merchant fails**, the backend **automatically refunds the full amount to the customer** (no fee), and the merchant app shows the payment as **failed-refunded**. If the refund itself fails twice, the transaction escalates to **manual review** (funds stuck at pawaPay; ops resolves). The customer also gets the operator's own refund-confirmation SMS. (This mirrors the built state machine exactly.)

### 13. Pending / interrupted recovery — resume from backend
If the merchant app loses connectivity mid-payment, on reopen it **fetches the transaction's current state from our backend** and shows the right state (pending / settled / failed-refunded). The customer's USSD session is managed by the aggregator and is independent of the merchant app, so a merchant-app hiccup never blocks a customer's payment.

### 14. Operator availability — banner + warning, don't block
The backend tracks per-operator success rates (rolling window). If an operator degrades, show a **status banner** in the merchant app (and, where relevant, a note on the charge-by-number step). **Don't hard-block** — surface the risk and let the merchant decide. When the operator recovers, the banner clears. _(Planned; the operator-status surface is a v1 nicety, not a money-path dependency.)_

### 15. Risk / velocity limits — conservative, server-enforced
Apply conservative per-merchant and per-transaction caps initially (amount ceilings + velocity caps), relaxing toward operator caps as a merchant builds clean history. Enforced server-side (the server re-derives amount/fee/limits; never trusts the client). Exact thresholds are a build-time setting; align with BCC operator caps (e.g. Tier-1 per-BCC Directive #24) and revisit with legal.

### 16. USSD / feature-phone — **first-class MVP channel** (not v2)
**This is the primary customer channel and it is in the MVP.** The customer reaches us over the GSM signalling channel (no internet) by dialing our shortcode or scanning the merchant's `tel:` QR. We **rent the USSD *bearer*** (shortcode + gateway) from an aggregator and **already own the USSD *application*** (our `ussd/` channel). Going live is a **thin adapter** at the `/ussd` boundary plus a provisioned shortcode — **no money-logic change**. Full feasibility + provider/cost detail in [ussd-gateway-providers.md](../02-findings/cross-cutting/ussd-gateway-providers.md). Estimate below.

---

## MVP channel — USSD bearer (rent the transport; we own the menu)

| Sub-decision | Detail |
|---|---|
| **What we build** | A thin adapter at the `/ussd` HTTP boundary for the chosen aggregator's wire format (form fields; the full `*`-joined `text`). The menu logic + money path already exist and are channel-agnostic. |
| **Bearer provider** | **Africa's Talking** (front-runner — confirms **dedicated USSD codes in the DRC**; uses the exact `sessionId/phoneNumber/serviceCode/text` → `CON/END` model our code speaks). **Infobip** (directly or via Telerivet) is the alternative. See [ussd-gateway-providers.md](../02-findings/cross-cutting/ussd-gateway-providers.md). |
| **What the bearer does NOT do** | It only transports the **menu**. The **money** still moves through **pawaPay** (its USSD *payments* product is Nigeria-only today). So USSD-channel cost is **additive** to pawaPay's fees. |
| **Recurring cost (DRC, secondary source — confirm)** | ~**$1,000** one-time setup + ~**$200/telco/month** (≈$600/mo for 3 operators) + ~**$0.034 per dial session** (charged on every session, including abandoned ones). Indicative only; get a real rate card on contact. |
| **Lead time** | Shortcode provisioning via an aggregator should beat the direct-MNO route (the direct route cited 3–9 months); the **aggregator** lead time is not yet confirmed — flag for launch planning. |
| **Open question** | Whether BCC/ARPTC requires VAS/WASP or PSP/EMI licensing for a payment USSD menu specifically — flag for legal. |

**Do NOT self-host a USSD gateway for the MVP** — that needs a per-operator VAS/WASP agreement + an ARPTC-allocated shortcode + a gateway link to each MNO (the same per-operator-contract barrier that makes owning the payment rails impractical for v1). Rent now; reconsider only at scale.

---

## Out of scope for v1 (catalog for later)
- **A separate consumer app** (a customer-facing "pay any merchant" app) — a much-later possibility, not v1.
- **Batched / scheduled settlement** (instant pass-through only in v1).
- **Bank-account settlement** (mobile-money settlement only).
- **Multi-currency on the USSD channel** (USD-only on USSD for now).
- **Owning the rails** (direct operator integration) and **self-hosting the USSD gateway** — both reconsidered at scale.
- **Rewards / loyalty, bill pay** — not in the acquiring MVP.
- **Afrimoney coverage** via MOKO Afrika ([aggregator-comparison.md](../03-reports/aggregator-comparison.md)).
- **Lingala / Swahili UI** (French first; USSD copy in French, English in code today).
- **Biometric merchant-app auth** layered over PIN.

---

## Open questions / flags for revisit
- **Real MDR pricing.** The single most important open decision — 1% is below cost; settle a sustainable, possibly-tiered MDR over the all-in (pawaPay + USSD bearer) cost.
- **Business-KYC depth and provider** for merchant onboarding (Smile ID business docs?). Flag for legal + onboarding design.
- **USSD-channel licensing** (VAS/WASP vs PSP/EMI for a payment menu) — flag for legal.
- **pawaPay name-lookup** — if available, the merchant app / charge-by-number step could show a customer-name confirmation; if not, proceed on number only. Flag.
- **iPhone customer coverage** — iOS blocks `*`/`#` USSD dialing, so iPhone customers rely on the printed dial string or the merchant-initiated charge-by-number fallback. Acceptable for v1; revisit.
- **pawaPay DRC USSD roadmap** — if pawaPay adds USSD collections in DRC, one vendor could cover both payments and the channel.

---

## Sources
- [product-summary.md](../00-overview/product-summary.md) — the merchant-pivot product shape (reference)
- [ussd-gateway-providers.md](../02-findings/cross-cutting/ussd-gateway-providers.md) — USSD bearer providers, DRC coverage, cost, architecture fit
- [offline-ussd-feasibility.md](../02-findings/cross-cutting/offline-ussd-feasibility.md) — the contrasting client-side USSD-automation architecture
- [fees-and-costs.md](../02-findings/cross-cutting/fees-and-costs.md) — pawaPay round-trip cost envelope (drives the MDR floor)
- [kyc-and-onboarding.md](../02-findings/cross-cutting/kyc-and-onboarding.md) — KYC tiers and SIM-tied wallets
- [own-aggregator.md](../02-findings/cross-cutting/own-aggregator.md) — pass-through model + why we rent rails (and the bearer)
- [02-findings/app-development/](../02-findings/app-development/) — pan-African fintech research (dual currency, WhatsApp dominance, native-vs-RN, Wave's stack, etc.)
