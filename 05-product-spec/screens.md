# Screens (v1 MVP)

Screen-by-screen inventory for the **merchant app**, plus the **customer's USSD text flow**. Drives mobile-engineer task pacing and designer handoff.

- **Last updated:** 2026-06-11
- **Status:** v1 inventory complete (merchant pivot)

> **Direction (2026-06-11): merchant-facing.** These are the **merchant's** screens — the app user is the merchant. The **customer never opens an app**: they pay over **USSD** (scanning the merchant's QR or dialing the till), so the customer's experience is a **USSD text flow** (last section), not a set of app screens. The merchant-initiated **charge-by-customer-number** path is an in-app fallback.

> **Subject to revision when team grows.** Reflects current decisions with the existing 4-person team. New engineers/designers may shift specific screen patterns, component-library choices, and platform conventions. The app shape (onboarding → home with QR + feed → charge-by-number → settings) is durable; surface details (iconography, density, motion) are most likely to evolve.

## Screen map (merchant app)

```
Onboarding (first launch only)
├── 01 Welcome
├── 02 Phone entry
├── 03 OTP verification
├── 04 Business details        (business name)
├── 05 Settlement account       (settlement mobile-money number + operator)
└── 06 Till / short-code        (assign or confirm the till)
       ↓
Set PIN (first run after onboarding)
├── 07 Create PIN
└── 08 Confirm PIN
       ↓
Authenticated merchant app (3-tab bottom nav)
├── Tab: Encaisser (Collect)
│   ├── 09 Home — QR + till + live payments feed   ← default
│   ├── 10 Charge by customer number (fallback)     ← amount + customer number
│   ├── 11a Charge pending
│   ├── 11b Charge settled
│   └── 11c Charge failed (refunded to customer)
├── Tab: Paiements (Payments)
│   ├── 12 Payments list
│   └── 13 Payment detail (receipt view)
└── Tab: Réglages (Settings)
    ├── 14 Settings root
    ├── 15 Settlement account
    ├── 16 Change PIN
    └── 17 Sign out confirm

Modal / overlay (any tab)
├── M1 Operator status banner (home)
├── M2 Offline banner (home)
├── M3 Forgot PIN flow (re-enter phone → OTP → set new PIN)
├── M4 Force-update sheet (when blocked)
└── M5 Show-QR fullscreen (enlarge the payment QR for a customer to scan)
```

Total: **17 primary screens + 5 modals/overlays = 22 surfaces** in v1.

## Onboarding flow (merchant)

### 01 — Welcome
- **Purpose:** introduce the brand in one sentence; nudge toward registering the business.
- **Components:** brand mark (top center); headline ("Encaissez les paiements mobile money, tous réseaux."); single sub-line; primary CTA "Commencer."
- **States:** static.
- **Out:** tap CTA → Screen 02.

### 02 — Phone entry
- **Purpose:** capture the merchant's phone number for OTP.
- **Components:** large numeric input with `+243` prefix locked; helper text ("Nous vous enverrons un code par SMS"); primary CTA "Envoyer le code" (disabled until a 9-digit local part).
- **States:** default, focused, sending (button spinner), error (invalid format), error (SMS provider down).
- **Out:** valid → Screen 03.

### 03 — OTP verification
- **Purpose:** verify control of the phone.
- **Components:** 6-digit OTP entry (auto-paste where supported); 60-second resend timer; "Modifier le numéro" link.
- **States:** entering, verifying, success, error (wrong / expired), resend cooldown.
- **Out:** correct OTP → Screen 04 (first time) or Screen 09 / PIN entry (returning merchant).

### 04 — Business details
- **Purpose:** capture the merchant's business name (the name customers see at the USSD confirm).
- **Components:** "Nom de votre commerce" text input; helper ("Ce nom apparaît au client au moment de payer."); primary CTA "Continuer."
- **States:** entering, saving, error.
- **Out:** → Screen 05.
- **Flag:** **business KYC** (documents) is a flagged item — not collected here in the lightweight v1, tightened later.

