# Tech stack patterns in African fintech apps

## Bottom line
There is no consensus mobile framework among successful African fintech apps: Wave (the closest analogue to our MVP) uses native Kotlin Android + Swift iOS with Python/GraphQL backend on GCP; OPay uses native Java/Kotlin + Swift/Objective-C on Huawei Cloud; Kuda built a custom in-house core banking system; Flutter and React Native are popular in the Nigerian fintech *developer ecosystem* but rarely confirmed in shipped flagship apps. For an MVP with a small team, cross-platform (Flutter or React Native) is defensible — but every well-sourced flagship app this research found is native.

## Key patterns

### Mobile framework: native dominates among the well-sourced flagships
| App | Mobile stack | Confidence | Source |
|---|---|---|---|
| Wave Mobile Money | Kotlin/Jetpack Android, Swift/SwiftUI iOS | High | wave.engineering/about/ (their own site) |
| OPay | Java/Kotlin Android, Swift/Objective-C iOS | High | Crunchbase tech profile; Huawei Cloud OPay case study |
| M-Pesa Kenya | Not publicly stated; native is the long-held assumption | Low — not directly confirmed | Safaricom does not publish stack |
| M-Pesa DRC | Not publicly stated | Not found | — |
| PalmPay | Likely native (article references PalmPay/OPay as "native with deep OS integration"); also confirmed Flutter SDK exists for *third-party* integration | Medium — inferred from secondary source | nimbleappgenie, dev.to comparisons |
| Kuda | Mobile stack not directly published; HTEC was brought in for backend work | Low | TechCabal, Kuda blog, HTEC case study |
| Flutterwave (Send / former Barter) | Offers both Flutter and React Native SDKs *for integrators*; in-house mobile stack not confirmed | Not found for the consumer app | flutterwave/React-Native GitHub |

**The "Flutter or React Native?" question is largely answered by the data: not in the flagships.** Where Flutter/React Native show up confirmed, it's almost always for *SDKs the company offers third parties* — OPay's React Native SDK, Flutterwave's React Native SDK, Kuda's open API. The biggest flagship that does cross-platform that we could confirm at all is unclear; Cash App (not Africa) uses Kotlin Multiplatform; Wolt's merchant app uses Flutter but is European.

### Backend & infrastructure
Wave's published stack is the most decision-relevant data point we have, since Wave is the closest commercial analogue:
- Backend: Python 3 + mypy
- API: GraphQL
- Database: PostgreSQL
- Infrastructure: GCP + Terraform
- Orchestration: Kubernetes
- Internal tools: JavaScript / React / Relay
[verified fact — wave.engineering/about/]

