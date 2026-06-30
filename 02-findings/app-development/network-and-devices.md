# Network and device realities in DRC

## Bottom line
DRC has 60.3 million mobile connections (54% population penetration) with 75% on 3G/4G/5G, but only 30.6% of individuals use the internet, and a typical user spends ~31% of income on mobile services — so data is the dominant cost barrier. Transsion-brand low-end Android (Tecno/Infinix/Itel) dominates devices in DRC and across Sub-Saharan Africa, meaning the MVP must run well on 2–4 GB RAM phones with intermittent 3G and aggressive data caps.

## Key patterns

### DRC mobile market — verified facts
- 60.3 million active cellular connections at start of 2025 (54.3% of population) [verified fact — DataReportal Digital 2025 DRC]
- 34.0 million individuals using the internet (30.6% penetration) [verified fact — DataReportal]
- 75.1% of mobile connections are "broadband" (3G/4G/5G) [verified fact — DataReportal]
- Average mobile data price ~$0.88/GB — among Africa's cheapest [verified fact — ts2.tech "No Signal" article, citing benchmark data]
- BUT a monthly mobile budget of US$17.50 (140 minutes, 20 SMS, 5GB) is calculated for DRC and a typical user spends 31.31% of income on mobile telephony [verified fact — worlddata.info DRC telecom page]
- Median daily income around $2.15; basic Android phones cost $40+ [verified fact — ts2.tech, cross-referenced with World Bank ranges]

These two facts in tension — cheap per-GB but high share of income — mean DRC users *will* care about every megabyte the app uses.

### Device landscape — Sub-Saharan Africa wide
- Transsion (Tecno + Infinix + Itel) holds 46–52% of African smartphone market share in Q1 2025 [verified fact — multiple sources: Intelpoint, TheAfricaReport, EqualOcean]
- In Nigeria (Feb 2025): Tecno 23.55%, Infinix 21.73%, Itel 5.41% = 50.69% [verified fact — Intelpoint]
- Samsung ~21%, Xiaomi ~13%, others smaller [verified fact — Intelpoint]
- iOS share in Sub-Saharan Africa is approximately 5% [labeled as user's brief; widely cited but not freshly verified here — flag for confirmation, e.g., StatCounter as a primary source]
- Tecno/Infinix/Itel design specifically for African needs: extended battery life, multi-SIM (typically dual-SIM), camera tuning for darker skin tones [verified fact — EqualOcean, African Business]

### Implications of device profile
- Low-end Tecno/Infinix devices commonly ship with 2–4 GB RAM, 32–64 GB storage, Android Go or full Android 11–13 with aggressive vendor skins.
- Storage pressure is real: users routinely uninstall apps to free space. Wave's app is notably small (not measured here but reputed). M-Pesa for Business reports ~72–74 MB on Android [secondary source — softonic listing; flagged as approximate].
- A 30–50 MB APK is the right target. A 100+ MB APK is a serious adoption tax in DRC.
- Multi-SIM matters: users may have one Vodacom number for M-Pesa, one Orange for Orange Money, one Airtel for Airtel Money. Our app should not assume the phone number tied to one operator is the only number; users may want to pay to numbers across networks.

### Network reality
- 4G coverage in urban centers (Kinshasa, Lubumbashi, Goma) is reasonable; rural areas often 2G/edge only.
- 75% of connections being "broadband" headline-friendly but doesn't translate to reliable 4G speeds — congested cell sites, frequent fallbacks to 3G/2G.
- Vodacom-Orange announced jointly building 2,000 solar-powered rural base stations in DRC over 6 years [verified fact — connectingafrica.com] — rural network is improving but is *not* fixed for the MVP window.

### Offline-first / low-bandwidth strategies in African fintech
- **MiniPay (Opera)** — built into Opera Mini, ~7M users, marketed as "lightweight" [Opera Limited self-reported]
- **Musoni's Digital Field Application** — fully offline for microfinance officers, syncs when reconnected [secondary]
- **Wave** — app + QR card; QR card allows offline use through agents [verified fact — accessgambia.com]
- **Eversend** — multi-channel: iOS, Android, *and USSD* for feature phones [verified fact — Eversend / FintechFutures]
- **M-Pesa core service** — runs over USSD on feature phones; the app is an *additive* smartphone interface. The DRC M-Pesa app even says "manage your M-Pesa wallet with a data connection or SMS capability" [Google Play description]

## DRC-specific notes
- DRC has 4 mobile operators (Vodacom, Orange, Airtel, Africell) running mobile money on 3 of them (Vodacom M-Pesa, Orange Money, Airtel Money; Africell has its own).
- Vodacom-Orange rural BTS partnership suggests both operators acknowledge the rural coverage gap is real, not solved.
- Network blackouts and government-imposed shutdowns have occurred in DRC during political moments — design assumes the network is *usually* available but cannot assume always.
- Power is a related but separate issue: many users charge phones at shared kiosks. App must tolerate sudden process death and resume cleanly.

## What this means for our MVP
1. **Target APK size: ≤ 40 MB** for the initial install. Use App Bundles (AAB) for per-device delivery to keep installs even smaller. iOS thinner.
2. **Test on at least one Tecno Spark or Infinix Hot device** with 2–3 GB RAM as the primary dev target, not on flagship Pixels.
3. **Optimize for 3G**: target API responses under 200KB, image assets ≤50KB each, lazy-load avatars and receipts.
4. **Persist transaction state aggressively**. If the user kills the app mid-flow, resume on relaunch and never lose money in limbo.
5. **Provide SMS fallback for receipts**, never push-notifications only. SMS is the universal trust signal; push gets lost when network is sketchy.
6. **Multi-SIM aware**: don't auto-detect "your phone number" — let the user pick which number (and which operator) to send from / receive on.
7. **Skip USSD for MVP**, but design the API surface so a future USSD bridge is possible. The smartphone-only segment (30% internet penetration) is still 34M individuals — large enough for MVP.
8. **Defer Lingala/Swahili until v2** unless the team has a native speaker; French + English is what M-Pesa DRC ships [verified fact — Google Play store description].
9. **Skip animations and splash-screen extravaganzas**. Yellow Card's "loads quickly with no slowdown" praise suggests this is universally appreciated.

## Open questions
- What is the actual Transsion device split inside DRC specifically (vs. continent-wide)? StatCounter or GSMA Intelligence could confirm.
- iOS share in DRC specifically — likely <5%, possibly <2% given income — but worth confirming if iOS dev is being weighed against Android-first.
- Does pawaPay's mobile SDK have a measured byte footprint? Material to know before sizing the APK budget.

## Sources used
- DataReportal Digital 2025 DRC — primary (aggregated from GSMA/ITU)
- worlddata.info DRC telecommunication page — secondary
- ts2.tech "No Signal: The Shocking Digital Divide in the DRC" — secondary
- Intelpoint "Mobile phone market share in Africa by region" 2025 — secondary, sourced
- TheAfricaReport on Transsion dominance — secondary
- EqualOcean on Transsion in Africa — secondary
- African Business on Transsion / Chinese investment — secondary
- connectingafrica.com on Vodacom-Orange DRC BTS partnership — secondary
- Eversend rebrand / FintechFutures launch coverage — secondary
- accessgambia.com Wave Gambia profile — secondary
- Tech In Africa €8B DRC digital plan coverage — secondary
- techinafrica / financeinafrica offline-first article — secondary
- M-Pesa DRC Google Play listing — primary