### 05 — Settlement account
- **Purpose:** capture where the merchant gets paid — the **settlement mobile-money number** (+ operator).
- **Components:** phone input (`+243` locked) for the settlement number; operator badge auto-detected (with "Modifier l'opérateur" override); explainer ("Chaque paiement est versé instantanément sur ce numéro, déduction faite de notre commission."); primary CTA "Continuer."
- **States:** entering, operator auto-detected, override open, saving, error.
- **Out:** → Screen 06.
- **Critical:** this number is **server-derived/confirmed** and is the settlement target — never client-overridable at payment time.

### 06 — Till / short-code
- **Purpose:** assign or confirm the merchant's **till / short-code** (what the customer references).
- **Components:** the assigned till displayed prominently (e.g. "1001"); the resulting dial string preview ("Vos clients composeront *123*1001#"); primary CTA "Terminer."
- **States:** assigning (loading), assigned, error.
- **Out:** → Set PIN (Screen 07).

### 07 — Create PIN
- **Purpose:** set the merchant-app PIN.
- **Components:** "Créez votre code PIN" headline; 4 dots; custom 4-column keypad; "Pourquoi un PIN ?" inline link → bottom sheet.
- **Validation:** reject sequential (`1234`), repeated (`1111`), common deny-list; inline "Choisissez un code plus difficile à deviner."
- **States:** entering, full (auto-advance), bottom-sheet open.

### 08 — Confirm PIN
- **Purpose:** confirm the freshly-created PIN.
- **Components:** "Confirmez votre code PIN"; 4 dots; same keypad.
- **States:** entering, success (animate → Home), mismatch (shake + "Les codes ne correspondent pas, réessayez").

## Collect flow (Encaisser)

### 09 — Home — QR + till + live payments feed (Collect tab default)
- **Purpose:** the merchant's day-to-day surface — show the code, watch the money land.
- **Components:**
  - Top: greeting + tiny menu icon (top-right; opens M1/M2 if active).
  - **Payment QR** card: the merchant's QR (a `tel:` USSD dial-through, served from `GET /merchants/{id}/qr.svg`) + the **printed dial string** below it ("*123*1001#") so non-scanning / iPhone customers can dial or read it. Tap → **M5** (fullscreen QR to hand to a customer).
  - **Till** chip: the short-code, prominent.
  - Secondary CTA: "Encaisser par numéro" → Screen 10 (the charge-by-number fallback).
  - Section header "Paiements récents" + a **live feed** of incoming payments: each row = amount, customer number, operator badge, time, and a state chip (`en attente` / `réglé` / `échoué`). Empty state: "Vos paiements apparaîtront ici dès le premier encaissement." (no illustration).
- **States:** default, offline (M2), operator-degraded (M1), empty (no payments yet), feed updating (new row animates in).
- **Out:** tap QR → M5; tap "Encaisser par numéro" → 10; tap a feed row → 13.

### 10 — Charge by customer number (fallback)
- **Purpose:** merchant-initiated charge when the customer can't scan/dial (iPhone, no sticker, merchant prefers to drive).
- **Components:**
  - Top: back arrow.
  - Large amount display (`display` token, auto-grows); currency pill (USD default; CDF where enabled).
  - Customer number input (`+243` prefix locked, paste supported); operator badge auto-detected.
  - Helper: "Le client recevra une demande de paiement à approuver sur son téléphone."
  - Bottom: primary CTA "Demander le paiement" (disabled until valid number + amount > 0). Requires a fresh PIN (re-auth) before firing.
- **States:** entering, amount over merchant limit (inline warning), number invalid (inline error), operator-degraded (inline note), submitting.
- **Out:** "Demander le paiement" → 11a.

### 11a — Charge pending
- **Purpose:** show the customer has been asked to approve on their handset.
- **Components:**
  - Centered subtle animated progress.
  - Headline: "Demande envoyée au client."
  - Sub-line: "Le client approuve le paiement sur son téléphone (son opérateur lui demande son code)."
  - Two-leg progress: "Collecte" → "Versement."
  - Footer: "Vous verrez le paiement ici dès qu'il est confirmé."
- **States:** collecting, settling, success (auto → 11b), failure (auto → 11c), timeout (soft-failure + "Réessayer").
- **Out:** terminal → 11b / 11c.

