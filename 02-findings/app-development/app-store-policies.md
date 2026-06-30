# App store policies for African fintech

## Bottom line
Apple's Guideline 5.2.1 — "apps providing services in highly regulated fields like banking and financial services must be submitted by the legal entity that provides the services, not an individual developer" — is the dominant fintech rejection trigger. Google Play's 2025 update to the Financial Services policy expanded disclosure requirements (especially for credit/loan products) and now requires a "Financial features declaration" form. For our DRC MVP, the practical implication is that the *legal entity* of the publisher must be a licensed financial institution or the licensed partner whose service the app fronts, and we need to be prepared to provide regulator licensing evidence at submission.

## Key patterns

### Apple App Store — guideline 5.2.1
- "Apps must be submitted by the legal entity that owns and is responsible for offering any services provided by the app." [verified fact — Apple App Review Guidelines]
- Apple flags fintech (banking, payments, lending, crypto, derivatives) as a "highly regulated" category requiring proof of licensing [verified fact — fintechlawblog.com, natlawreview.com]
- Specific extra scrutiny on: contracts for difference (CFDs), forex, securities trading, crypto exchange — all require "properly licensed in all jurisdictions where the service is available" [verified fact — natlawreview.com]
- Proving licensure across the 170+ Apple-supported jurisdictions can be a major blocker; restricting App Store availability to specific countries via App Store Connect is the common workaround [verified fact — fintechlawblog.com]
- Common-but-related rejection patterns: 3.2.1 Business (you must own/control the service); 1.1.6 (misleading users about financial relationship); 2.1 (incomplete information about how the service works)

### Google Play — Financial Services policy (2025 update)
- All apps providing "financial features" must complete the **Financial features declaration form** in Play Console [verified fact — Google Play Console help, support.google.com/googleplay/android-developer/answer/13849271]
- April 2025 policy update: expanded the Personal Loans policy to include **lines of credit** apps; same disclosure rules apply: repayment terms, max APR, representative cost example, comprehensive privacy policy [verified fact — Google Play policy update, April 10, 2025; deadline May 28, 2025]
- Apps providing financial services are **prohibited from accessing**: photos, contacts, location, SMS (full-permission access) — this was a major change targeting predatory lending [verified fact — Google blog post; asoworld.com summary]
- Google "does not allow apps that expose users to deceptive or harmful financial products" — broadly enforced [verified fact — Google Play Developer Program Policy]
- Compliance deadline tightness: when Google updates the policy, apps that don't comply within ~60-90 days **get removed from the store** [verified fact — asoworld.com]

### Multi-region store listings — French + English minimum for DRC
- App Store Connect and Play Console both support per-locale metadata (title, description, screenshots, keywords)
- DRC's primary store-listing language is French
- English secondary is the common pattern for African apps because the store algorithms (search, recommendations) lean Anglophone
- **M-Pesa DRC** ships with French + English in-app and listings in both [verified — Google Play store description]
- **Orange Money Afrique** ships with multi-country French listings [verified — Play Store, in French]

### Pre-launch checklist for fintech apps
Patterns from the legal and developer-forum literature:
1. **Legal entity**: ensure the publishing entity matches the regulated entity (or has explicit partnership documentation with the regulated entity)
2. **License documentation**: ready to upload regulator letters for each market
3. **Privacy policy URL**: hosted, accessible, in plain language, covers what Google's policy requires
4. **Data Safety section in Play Console**: accurately disclose data collection and sharing
5. **Permissions justification**: be ready to defend each permission with in-app context
6. **Crypto disclosure** (if relevant): both stores require disclosure that the app handles crypto/digital assets, and the entity must be licensed where applicable
7. **Test account credentials**: required for Apple review (give them a way to actually test the wallet)
8. **In-app onboarding**: must work for the reviewer's geographic location (Apple reviewers are in California; if the app blocks non-DRC numbers from signing up, provide a test account)

### Common rejection reasons (Apple, fintech-relevant)
From multiple sources [natlawreview.com, onemobile.ai, fintechlawblog.com]:
1. **5.2.1 Legal Entity** — individual developer attempted to ship a financial app
2. **2.1 Incomplete information** — missing test account, missing documentation
3. **5.1.1 Data Collection** — inadequate privacy policy or data justification
4. **5.1.5 Location Services** — if location is used without clear user benefit
5. **3.1.1 In-App Purchase** — if the app tries to monetize digital content outside of IAP (rare for payments apps but worth knowing)

