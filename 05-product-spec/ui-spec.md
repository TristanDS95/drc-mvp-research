# UI / UX spec (v1 MVP)

The design-system foundation and core interaction patterns for the **merchant app**. Pixel-level decisions and per-screen detail live in `screens.md`; this file is the rules everything else inherits from.

- **Last updated:** 2026-06-11
- **Status:** foundation set (design system unchanged by the merchant pivot; nav/flow reframed)

> **Direction (2026-06-11): merchant-facing.** The app user is the **merchant**. The **design system below (colors, type, spacing, components, motion) is unchanged** by the pivot and is reused as-is. What changed is the **navigation and flow**: a merchant-acquiring app (Collect / Payments / Settings) rather than a consumer send-app. The customer never opens an app — they pay over USSD (see the customer USSD text flow in `screens.md`).

> **Subject to revision when team grows.** This spec reflects current decisions with the existing 4-person team. When we onboard two more members (especially the designer), their preferences on specific tokens, motion, and component shapes may shift individual choices. The visual direction (Wave-style minimal + coral) and structural rules (3-tab nav, soft flat components, follow-system dark mode) are the durable parts; exact pixel values, font fallback chains, and microinteractions are most likely to evolve.

## Visual direction in one line
**Wave-style minimal structure** (one primary action per screen, generous whitespace, single-purpose feel) **with a distinctive coral accent** (#FF6B5B) — eye-catching and human, not banking-cold.

## Design system foundations

### Color palette

| Token | Light | Dark | Use |
|---|---|---|---|
| `accent` | `#FF6B5B` (coral) | `#FF7E70` (slightly lifted coral) | Primary CTA, key text, brand mark |
| `accent-soft` | `#FFE4E0` | `#3A1F1B` | Accent backgrounds, badges, soft fills |
| `bg` | `#FFFCF8` (cream) | `#121214` (near-black) | App background |
| `surface` | `#FFFFFF` | `#1C1C20` | Cards, sheets, modals |
| `text` | `#1A1A1A` (charcoal) | `#F4F4F0` (cream) | Body text |
| `text-soft` | `#5B5B5B` | `#A3A3A0` | Secondary text, captions |
| `border` | `#E8E5DD` | `#2E2E32` | Subtle dividers |
| `success` | `#1B7F3A` (deep green) | `#3CBE5F` | Confirmed transactions |
| `danger` | `#B42318` (deep red) | `#F97066` | Failures, refunds, warnings |
| `warning` | `#B45309` (amber) | `#FDB022` | Operator outages, status banners |

> _Why coral:_ Differentiates from every DRC operator (Vodacom red, Orange orange, Airtel red, Africell purple/green) and every major African fintech (Wave teal, M-Pesa green, MTN yellow, OPay green). Warm without being childish; eye-catching without being garish. Pairs well with cream and charcoal.

### Typography

**Family:** Modern geometric sans — **Inter** as the primary recommendation (variable-font, excellent screen rendering, free OSS license, comprehensive Latin coverage including French diacritics, broad fallback support). Backup options: Plus Jakarta Sans, Manrope.

| Token | Size | Weight | Line height | Use |
|---|---|---|---|---|
| `display` | 48–64 | 600 | 1.1 | Transaction amounts, key confirmations |
| `h1` | 28 | 600 | 1.25 | Screen titles |
| `h2` | 22 | 600 | 1.3 | Section headers |
| `body-l` | 17 | 400 | 1.5 | Default body text |
| `body` | 15 | 400 | 1.5 | Secondary body |
| `caption` | 13 | 400 | 1.4 | Helper text, timestamps |
| `caption-bold` | 13 | 600 | 1.4 | Labels, badges |
| `button` | 17 | 600 | 1 | CTAs |

Numerals: tabular figures for amounts (so they don't jitter across lines).

### Spacing scale

`4 / 8 / 12 / 16 / 24 / 32 / 48 / 64` — only these values, no in-between. Tight enough to be consistent; expressive enough.

### Component style

- **Rounding:** 12px for cards, sheets, modals. 12px for buttons. 8px for inline pills, badges, and inputs.
- **Shadows:** **None.** Pure flat single-layer design — cleaner rendering on low-end Transsion phones that dominate DRC, lower battery draw, sharper screenshots in WhatsApp shares.
- **Borders:** 1px `border` token on interactive elements; subtle, present.
- **Buttons:** filled `accent` for primary; outlined `accent` for secondary; ghost (text-only) for tertiary. Min height 48px (Android touch target).
- **Inputs:** flat backgrounds with `border` underline focus state. No floating labels in v1 — labels above inputs (lower cognitive load, better French copy fit).

### Dark mode

Follow system. Both themes shipped v1 with token parity. Test on a Tecno or Infinix AMOLED panel — these dominate DRC per [network-and-devices.md](../02-findings/app-development/network-and-devices.md).

## Layout patterns

### Home screen (merchant — "Encaisser")
- Top: small greeting + tiny menu icon (top-right).
- Center of attention: the **payment QR** card — the merchant's QR (a `tel:` USSD dial-through) with the **printed dial string** ("*123*1001#") beneath it, plus the **till** chip. Tap → fullscreen QR (max brightness) to present to a customer.
- Secondary CTA: **"Encaisser par numéro"** (`accent` outlined) — the merchant-initiated charge-by-customer-number fallback.
- Below: **"Paiements récents"** — a live feed of incoming payments as compact rows (amount, customer number, operator badge, time, state chip).
- Empty state: a single-line message ("Vos paiements apparaîtront ici dès le premier encaissement.") with no illustration.

> The merchant's home is a **display-and-monitor** surface ("show the code, watch the money land"), not a send form. The primary money-in action is the **customer's** USSD scan/dial; the in-app CTA is the **fallback** charge-by-number.

### Bottom navigation
Three tabs, equal weight:
- **Encaisser** (Collect) — storefront / QR icon — default tab
- **Paiements** (Payments) — list icon
- **Réglages** (Settings) — gear icon

Active tab indicated by `accent` color on icon + label.

### Charge-by-number flow (fallback, 3 screens)
1. **Amount + customer number** — large numeric display (`display` token), currency pill (USD default; CDF where enabled), customer-number input with auto-detected operator badge. Single primary CTA "Demander le paiement" (requires PIN re-auth).
2. **Pending** — "Demande envoyée au client." Two-leg progress ("Collecte" → "Versement"); the customer approves on their own handset (operator PIN prompt).
3. **Result** — full-screen named outcome (settled / failed-refunded) with appropriate `success` / `danger` color, showing the **net after commission** on success, and a clear next action.

### Payments
Grouped by day. State chip per row (réglé / en attente / échoué). Tap → payment detail (amount, **net after commission**, customer number, operator, time, transaction ID) with WhatsApp share.

### Settings
Minimal: settlement account, change PIN, sign out. App version at the bottom.

### Customer USSD flow (not an app surface)
The customer's experience is **USSD text**, not screens — dial/scan → amount → confirm → operator PIN prompt. Specced as a text flow in `screens.md`. Production USSD strings are French (English in code today).

## Tone of voice

- **Functional + warm.** Most copy is direct: "Encaissé. Reçu confirmé." Occasional warmth on key moments: "Bienvenue. Mettons votre commerce en route."
- **Speak to the merchant** — the app user is the business owner. The customer is referred to in the third person ("le client approuve sur son téléphone").
- **Never blame anyone** — failure copy says "le paiement n'a pas pu être finalisé — le client a été remboursé," not "le paiement a échoué."
- **Be explicit about the fee direction** — the customer pays the sticker price; our commission (MDR) comes out of the **merchant's** settlement. Surface the merchant's **net** ("vous recevez ... après commission"), never a fee added to the customer.
- **Action-oriented buttons** — verbs, not nouns. "Encaisser," "Demander le paiement," "Réessayer."
- **No exclamation marks in transactional copy.** Save them for onboarding.

## Microinteractions / motion

- **Default transitions:** 200ms ease-out, no fancy choreography.
- **Success haptic** (light-medium) on completed send.
- **Failure haptic** (medium-heavy double-tap) on error states.
- **Skeleton loaders** for history (cheaper than spinners on slow networks).
- **No splash animation** beyond the system splash. Cold start should reach the auth screen in under 2 seconds on a mid-range Android.

## Accessibility floor

- Minimum contrast ratio: WCAG AA (4.5:1 for body text, 3:1 for large text).
- Minimum touch target: 48×48 dp (Android), 44×44 pt (iOS).
- Screen-reader labels on every interactive element.
- Numeric inputs always trigger the numeric keyboard.
- Dynamic type: respect system text-size setting up to 130%.

## Asset & token strategy

- All visual tokens (colors, type, spacing) defined in a single TypeScript module + Figma library — single source of truth for engineering and design.
- Icon library: Phosphor Icons (regular weight) — generous coverage, consistent stroke, free OSS license. Custom marks (brand, operator badges) authored separately.

## What this DOESN'T specify (intentional)

- Exact logo / wordmark — needs a brand exploration sprint in Phase 0 Week 1.
- App icon — same.
- Splash screen visual — same.
- Onboarding illustrations (if any) — keep minimal; explore in Phase 0 Week 3.
- Empty-state illustrations — start with text-only, add illustrations only if testing shows confusion.

## Open questions

- **Brand name + wordmark.** Placeholder used throughout. Settle in Phase 0 Week 1.
- **Custom numeric keypad vs system numeric keypad** for amount/PIN entry. Custom is more on-brand but more code; system is faster to ship and accessible by default. Default for v1: system numeric for amount; custom 4-button grid for PIN (faster than full keypad).
- **Coral on iOS vs Android.** Render the exact coral hex on at least one real Tecno/Infinix and at least one iPhone before committing — perceived warmth can shift across panel calibrations.

## Sources
- [ux-patterns.md](../02-findings/app-development/ux-patterns.md) — Wave's minimalism, trust signals, fee-direction patterns
- [language-and-locale.md](../02-findings/app-development/language-and-locale.md) — French primary, DRC locale norms
- [network-and-devices.md](../02-findings/app-development/network-and-devices.md) — Transsion dominance, AMOLED considerations
