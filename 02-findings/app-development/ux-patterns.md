# UX patterns from successful African fintech apps (incl. fees + trust signals)

## Bottom line
Successful African fintech apps converge on a near-identical home screen pattern (balance + 4-6 quick actions + recent transactions) and a 3-step "amount → confirm with PIN → SMS receipt" pay flow. The biggest differentiators are fee transparency (Wave's 1% flat fee is the single most cited UX innovation in Francophone Africa) and brand-level trust signals (regulator licensing badge, agent network visibility, consistent green/blue/teal palettes).

## Key patterns

### Home screen pattern (verified across M-Pesa Kenya, M-Pesa DRC, Wave, OPay, Orange Money)
Home screens in nearly every reviewed app are organized as:
1. Top: account balance with a hide/show "eye" icon (M-Pesa Kenya explicitly added this feature in 2021; M-Pesa DRC has the same).
2. Middle row: 4-6 large icon tiles for the most-used actions — Send, Pay (merchant/bill), Withdraw, Buy Airtime, plus 1-2 country-specific (e.g., Fuliza for M-Pesa, TV subscriptions for M-Pesa DRC, Salary Advance for M-Pesa DRC).
3. Below tiles: scrollable list of recent transactions / statement.
4. Bottom: 4-5 tab navigation (Home / Transact / My Spend / Grow / More in M-Pesa Kenya). [verified fact — Safaricom M-PESA App Manual; The Kenyan Wallstreet]

M-Pesa Kenya's newer "My OneApp" added AI-driven personalization: frequently used features auto-surface on the home screen [company self-reported — Safaricom / The-Star, 2026-04-01]. This is leading-edge and probably *not* table stakes for an MVP.

### Pay flow (3-step convergent pattern)
M-Pesa STK Push and the M-Pesa Kenya app both use the same 3-step structure:
1. Enter recipient (number / paybill / merchant code) + amount.
2. Confirm details on a "Hakikisha" (Swahili: "make sure") preview screen showing recipient name, amount, and fee.
3. Enter PIN; receive SMS confirmation. [verified fact — mpesa.or.ke Hakikisha docs; Safaricom M-PESA App Manual]

The "preview with recipient name" step is universal and is the single most important anti-fraud UX pattern. PalmPay, OPay, Wave all do it; sources explicitly call it out for M-Pesa as "Hakikisha".

Wave's pay flow uses QR codes more aggressively than the others: agents and merchants display a QR; payer scans, confirms amount, enters PIN [verified fact — TriplePundit; AccessGambia profile of Wave]. App-to-app pay can be by phone number or QR.

### Receipt UX
- M-Pesa: SMS to both payer and payee on every transaction, plus in-app "mini statement" list. Receipts include transaction reference, balance after, and fee paid. [verified fact — Safaricom docs]
- Wave: SMS + in-app history. Notably, the Orange Money app received user praise specifically for *retaining* receipts inside the app, vs. other Mobile Money apps where receipts vanish [user reviews summarized in App Store / Google Play search results].
- Implication: SMS receipt is non-negotiable; in-app persistent transaction history is a trust differentiator.

### Auth model (universal pattern)
Phone number + OTP for signup; 4-digit PIN for every transaction; biometric (fingerprint/face) for app *unlock* (replaces PIN at login but PIN still required to transact). Verified for M-Pesa Kenya [Safaricom docs] and M-Pesa DRC [app store description]. PalmPay onboards on phone+OTP, requires PIN for transactions, BVN/NIN for higher tiers [PalmPay Medium].

### Identity disclosure on payment
M-Pesa's "Hakikisha" surfaces the *registered name* of the recipient before PIN entry — this requires a name-lookup service. M-Pesa DRC's "send money to contacts directly from phone book" confirms the app reads contacts and resolves to registered Vodacom/M-Pesa users [Google Play store description for com.vodafone.mpesa.drc].

### Fee display UX (topic 7)
- **Wave (Senegal/Côte d'Ivoire)**: 1% flat fee on send, free deposits/withdrawals at agents, free bill payment. Wave's marketing leads with the fee number on every screen of the website and store listing [verified fact — wave.com; TriplePundit, 2025]. Maximum fee capped at 5,000 F CFA per transaction [theafricareport.com].
- **Orange Money Senegal**: 0.8% sending fee for certain transfers [Google Play description summarized in search results — labeled as company self-reported]. Multi-tier fee table not surfaced in the home flow.
- **M-Pesa**: Fee shown on Hakikisha preview before PIN entry. Fee is *added on top* of the amount being sent (not deducted from the send amount). [verified fact — mpesa.or.ke]
- **General pattern**: Pre-payment fee preview is standard. The differentiator is *flatness* and *predictability* — Wave's 1% flat is famously easy to mentally calculate; M-Pesa/Orange use stepped fee tables that users find opaque.

### Trust signals & branding (topic 8)
- **Color**: Blue and green dominate. Wave's distinctive teal/electric-blue is one of its strongest brand assets and gets named in every review. OPay green, M-Pesa green (across markets), Orange Money's literal orange, Kuda purple. PalmPay uses a purple-blue. [observation across store listings and product pages]
- **Trust patterns mentioned in fintech-branding literature**: regulator license badge ("licensed by Central Bank of Nigeria" on PalmPay; Wave touts WAEMU e-money license as first multi-country fintech licensee [wave.com blog]); customer count ("4 million users" Chipper, "35 million users" PalmPay self-reported); large physical agent network is itself a trust signal in Senegal — Wave employs ~2,000 worldwide, 900 in Senegal [Wave self-reported, TriplePundit].
- **What doesn't appear in successful apps**: heavy animations, splash screens longer than 2 seconds, aggressive promo carousels. Yellow Card UX review explicitly noted app "loads content very quickly with no slowdown" as a positive [medium.com Userhub Africa].

## DRC-specific notes
- **M-Pesa DRC** (com.vodafone.mpesa.drc): supports English + French; dual-currency display USD and CDF; pay to FINCA, EQUITY, Ecobank, Rawbank, UBA, AccessBank; pay Canal+, Startimes, DSTV, EasyTV, Blueusat; salary advance; lay-buy. Users in reviews praise "well-conceived, easy to use" but ask for unified Vodacom features (bundle purchases, balance check) — this suggests M-Pesa DRC is a *standalone* app, not a super-app, which is a possible MVP angle. [Google Play store description]
- DRC has no national identification system [verified fact — GSMA/FindevGateway 2014, but still relevant; flagged as time-sensitive — DRC is currently building digital ID]. Mobile money KYC uses MSISDN (SIM registration) + a tiered limit system: Tier 1 up to ~US$100/day, max wallet US$3,000, US$2,500 monthly. [verified fact — GSMA "Enabling Mobile Money Policies in DRC"; sourced from Banque Centrale du Congo Directive #24, 2011 — confirm whether limits have been updated in last 14 years before relying]
- Strong USD/CDF dollarization means apps *must* let users hold and pay in both currencies. M-Pesa DRC does. [verified fact — Google Play store description; PayAtlas DRC profile]

## What this means for our MVP
1. **Copy the home-screen pattern**: balance with eye-icon, 4-6 quick actions, transaction list. Do not try to be original here — users have a strong mental model.
2. **Adopt the 3-step pay flow with a Hakikisha-style preview** that shows resolved recipient name (look up via pawaPay or operator name resolution) before PIN entry. This is the single highest-leverage anti-fraud UX move.
3. **Show fee in the preview, not after**. If pawaPay's pricing allows a flat or near-flat margin, advertise it Wave-style ("X% all-in, max Y CDF"). Predictable fees beat slightly-lower-but-stepped fees in this market.
4. **SMS receipt + persistent in-app history are non-negotiable**. The in-app history is what makes Orange Money look better than other operators in user reviews.
5. **PIN-on-every-transaction**, biometric only for app unlock. This matches user mental models and the M-Pesa security pattern.
6. **Default to dual USD/CDF display**. Picking one will alienate half the market.
7. **Trust signals to display**: pawaPay name (recognizable), Central Bank licensing status when achieved, partner operator logos (Vodacom, Orange, Airtel) on the pay flow's first screen.

## Open questions
- Does pawaPay support recipient-name lookup across operators (the "Hakikisha" pattern)? If not, what does it return on initiate? — for operator-feasibility research, not here.
- Do DRC-specific KYC tier limits from 2011 still apply? Worth a primary-source check at bcc.cd.
- Is there a successful Francophone Africa MVP that shipped without a Lingala interface? Wave Senegal apparently runs in French only (not verified directly) — and is the dominant player. This may mean a French-only MVP is fine for DRC v1.

## Sources used
- M-Pesa DRC Google Play (com.vodafone.mpesa.drc) — primary
- Safaricom M-PESA App Manual (PDF) — primary
- mpesa.or.ke Hakikisha guide — primary
- Wave.com homepage and blog (WAEMU e-money license post) — primary self-reported
- TriplePundit "Zero-Fee Mobile Banking" 2025 — secondary, well-sourced
- Quartz "How Wave rose to become Francophone Africa's first unicorn" — secondary
- Rest of World "How Wave is using Amazon's playbook" 2022 — secondary
- TheAfricaReport.com on Wave fee cap — secondary
- PalmPay Medium "How to link NIN/BVN" — primary self-reported
- Userhub Africa Medium UX review of Yellow Card — secondary
- Safaricom "Hide Balance" coverage (Techweez, Nairobi Times) — secondary
- The Kenyan Wallstreet on M-Pesa app redesign — secondary
- Moses Kemibaro on M-PESA "My OneApp" launch 2026 — secondary
- GSMA "Enabling Mobile Money Policies in DRC" PDF — primary (regulatory)
- PayAtlas DRC country profile — secondary
