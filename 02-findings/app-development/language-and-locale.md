# Language and locale considerations for DRC

## Bottom line
French is the official language of DRC and the primary UI language used by every existing mobile money app in market (M-Pesa DRC, Orange Money) — shipping a French-first MVP with English fallback matches user expectations. Lingala/Swahili/Kikongo/Tshiluba are spoken by tens of millions but are not present in any commercial app's UI surfaced in this research; serving non-French speakers in the MVP is realistic via phone-support and agent-assisted onboarding rather than via translated UI.

## Key patterns

### Language map of DRC
- **Official language**: French [verified fact — Wikipedia "Languages of the Democratic Republic of the Congo"; clearglobal.org]
- **Four national languages**:
  - **Lingala** — lingua franca of Kinshasa and the north/west; ~25M+ speakers
  - **Swahili** (Congolese variant — differs from East African Swahili) — east DRC, Lubumbashi
  - **Kikongo** — west and southwest (Bas-Congo)
  - **Tshiluba** — central/Kasai
- Over 200 languages total in DRC
- **Regional variation matters**: Eastern DRC Swahili is *not* the same as Kenyan/Tanzanian Swahili; word lists and idioms differ [verified fact — bolingoconsult.com, scientifically.blog]

### Language coverage of existing apps in DRC
- **M-Pesa DRC** (com.vodafone.mpesa.drc): **French + English** in-app [verified fact — Google Play store description: "The app supports English and French, ensuring accessibility for all users in the region"]
- **Orange Money Afrique**: French primary; multi-country app supports varying secondary languages per country [verified — Play Store; in French]
- **Wave Mobile Money in Senegal**: French primary (Senegal's official language is also French); Wave's Senegalese marketing is in French
- **None of the major mobile money apps in DRC ship with Lingala or Swahili UI** based on this research. This is a real gap and a possible product differentiator, but it is *not* the convergent pattern.

### Numerals / date formats / phone number display

**Phone numbers in DRC**:
- Country code: **+243**
- Mobile operator prefixes (most common):
  - Vodacom: +243 81/82/83/84
  - Orange: +243 84/85/89 (some overlap with Vodacom)
  - Airtel: +243 97/99
  - Africell: +243 90/91
- Local notation often uses 0 prefix: 081xxxxxxx (vs. +243 81 xxxxxxx)
- Apps must handle both 0-prefixed local and +243-prefixed international entry — M-Pesa DRC and Orange Money do
- Operator-prefix parsing is non-trivial because some prefixes are shared (84 is both Vodacom and Orange historically) — *flagged as approximate; needs verification with DRC telco regulator (ARPTC)*

**Date format**: French convention — DD/MM/YYYY. M-Pesa DRC and Orange Money follow this.

**Numeral format**:
- French convention: space as thousands separator, comma as decimal separator (1 234 567,89)
- Some apps default to international/English (1,234,567.89) — inconsistent across apps surveyed; both formats appear

### Currency display — USD vs CDF
- DRC is heavily **dollarized**. Both USD and CDF circulate widely. USD is preferred for larger purchases; CDF for daily small purchases. [verified fact — multiple sources: PayAtlas, TravelWithHello, World Bank country reports]
- The de-dollarization push in 2013 had partial impact; re-dollarization followed in 2016. **Both currencies remain in active everyday use** [verified fact — privacyshield.gov; travelwithhello.com]
- Exchange rate as of June 2026 search: ~$1 = ~2,300+ CDF [verified — xe.com]
- **M-Pesa DRC defaults to dual-currency**: users can transact in both USD and CDF, and have separate balances [verified fact — Google Play store description]
- This is the convergent pattern: **both currencies must be available**; defaulting to one alienates users

### Cultural and copy patterns
- Formal French address (vous, not tu) is the default in financial apps — matches user expectations of a bank or telco
- Avoid Anglicisms in French copy ("transférer" not "transferer", "compte" not "account") — DRC French follows standard Metropolitan French conventions in writing
- WhatsApp Business voice-note support can serve illiterate/semi-literate users in Lingala/Swahili more effectively than text translation

### Language strategy patterns from neighbors
- **Wave Senegal**: French only, despite Wolof being the lingua franca of Senegal — and Wave is dominant. Argues that a French-only MVP can work in a Francophone African market.
- **Eversend** in East Africa: English-first across multiple countries [verified — eversend.co marketing]. Operating in 14 African countries with multi-currency does not necessarily mean multi-language UI.
- **No major Francophone-Africa mobile money app currently ships with Lingala/Swahili UI as a confirmed feature** — this is a "first mover could win goodwill" opportunity but also "no one has had to do it to win" data point.

## DRC-specific notes
- The 4 national languages are constitutionally recognized but **lack the digital infrastructure** (corpora, automated translation tools) needed for high-quality UI translation [verified fact — bolingoconsult.com, scientifically.blog]. Translating financial terms accurately to Lingala/Swahili requires native-speaker review, not Google Translate.
- Eastern DRC Swahili ≠ Kenyan/Tanzanian Swahili — using a Tanzanian translator would produce subtle but real friction.
- The CDF/USD dual-currency reality means UI must handle large numbers comfortably: 50,000 CDF on screen looks "scary big" compared to $22, even though it's the same value. Display both side-by-side in the pay flow.
- Phone numbers: 9 digits after the country code (+243 8X XXX XXXX). Some apps display with 3-3-3 spacing, others 2-3-4. Pick one and stick.

## What this means for our MVP
1. **Ship French as the primary UI language**. English as fallback. Match M-Pesa DRC's pattern exactly — users know it.
2. **Defer Lingala/Swahili/Kikongo/Tshiluba UI** until post-MVP — not because they don't matter, but because no commercial mobile money app in DRC has had to ship them to compete, and the translation lift is real.
3. **For non-French speakers, lean on**:
   - Toll-free phone support that routes to Lingala (Kinshasa) and Swahili (east) speakers
   - WhatsApp voice notes for support
   - Agent-assisted onboarding (when an agent network exists)
4. **Default to dual USD/CDF display** in the home screen and pay flow. Let the user choose preferred display currency in settings, but always show the *other* currency as secondary text in the pay confirmation.
5. **Phone-number entry**:
   - Accept both 0-prefixed local and +243 international
   - Validate against known DRC operator prefixes; show the detected operator (Vodacom/Orange/Airtel/Africell) as visual feedback
   - Auto-format as the user types
6. **Date format DD/MM/YYYY**. Numeric format: space thousands separator, comma decimal (French convention). Or commit to a single format and avoid mixing.
7. **Formal French copy**. Hire a native-French copywriter or have a Kinshasa-based reviewer. Direct-translated English copy will read as foreign and reduce trust.
8. **Voice support in Lingala and Swahili from launch**. This is a hiring decision and an under-rated trust signal.

## Open questions
- What does pawaPay's customer-facing string set look like in French? If they have French translations for state codes ("Payment pending", "Insufficient funds at operator"), use those verbatim. If not, we own the translation.
- Are there local fintech UX studies on French-only vs multilingual UI uptake in DRC? Not surfaced in this research.
- Operator-prefix table for DRC mobile numbers: needs a primary source (ARPTC) confirmation. The 9-digit format and +243 country code are confirmed; specific operator-prefix mapping is *partly* confirmed and worth a clean primary lookup.

## Sources used
- Wikipedia "Languages of the Democratic Republic of the Congo" — secondary, well-sourced
- clearglobal.org "Language data for DRC" — primary, NGO
- bolingoconsult.com "Understanding Localization in DRC and Cameroon" — secondary, vendor
- scientifically.blog "DRC Congo's Languages" — secondary
- translatorswithoutborders.org four national languages — primary, NGO
- M-Pesa DRC Google Play store description — primary
- PayAtlas DRC country profile (USD/CDF) — secondary
- privacyshield.gov DRC currency overview — primary archival US gov
- travelwithhello.com DRC currency guide — secondary
- xe.com USD/CDF exchange rate — primary
- wave.com Senegal marketing (French) — primary
