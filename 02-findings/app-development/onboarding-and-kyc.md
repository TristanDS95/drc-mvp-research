# Onboarding & KYC UX patterns

## Bottom line
The convergent pattern across successful African fintech apps is a phone-OTP signup that gets users to a *usable* (but limited) account in under 90 seconds, with KYC document upload deferred to a "tier upgrade" flow that unlocks higher transaction limits. PalmPay, OPay, Chipper Cash and Eversend all do tier-gated KYC; DRC's central bank already structures mobile money this way (Tier 1 up to ~$100/day, full KYC needed for higher tiers), so the pattern fits regulation natively.

## Key patterns

### Phase 1: Account creation — under 90 seconds
Every reviewed app follows the same first-launch flow:
1. Phone number entry
2. SMS OTP verification (4–6 digit code)
3. PIN setup (4 digits, double entry to confirm)
4. (Sometimes) name entry — often optional or pulled from KYC later
5. Drop into the home screen with a "verify to unlock more" banner

Confirmed for:
- **PalmPay**: "Download, enter phone number, verify with OTP" → into the app at Tier 1 (₦50,000/day limit) [verified fact — Medium @PalmPay; techeconomy.ng]
- **Chipper Cash**: phone+OTP signup, then "verify to send/cash out" gate. Cannot cash out or send payments until verified. [verified fact — Chipper Cash Help Center]
- **Wave**: phone+OTP signup; secret code via SMS activates account [verified fact — accessgambia.com Wave profile]
- **M-Pesa Kenya app**: secret PIN + OTP-based verification at app setup [verified fact — Safaricom T&Cs / setup guide]
- **M-Pesa DRC**: PIN-based login; OTP at setup [verified fact — Google Play description]

### Phase 2: KYC tier upgrade — deferred and gated to limits
This is the *most consistent* pattern in African fintech, and it maps directly to DRC's regulatory structure.

**PalmPay's tiers (Nigeria — useful as proxy):**
- Tier 1 (phone only): ₦50,000 (~$30) per day
- Tier 2 (BVN verified): ₦200,000 (~$120) per day
- Tier 3 (BVN + ID + utility bill): ₦5,000,000 (~$3,000) per day
[verified fact — multiple sources: PalmPay Medium, techeconomy.ng, businessmetricsng.com]

**Chipper Cash:** "Tier 1 account verification" in Nigeria requires BVN + NIN [verified fact — Chipper Cash Help Center]. Selfie + ID upload powered by Entrust (formerly Onfido) [verified fact — onfido.com Chipper Cash case study].

**Chipper Cash document upload flow:**
1. Take a video selfie (positioning prompts on-screen)
2. Submit ID document
3. Wait up to 2 business days for review
4. Receive SMS with success/failure
[verified fact — Chipper Cash Help Center]