### 11b — Charge settled
- **Purpose:** confirm settlement + show what the merchant netted.
- **Components:**
  - Subtle coral success icon (no confetti).
  - "Encaissé." headline.
  - Receipt card: amount paid by the customer; **net after commission** (amount − MDR); customer number + operator; timestamp; transaction ID.
  - Primary CTA "Partager le reçu" → system share sheet (WhatsApp prominent).
  - Secondary CTA "Nouvel encaissement" → 09.
- **States:** static; share sheet open.

### 11c — Charge failed (refunded to customer)
- **Purpose:** explain a settlement failure and confirm the customer was made whole.
- **Components:**
  - Subtle danger icon (not alarming).
  - "Le paiement n'a pas pu être finalisé." headline.
  - Sub-line: "Aucun montant n'a été retenu — le client a été intégralement remboursé sur son [Vodacom M-Pesa]." (If the refund is still pending: "Le remboursement du client est en cours.")
  - Primary CTA "Réessayer" → 10 with amount + customer number prefilled.
  - Secondary CTA "Retour" → 09.
- **States:** refund pending, refund confirmed (sub-line updates with a checkmark), manual-review (rare: "Nous régularisons ce paiement manuellement.").

## Payments flow

### 12 — Payments list
- **Purpose:** show all payments; find and share a receipt.
- **Components:**
  - Grouped by day ("Aujourd'hui", "Hier", "Lundi 15 juin").
  - Each row: amount + currency + customer number + operator badge + state chip (`réglé` / `en attente` / `échoué`).
  - Pull-to-refresh.
  - Empty state: "Vos paiements apparaîtront ici."
- **States:** loading (skeleton), empty, loaded.
- **Out:** tap row → 13.

### 13 — Payment detail (receipt view)
- **Purpose:** full record; allow share.
- **Components:** receipt card (amount paid; **net after commission**; customer number + operator; timestamp; transaction ID; state); "Partager le reçu" CTA; "Signaler un problème" link (opens WhatsApp with the transaction ID prefilled).
- **States:** static.

## Settings flow (Réglages)

### 14 — Settings root
- **Purpose:** merchant settings hub.
- **Components:** business name + till at top; menu rows: "Compte de versement," "Changer le code PIN," "Aide" (→ WhatsApp Business), "À propos," "Se déconnecter" (danger red). App version at bottom.
- **Not in v1:** language selector (French only), notification settings (defaults), dark-mode toggle (system-controlled).

### 15 — Settlement account
- **Purpose:** view / change the settlement mobile-money number.
- **Components:** current settlement number + operator; "Modifier" → number entry (re-detect operator); **PIN re-auth required** to change. Explainer that future payouts go to the new number.
- **States:** viewing, editing, verifying PIN, saving, error.

### 16 — Change PIN
- **Purpose:** rotate the PIN.
- **Components:** current PIN → new PIN → confirm new PIN (same keypad pattern as 07/08).
- **States:** entering current, verifying, wrong (3 attempts → forgot-PIN flow), entering new, confirming, success toast → 14.

### 17 — Sign out confirm
- **Purpose:** confirm sign-out (requires re-OTP next time).
- **Components:** sheet "Vous devrez vous reconnecter avec un nouveau code par SMS." Two CTAs: "Annuler" (default) / "Se déconnecter" (danger).
- **Out:** sign-out → Screen 01 fresh.

## Modals / overlays

### M1 — Operator status banner
- Top of Screen 09 when an operator's success rate drops. Warning color; "Vodacom M-Pesa rencontre des problèmes en ce moment. Les paiements peuvent échouer." Dismiss with X. On Screen 10, an inline note under the operator badge when the customer's operator is degraded.

