# DRC Cross-Network Payment App — research & product workspace

A structured workspace that started as **MVP feasibility research** and has grown to also hold the **product spec**, **comparative fintech research**, and **decision reports**. App development is **underway** in the sibling `../drc-pay/`.

The product: a **merchant-facing** app for the DRC — merchants accept mobile-money payments from customers **across networks** (Vodacom M-Pesa, Airtel, Orange), bridged behind the scenes, on **rented rails** (pawaPay) as a **pure pass-through** (we never hold funds). The **customer pays the sticker price and the merchant absorbs our fee (MDR)**; settlement is instant. **Customers need no app or internet** — they scan the merchant's QR or dial a USSD till — so **USSD is a first-class, MVP-critical channel** (not phased). A consumer-facing version may come much later.

Driven by an agentic assistant (Claude Code), which reads `CLAUDE.md` for its working standards.

## What's been done (high level)
1. **Feasibility** — operators (M-Pesa, Orange, Airtel, Afrimoney), aggregators (pawaPay, DPO, MOKO, Onafriq, Hub2), fees, payee-ID, KYC, and the **USSD gateway** options. Conclusion: build on **pawaPay**; customers pay a merchant **till** (dialed or via a QR `tel:` dial-through); **merchant onboarding is in scope**.
2. **Product spec** — functionality, UI, tech stack, screens, data model, API contracts (`05-product-spec/`).
3. **Build-vs-rent & offline** — why we rent rails now; feasibility of a feature-phone/USSD version.
4. **Architecture comparison** — merchant-acquiring (now chosen) vs the earlier consumer pass-through; the 2026-06 team pivot reversed the earlier verdict (the report records the evolution, and caught real spec bugs).
5. **Comparative fintech research** — 8 companies (M-Pesa, Wave, OPay/PalmPay, MTN/Airtel, Chipper/Flutterwave, Onafriq, bKash, global super-apps) studied for structure, monetisation, and rollout.

## Start here (key documents)
- **Short digest (start here for the tour):** `reports.html` - a phase-by-phase plain-English summary that links into the detail.
- **Full research report (detailed):** `report.html` - the deep, fully-sourced feasibility report.
- **Who competes with us (plain-language):** `competitor-landscape.html` - sorts pawaPay, IllicoCash, the mobile networks, and foreign look-alikes into real vs. not-real competitors.
- **Product spec:** `05-product-spec/README.md` (+ `product-spec.html` summary)
- **The recommendation:** `03-reports/recommendation.md`
- **Architecture: merchant-acquiring (chosen) vs the earlier consumer model:** `03-reports/architecture-comparison.md`
- **What we learned from other fintechs:** `03-reports/comparables-synthesis.md`
- **Build plan & costs:** `mvp-deliverables.html`, `mvp-schedule.html`, `roadmap.html`
- **Progress tracker:** `_status/progress.md`

## Structure
```
drc-mvp-research/
├── README.md                  # this file
├── CLAUDE.md                  # standing instructions / standards for the agent
├── 00-overview/               # context: product summary, research goals, glossary
├── 01-questions/              # what we're investigating, by area
├── 02-findings/               # raw, SOURCED findings (the evidence base)
│   ├── operators/             # M-Pesa, Orange, Airtel, Afrimoney
│   ├── aggregators/           # pawaPay, DPO, MOKO, Onafriq (+ Hub2 considered)
│   ├── cross-cutting/         # fees, payee-ID, KYC, build-own-aggregator, offline-USSD
│   ├── app-development/       # pan-African fintech app-dev research (UX, stack, devices…)
│   └── comparables/           # 8 fintech company profiles (structure/value/rollout)
├── 03-reports/                # synthesised, decision-oriented reports (built from findings)
│   ├── feasibility-summary.md · operator-api-comparison.md · aggregator-comparison.md
│   ├── recommendation.md · architecture-comparison.md · comparables-synthesis.md
├── 04-sources/sources.md      # master source registry (every claim traces here)
├── 05-product-spec/           # how the app works: functionality, ui, screens, data-model, api, tech-stack
├── _templates/ · _status/     # note templates · progress tracker
└── *.html                     # human-readable summaries (reports=digest, report=full, competitor-landscape, product-spec, schedule, roadmap, deliverables)
```

## Ground rules (full version in CLAUDE.md)
- Every factual claim is **sourced and dated**; reports synthesise **only** from recorded findings.
- **"Not found / unknown" is a valid finding** — never guess or invent.
- Prefer primary/official sources; label self-reported/marketing claims; flag DRC-specific vs. regional proxies.
- **In scope now:** feasibility + feature-phone/USSD + comparative product-strategy research.
- **Still a separate track:** quantitative demand forecasting / market sizing.
- Legal/licensing is an item **to investigate**, not a defined requirement.