### Google Play — common rejections
- Missing Financial features declaration
- Personal Loans apps without APR/repayment disclosure (since 2023, hardened in 2025)
- Permission requests deemed "sensitive" (SMS, accessibility) without strong justification
- Missing or incomplete Data Safety section
- Country/region restrictions: if the app is "Financial Services" type, certain countries on the Play list require additional license proof to publish there

## DRC-specific notes
- DRC is a supported country on both App Store and Google Play for distribution.
- DRC is **not** on Google Play's list of high-scrutiny countries for personal loans (that list is heavily anglophone Africa: Kenya, Nigeria, India, etc.), but our MVP is payments, not loans, so this is moot.
- **No DRC-specific Apple/Google policy quirks were surfaced** in research. Standard global rules apply.
- The publishing entity will need a relationship with the licensed entity providing the service:
  - Either pawaPay's license covers our app's flows (unlikely — they're the rails)
  - Or our entity holds a DRC EMI/PSP license directly (flag for legal investigation per CLAUDE.md)
  - Or we partner with a licensed DRC operator and publish via their entity
- Wave Mobile Money's WAEMU e-money license — they publicly market being the **first fintech to operate in multiple WAEMU countries with an EMI license** [verified fact — wave.com blog]. This is the regulatory pattern that lets a fintech publish a financial app under its own brand. WAEMU is not DRC, so the equivalent in DRC is a Banque Centrale du Congo license.

## What this means for our MVP
1. **Establish the publishing entity early**. Apple will reject "indie developer publishes financial app" on Guideline 5.2.1. The entity needs to either hold a financial license or have a contractual relationship with a licensed entity that we can document.
2. **Submit per-country store listings**: French primary, English secondary, both for App Store Connect and Play Console.
3. **Submit Google Play's Financial features declaration** as soon as the entity is established — this is a form, not optional.
4. **Privacy policy URL** must be live before submission, in French (and English) — covers our handling of pawaPay PII pass-through, biometric data, etc.
5. **Restrict App Store/Play availability to DRC initially** — limits cross-jurisdiction licensing risk. Expand to other markets only after explicit due diligence per market.
6. **Test account credentials**: prepare a sandbox phone number and PIN that Apple/Google reviewers in their geographies can use without DRC-SIM gymnastics. This is the most common reason fintech apps get stuck in review.
7. **Permissions hygiene**: request *no* permissions that aren't strictly required. No SMS access (use OTP via Firebase Phone Auth or similar, not SMS-read autofill — Google Play forbids the autofill SMS permission for financial apps).
8. **Plan for resubmission**. Fintech app first-submit rejection rates are high; budget for 2-3 review cycles.

## Open questions
- Is there a **DRC-specific** App Store / Play Store quirk we're missing? Not found in research. May need a DRC-based mobile dev to confirm.
- Does pawaPay's brand surfaced inside our app trigger Apple's "you must own the service" rule? Worth checking how Apple treated apps built on Flutterwave Send or Paystack's checkout SDK historically.
- What's the cycle time on Banque Centrale du Congo licensure for an EMI? Out of scope for this brief but critical for go-to-market.

## Sources used
- natlawreview.com "The Long and Winding Road: Navigating Fintech and Crypto App Approvals by the Apple Store" — secondary, legal
- fintechlawblog.com same article — secondary, legal
- support.google.com/googleplay/android-developer/answer/13849271 (Play Console "Provide information for the Financial features declaration") — primary
- support.google.com/googleplay/android-developer/answer/9876821 (Financial Services policy) — primary
- support.google.com/googleplay/android-developer/answer/16933379 (Developer Program Policy) — primary
- blog.google "Protecting against harmful financial services products" — primary
- asoworld.com April 2025 Google Play policy updates summary — secondary
- onemobile.ai "14 Common Apple App Store Rejections" — secondary
- adalo.com "7 Common App Store Rejection Reasons" — secondary
- developer.apple.com forum threads on 3.2.1 and licensing — primary user reports
- wave.com/en/blog/sn-emi/ (Wave's WAEMU EMI license post) — primary self-reported
- M-Pesa DRC Google Play listing — primary