### M2 — Offline banner
- Top of every screen when the device is offline. "Pas de connexion Internet." The charge-by-number CTA is disabled while offline. (The **customer's** USSD payment is unaffected — it runs on the GSM channel via the aggregator, independent of the merchant app.)

### M3 — Forgot PIN flow
- Triggered from PIN entry ("Code oublié") or after too many wrong attempts. Reuses Screens 02 + 03 (phone + OTP) → 07 + 08 (new PIN). Backend invalidates the old PIN, writes the new hash.

### M4 — Force-update sheet
- Triggered when the backend signals the app is below the minimum supported version. Full-screen, blocking. "Veuillez mettre à jour pour continuer à encaisser en toute sécurité." Single CTA → App Store / Play Store. Used sparingly.

### M5 — Show-QR fullscreen
- Tapping the home QR enlarges it to fill the screen (max brightness) so a customer can scan it cleanly across a counter. Shows the printed dial string beneath for non-scanning / iPhone customers. Dismiss → back to 09.

---

## Customer USSD text flow (no app, no internet)

This is the **primary customer experience** — pure USSD text over the GSM signalling channel. The customer reaches it by **dialing `*123*1001#`** or **scanning the merchant's QR** (`tel:*123*1001%23`, which offers to dial that same string on Android). A QR/dial that already carries the till **jumps past the first prompt** (the till arrives pre-filled). Steps mirror the built `ussd/` handler exactly:

| Step | Customer sees (USSD) | Customer does |
|---|---|---|
| **Dial** (no till) | `CON Enter merchant till code:` | enters the till, e.g. `1001` |
| **Till resolved** (or pre-filled from QR/dial) | `CON Pay Alpha Gas Station`<br>`Enter amount (USD):` | enters the amount, e.g. `10` |
| **Confirm** | `CON Pay 10.00 USD to Alpha Gas Station?`<br>`1. Confirm  2. Cancel` | presses `1` |
| **Initiated** | `END Payment of 10.00 USD to Alpha Gas Station initiated. Approve on your phone.` | — |
| **Operator PIN prompt** | the **operator's own** PIN prompt arrives on the handset (we never see it) | enters their mobile-money **PIN** to authorise |
| **Confirmation** | the **operator's** confirmation SMS arrives | — |

Unhappy paths (also as built): unknown/inactive till → `END Merchant not found.`; non-numeric/zero amount → `END Invalid amount. Please dial again.`; choosing `2` → `END Cancelled.`; any other choice → `END Invalid choice. Please dial again.`

Notes:
- **USSD copy is currently English in code; production copy is French** (the bearer/menu strings get localised — flag during the bearer integration). The merchant-facing app copy is French.
- **USSD is single-currency (USD) for now** (see `functionality.md` #8).
- **iPhone customers** can't auto-dial `*`/`#` from a QR (iOS blocks it) — they read the printed dial string or the merchant uses **charge-by-number** (Screen 10).

---

## Out of scope for v1 (catalog for later)
- A separate **consumer app** (customer-facing).
- Multi-currency on the USSD channel.
- In-app help articles / FAQ screens; notification-preferences screen; language selector; dark-mode toggle (we follow system).
- Merchant profile photo / logo upload; multi-user / staff accounts under one merchant.
- Saved-customer list / nicknames.

## Open questions (to settle during build or with new team)
- **Custom vs system numeric keypad** for amount entry (Screen 10) vs PIN (07/08/16). Default: system numeric for amount; custom 4-column for PIN. Revisit with mobile engineer.
- **Live-feed transport** — polling vs push vs websocket for the home feed (Screen 09). Default v1: polling + push.
- **Success-state motion** — default minimal. Revisit with designer.
- **Splash / brand** — defer to the brand exploration sprint in Phase 0 Week 1.
- **French USSD menu strings** — finalise during the bearer adapter work.

## Sources
- [product-summary.md](../00-overview/product-summary.md) — merchant-pivot product shape
- [ui-spec.md](./ui-spec.md) — visual system and tone
- [functionality.md](./functionality.md) — flow decisions (USSD primary + charge-by-number fallback; MDR direction)
- [ussd-gateway-providers.md](../02-findings/cross-cutting/ussd-gateway-providers.md) — USSD channel model, QR/dial behaviour
- [ux-patterns.md](../02-findings/app-development/ux-patterns.md) — Wave minimalism, fee-direction, trust signals
- [language-and-locale.md](../02-findings/app-development/language-and-locale.md) — French copy direction