**DRC regulatory tier structure (Banque Centrale du Congo Directive #24, 2011):**
- Tier 1: SIM registration only (MSISDN + MNO-stored info) → up to US$100/day, US$2,500/month, max wallet US$3,000
- Tier 2 (full KYC): higher limits up to US$500/day
- US$3,000 is the absolute cap on mobile money wallet balance
[verified fact — GSMA "Enabling Mobile Money Policies in DRC" 2014, citing the directive. **Flagged**: this is 14 years old and limits may have been revised — check bcc.cd before relying.]

### KYC document vendors used in DRC and Sub-Saharan Africa
- **Smile ID** — pan-African; covers all 54 African countries [company self-reported — usesmileid.com]; supports DRC driving licenses issued by Ministère des Transports et Communications [verified fact — Smile ID DRC country page]; 99.8% facial recognition accuracy on African faces [company self-reported]; recent Mastercard partnership extends to mobile money operators [verified fact — Mastercard press release 2025-09]
- **IdentityPass / Youverify** — Nigeria-strong, broader Africa coverage [secondary — allbusiness.africa comparison]
- **Onfido (now Entrust)** — global vendor used by Chipper Cash [verified fact — onfido.com case study]
- **uqudo** — claims DRC KYC/AML services [company self-reported — uqudo.com]

Smile ID is the most-cited vendor for DRC specifically. For a Francophone-Africa-first MVP, Smile ID + a manual review fallback is the lowest-risk default. **Pricing not surfaced in research** — needs vendor outreach.

### What documents work in DRC?
- DRC has **no national ID system** (was true in 2014; DRC is currently building one — flag for re-verification) [verified fact at time of GSMA report — GSMA/FindevGateway 2014]
- Acceptable IDs in practice: passport, voter's card, driver's license (issued by Ministère des Transports et Communications) [verified fact — Smile ID DRC]
- Some operators rely on SIM registration as the de facto KYC for Tier 1 — this means *the user's mobile number itself* is the lightweight identity proof

### Auth model after onboarding
- 4-digit PIN for every transaction (universal)
- Biometric (fingerprint/face) for app *unlock* only — does not replace PIN at transaction
- iris detection supported on M-Pesa Kenya for supported devices [verified fact — Safaricom T&Cs]

### Wave's onboarding (often praised as "simple")
Public sources do not give a step-by-step screen-by-screen of Wave's onboarding, but the available evidence:
- App download → sign up → phone number → SMS code → PIN → into the app
- Agent-assisted onboarding for users who need help (a key adoption channel) [verified fact — TriplePundit; promptloop directory]
- QR card option for users without smartphones (card issued by agent) [verified fact — accessgambia.com]
- "Famous simplicity" claim is widely repeated but not deeply unpacked in any single source — flagged as "company self-reported / popular wisdom"

## DRC-specific notes
- The 2-tier system (light + heavy KYC) is *already mandated* by Banque Centrale du Congo, so building a tiered onboarding isn't a UX choice — it's a regulatory requirement. The UX decision is how light Tier 1 can be and how smooth the Tier 2 upgrade flow is.
- Without a national ID, KYC document choices in DRC are narrower than Nigeria's BVN/NIN. Passport coverage is low (most DRC citizens do not hold passports). Voter's card and driver's license are the realistic document anchors.
- Smile ID supports DRC driver's licenses — confirmed.
- Lingala / Swahili / Kikongo / Tshiluba speakers may struggle with French-only KYC document instructions — leading agent-assisted onboarding (Wave model) is probably essential for non-French speakers even if the app itself is French-only.

## What this means for our MVP
1. **Get users into the app in ≤90 seconds at Tier 1.** Phone + OTP + PIN, then home screen. No name, no email, no document. Match PalmPay's onboarding speed.
2. **Tier 1 limits**: align with DRC's Banque Centrale ceiling — US$100/day, US$3,000 max wallet. This is what regulation allows without full KYC. Surface this limit prominently.
3. **Tier 2 upgrade flow** (called "unlock higher limits" or similar):
   - Selfie video (Chipper/Entrust-style positioning prompts)
   - One ID document: voter's card or driver's license (Smile ID supports both)
   - Address (typed, no utility bill upload for v1 — verify regulator allows)
   - Auto-verify via Smile ID where possible, manual review fallback
   - Target: <24 hour turnaround, ideally near-real-time for happy path
4. **Vendor**: Smile ID is the strongest DRC default. Onfido/Entrust is the global alternative if pricing/contracting is easier.
5. **PIN model**: 4-digit PIN required for every send/pay. Biometric only for *opening* the app. Don't get clever; users know this pattern.
6. **Agent-assisted onboarding**: even if we're rented-rails and have no agent network of our own, the *option* to "ask a friend to help" should be designed in. Wave's agent network is a key adoption asset; we won't have it, but we should not block onboarding for users who need help.
7. **Verify DRC's current tier limits with Banque Centrale du Congo** before lock — the 2014 GSMA report is the cleanest source we have but is 14 years old.

## Open questions
- Are the 2011 BCC mobile-money limits still in force? Flag for primary-source check at bcc.cd or via legal review.
- What is Smile ID's pricing for DRC document verification? Needs vendor RFP.
- Does DRC's emerging digital ID program produce a usable API for KYC? Material for medium-term but unlikely available for MVP.
- Can we run KYC fully on-device (e.g., NFC read of passport MRZ) for users with passports, to avoid round-tripping ID images? Out of scope for v1 but worth flagging.

## Sources used
- PalmPay Medium "How to Link Your NIN/BVN" — primary, self-reported
- techeconomy.ng PalmPay tier limits — secondary
- businessmetricsng.com PalmPay walk-through — secondary
- Chipper Cash Help Center articles on verification (Nigeria, Ghana, document types) — primary
- onfido.com Chipper Cash case study — primary (vendor self-reported)
- usesmileid.com DRC country page — primary, vendor self-reported
- mastercard.com press release on Smile ID partnership Sept 2025 — primary
- allbusiness.africa Smile ID/Youverify/Dojah comparison — secondary
- uqudo.com DRC KYC services page — primary, vendor self-reported
- GSMA "Enabling Mobile Money Policies in DRC" 2014 PDF — primary (regulatory)
- Safaricom My M-PESA T&Cs (iris, biometric) — primary
- Wave.com — primary self-reported
- TriplePundit Wave coverage 2025 — secondary
- accessgambia.com Wave profile — secondary