OPay:
- Huawei Cloud (cloud-native database, big data services, decoupled storage/compute)
- Firebase Cloud Messaging + AWS SNS for push
- ML/AI for fraud detection
[verified fact — Huawei Cloud case study published with OPay's name; Crunchbase]

Kuda:
- Built "Nerve" — a custom in-house core banking system, replacing third-party software they used from August 2019 [verified fact — joinkuda.medium.com; TechCabal]
- HTEC engaged for backend expansions [verified fact — htec.com case study]

### Hosting region
- **AWS Cape Town** launched April 2020, three AZs — the only hyperscaler AZ on the continent for years. POPIA-compliant data residency. [verified fact — aws.amazon.com/blogs/aws/now-open-aws-africa-cape-town-region/]
- **GCP Johannesburg** launched January 31, 2024. [verified fact — DCD coverage]
- **DRC-specific**: no in-country hyperscaler. DRC announced an €8B digital infrastructure plan for 2026–2030 that includes data centers, but it is years out. [verified fact — Tech In Africa coverage]
- Wave runs on GCP — given GCP Johannesburg's 2024 opening, this is a now-feasible same-continent option. Pre-2024, Wave was likely served from EU regions, which is high latency to West Africa but workable for low-volume async payments.

### Auth providers / SMS-OTP
SMS providers in Africa:
- **Africa's Talking** — has DRC coverage; pricing per market not surfaced publicly; offers local-currency billing in some markets [verified fact — africastalking.com pricing page]
- **Termii** — Nigeria-strong; can discuss local currency billing including CFA franc on contract [company self-reported — termii.com pricing]. CFA pricing relevant for WAEMU/CEMAC; DRC uses CDF, not CFA, so this may not be a strong fit.
- **Twilio** — works globally; DRC pricing is on their per-country calculator (not retrieved here).
- **HelloDuty** — claims DRC USSD/SMS/WhatsApp coverage [company self-reported — helloduty.com]
- Termii's *non-DRC* price ($0.0107/msg in Nigeria) is a usable order-of-magnitude anchor; expect DRC to be in the $0.02–0.05 range based on typical Sub-Saharan pricing — flagged as inference, not verified.

Authentication patterns are universal: phone number + SMS OTP for signup; PIN for transactions; biometric (fingerprint/face) for app unlock as an *optional* additive layer on top of PIN. M-Pesa Kenya supports iris detection on supported devices [verified fact — Safaricom My M-PESA T&Cs].

### Mobile push
FCM is universal on Android. iOS uses APNs (Apple Push Notification Service). OPay layers AWS SNS on top of FCM for orchestration [Huawei Cloud OPay case study].

## DRC-specific notes
- No DRC hyperscaler region. Latency from EU (Paris/Frankfurt) to Kinshasa is ~100-150ms; from GCP Johannesburg ~70-90ms; from AWS Cape Town ~80-100ms. Not formally measured here; flagged as inference based on geography.
- AWS Cape Town opened *after* Wave was founded — Wave's choice of GCP may reflect when they made the decision, not what's optimal today.
- Africa's Talking explicitly markets DRC; Termii does not (Nigeria-focused). Africa's Talking is the safer default for DRC SMS OTP.

## What this means for our MVP
1. **Stack proposal for a small team**: React Native or Flutter is defensible for an MVP given the rarity of mobile engineers familiar with both Android *and* iOS, even if no successful African flagship uses these as confirmed. If hiring is the constraint, this beats native. If maintainability and access to platform features is the constraint, native is better. Wave's choice of native after launching from app-only suggests they did *not* find cross-platform sufficient as they scaled.
2. **Backend default**: Python + Postgres + a hyperscaler is the lowest-risk path. Wave validates exactly this. GraphQL is optional; REST is fine for an MVP.
3. **Hosting**: GCP Johannesburg or AWS Cape Town — closest hyperscaler regions to DRC. Probably GCP because Wave is on GCP and there is a public engineering team to learn from.
4. **SMS provider**: Africa's Talking first; confirm DRC pricing during procurement. Have Twilio as fallback for international remit traffic if relevant.
5. **Auth model**: phone+OTP signup, PIN for transactions, biometric for app unlock. Match the M-Pesa pattern exactly — users already know it.
6. **Push**: FCM on Android, APNs on iOS. Skip SNS layer for the MVP.

## Open questions
- What is Africa's Talking's *actual* per-message DRC SMS price? Worth asking sales — the public page does not list it.
- Does pawaPay's SDK ship with a recommended mobile framework? Worth checking before locking the framework choice.
- Is Wave actually native iOS? Their about page lists Swift/SwiftUI — confirmed yes, but the Senegal app launched Android-first; iOS may have lagged.

## Sources used
- wave.engineering/about/ (Wave's own stack disclosure) — primary
- Huawei Cloud OPay case study — primary, company self-reported
- Crunchbase OPay tech profile — secondary
- TechCabal "How Kuda built Nerve" 2026-03-03 — secondary
- joinkuda.medium.com "Story Behind Nerve" — primary self-reported
- HTEC Kuda case study — primary partner-reported
- aws.amazon.com/blogs/aws/now-open-aws-africa-cape-town-region/ — primary
- DCD news on GCP Johannesburg launch Jan 2024 — secondary
- Tech In Africa coverage of DRC €8B digital plan — secondary
- africastalking.com pricing — primary
- termii.com pricing — primary
- HelloDuty DRC services page — primary self-reported
- Safaricom My M-PESA T&Cs (biometrics, iris) — primary
- nimbleappgenie / dev.to comparison articles (PalmPay native claim) — secondary, weak
- flutterwave/React-Native GitHub repo — primary
