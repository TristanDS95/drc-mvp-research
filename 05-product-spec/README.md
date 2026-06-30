# 05 — Product spec

How the **merchant-acquiring** MVP will function, look, and be built. Distinct from `02-findings/` (research) and `03-reports/` (research synthesis) — these are *design outputs*. The reference for the product's shape is `00-overview/product-summary.md`.

> ## Direction (2026-06-11): merchant-facing
> This spec was rewritten when the MVP pivoted from a *consumer* "pay any number" send-app to a **merchant-acquiring** app. A **customer pays a merchant**; the customer pays the **exact sticker price** and the **merchant absorbs our fee (MDR)**; settlement is **instant pass-through** (we never hold funds). The **primary customer flow is customer-initiated USSD** — the customer **scans the merchant's QR** (a `tel:` USSD dial-through) or **dials the till** on any phone, with **no app and no internet** — and the **fallback is merchant-initiated "charge by customer number."** **USSD is MVP-critical, not v2.** **Merchant onboarding is in scope.** Consumers do **not** need an app; a consumer app is a much-later possibility.

> ## Subject to revision when team grows
> Every file here reflects current decisions made by the existing 4-person team (founder/PM + backend + mobile + designer). When we onboard two more members in Phase 0, their preferences will reshape parts of this spec — especially backend language, framework picks, screen-level patterns, and design-system details.
>
> **What's durable** (unlikely to change with new team):
> - The use-case envelope (merchant accepts a payment; customer pays the sticker price; merchant absorbs the MDR; instant pass-through, no custody; USSD as an MVP channel)
> - The customer-initiated USSD/QR primary flow + charge-by-number fallback
> - The merchant entity set and the transaction state machine
> - The visual direction (Wave-style minimal + distinctive coral accent) and the design system
> - The endpoint shape (REST + idempotency; server re-derives amount/fee/settlement)
> - AWS Cape Town for cloud; React Native + Expo for the merchant app; the vendor bundle (now including a USSD bearer)
>
> **What's most likely to evolve** (revisit with new team):
> - Backend language (Python vs Node) — intentionally still open (the built API is Python/FastAPI; treat as a working reference, not a locked call)
> - Specific framework / ORM / queue choices within whatever language is picked
> - Real MDR pricing (1% is a below-cost placeholder — see below)
> - Component-library specifics and screen-level layout density
> - Microinteractions, motion, font fallback chains, exact pixel tokens
> - API serialization details (snake_case vs camelCase; error-envelope shape)

## Files

| File | Purpose | Status |
|---|---|---|
| `functionality.md` | What the app does — use cases, flows, behaviour, business rules | **done** (decisions settled; pricing open) |
| `ui-spec.md` | Design-system foundations: colors, type, spacing, components, motion, tone | **done** (Wave-style minimal + coral accent) |
| `tech-stack.md` | Concrete stack choices with rationale | **done with two open items** (backend language; USSD bearer rate card) |
| `screens.md` | Screen-by-screen inventory (merchant app) + the customer USSD text flow | **done** |
| `data-model.md` | Entities, relationships, state machines | **done** (matches the built domain model) |
| `api-contracts.md` | Endpoints, request/response shapes, conventions | **done** (built surface + planned auth/webhook) |

## What's settled (the foundation)

**Functionality** — see `functionality.md`
- One flow: a **customer pays a merchant**. Primary = **customer-initiated USSD** (scan the merchant's QR `tel:` dial-through, or dial the till) on any phone — no customer app, no internet. Fallback = **merchant-initiated "charge by customer number"** push.
- **Pure pass-through** — we never hold funds; instant settlement to the merchant's mobile-money account per transaction.
- **The merchant absorbs the fee (MDR).** The customer pays the **exact sticker price**; the merchant nets **price − fee**.
- **Merchant onboarding is in scope** (business details + settlement account + till/short-code; business KYC flagged).
- The customer authorises with **their operator's own PIN prompt** (pawaPay/MNO push) — **we never see the PIN**.
- USSD is an **MVP channel**; consumers need no app.

**UI / UX** — see `ui-spec.md`
- Wave-style minimal structure with **coral** (`#FF6B5B`) accent + cream + charcoal — design system reused as-is.
- Inter / modern geometric sans; soft + flat components (12px rounding, no shadows); light + dark (follow system).
- Merchant-app nav (not a consumer send-app): **Encaisser / Paiements / Réglages**.
- Functional + warm French copy.

**Screens** — see `screens.md`
- **Merchant app:** onboarding (business details → settlement account → till) → Set PIN → home (QR + till + live payments feed) → charge-by-number fallback → payment detail → settings.
- **Customer USSD text flow:** dial/scan → (till pre-filled) → amount → confirm → operator PIN prompt.

**Tech stack** — see `tech-stack.md`
- React Native + Expo (merchant app); AWS Cape Town (af-south-1); Postgres + Redis.
- pawaPay (rails) + a **USSD bearer** (Africa's Talking / Infobip) + Africa's Talking (SMS/OTP) + Smile ID (business KYC) + Sentry + PostHog.

**Data model** — see `data-model.md`
- Core entities **Merchant**, **Transaction**, **LedgerEntry** (double-entry; accounts `customer:external`, `merchant:external`, `pawapay:clearing`, `revenue:fees`) — matching the built domain. Planned: **Merchant auth** (phone+OTP+PIN) and **OperatorEvent** (webhook audit).
- 10-state transaction state machine with an automatic refund pathway.
- **MDR is deducted from the merchant's settlement** (merchant nets amount − fee) — **never added to the customer's debit**.

**API surface** — see `api-contracts.md`
- REST, JSON, idempotency keys; the server re-derives amount, fee, and the merchant settlement target (never trusts the client).
- Built: `POST /transactions` (charge-by-number), `GET /transactions[/{id}]`, `GET /merchants[/{id}]`, `GET /merchants/{id}/qr.svg`, `POST /ussd` → CON/END, `GET /health`.
- Planned: merchant auth + onboarding writes; `POST /webhooks/pawapay` (RFC-9421 / ECDSA-P256).

## Pricing decision (open, and load-bearing)
**MDR is a placeholder 1%.** That is **below cost**: pawaPay's round-trip envelope is **≈3.5–5%** ([fees-and-costs.md](../02-findings/cross-cutting/fees-and-costs.md)), and the USSD-bearer per-session fee is additive ([ussd-gateway-providers.md](../02-findings/cross-cutting/ussd-gateway-providers.md)). A sustainable MDR is likely **~5–7%+**. Real pricing (a margin over the all-in rail + channel cost, possibly tiered, with the CDF↔USD spread) is an **open decision** — the code isolates it in one `pricing` module so it can change without touching the money spine.

## Pending decision
**Backend language**: Python (FastAPI) or Node (TypeScript). Detailed side-by-side in `tech-stack.md`. The built reference API is **Python + FastAPI**; defer the firm call until the backend engineer is hired (their preference matters most).

## What's next
The spec is comprehensive enough to revisit the MVP schedule and add cost estimation:
- Update `mvp-schedule.html` with spec-informed task pacing (including the USSD-bearer shortcode lead time and merchant-onboarding work).
- Add a cost projection (team, infra, vendor — pawaPay **and** the USSD bearer — legal, contingency).

## Authority
When the spec disagrees with the schedule or roadmap, **the spec wins** and the schedule/roadmap gets updated.

When the spec disagrees with research findings, we re-check the research before changing the spec.

When the spec disagrees with the preferences of newly-onboarded team members, **revisit the spec** — it was written to inform a decision, not to lock it.
