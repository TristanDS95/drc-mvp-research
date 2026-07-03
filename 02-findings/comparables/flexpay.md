# FlexPay / FlexPaie (INFOSET) - the closest existing DRC rival

- **Topic / entity:** FlexPay, rebranded/relaunched as **FlexPaie** (Oct 2025), by INFOSET Sarl (Kinshasa) - the DRC product whose mechanics most resemble ours.
- **Question it addresses:** How is what FlexPay does different from our product, and what does its existence prove or threaten?
- **Date researched:** 2026-07-03
- **Overall confidence:** High for regulatory status and payment mechanics (their own terms + developer docs); Medium for positioning claims (their marketing); Medium-High for traction (Play Store metrics, a proxy).

## Bottom line
FlexPay is the strongest **validation** of our model and the weakest **threat** to it, at the same time.
Validation: it is a locally owned, BCC-authorized Instruction-42 aggregator (authorization n° GOUV/D25/00362, 6 April 2020 - one of the earliest), it explicitly **holds no funds** ("FlexPay ne possède pas son propre portemonnaie électronique"), and its payer flow is exactly ours: the customer receives a **USSD push from their own operator and enters their PIN - no app needed**, across **all four networks** (incl. Afrimoney) plus Visa/Mastercard/Amex.
So the permit, the pass-through posture, the USSD-push flow, and even 4-network coverage are all **proven feasible** by a local player.
Threat, assessed: small.
After ~6 years of authorization, its merchant app shows **~1K+ Play Store downloads and essentially no reviews** (updated May 2026, so actively maintained), and its October 2025 relaunch as "FlexPaie" positions it **up-market and online**: a "virtual TPE" and payment-pages/API product aimed at e-commerce, churches, event ticketing, rent collection, and formal partners (Kin-Marché, Regideso, Monishop; "powered by VISA," PCI-DSS hosting).
Nobody in that positioning is walking the street signing up informal merchants.
The lesson cuts both ways: **the mechanics are commodity; adoption is the moat** - FlexPay proves a working product with the right permit does not, by itself, win the small-merchant market.

## What FlexPay/FlexPaie is (verified)

- **Operator:** INFOSET Sarl, RCCM 14-B-2220, Gombe, Kinshasa (fp-001). INFOSET appears on the BCC's aggregator register (rg-003).
- **Regulatory:** BCC autorisation as "agrégateur des services monétiques" under Instruction 42, n° GOUV/D25/00362 of 6 April 2020 (fp-001). **Primary, High.**
- **No custody:** terms state FlexPay has no e-wallet of its own; funds move between accounts held at financial institutions; the merchant designates the receiving account (bank, mobile money, or prepaid) (fp-001). **Primary, High.**
- **Payer flow:** merchant code or QR; for mobile money the payer gets a **USSD push** on their own phone and confirms with their operator PIN - no FlexPay app required (fp-001 ambiguous; fp-004 explicit). **High.**
- **Coverage:** M-Pesa, Airtel Money, Orange Money, **Afrimoney**, plus Visa/Mastercard/Amex (fp-002, fp-004). Note: broader than pawaPay's 3-network DRC coverage.
- **Fees:** "tarification convenue d'un commun accord avec les opérateurs" - negotiated per operator, schedule not public (fp-001). **Not found: actual rates.**
- **Product surface (post-Oct-2025 FlexPaie):** Android/iOS/web "virtual TPE," 24/7 payment pages, merchant dashboard with mini-accounting, e-commerce API (npm package, webhooks), instant transfer to the account of choice (fp-002, fp-003, fp-005).
- **Declared targets:** e-commerce merchants, churches/nonprofits, event organizers, landlords, service providers; 13 named partner companies incl. Kin-Marché (supermarkets), Regideso (state water utility), Monishop (fp-002, fp-005). **Self-reported, Medium.**
- **Traction proxy:** Play Store ~**1K+ downloads**, no visible review base, last updated 2026-05-12 (fp-005). An iOS app exists. **Medium-High for the number itself; downloads ≈ merchant-side installs, so active merchants are likely fewer.**
- **Traction, searched exhaustively (2026-07-03): no adoption numbers exist anywhere public.** The launch-event coverage (fp-003, fp-006), the product site (fp-002), and INFOSET's own corporate portfolio page (fp-007) contain **zero** figures - no merchant counts, no user counts, no transaction volumes, no targets - even at their own October 2025 relaunch, where companies with good numbers cite them. The only public traction signals are the ~1K+ installs, the absent review base, and 13 named partner companies. **Caveat that keeps this honest:** their real volume may flow through web/API checkout (invisible to app downloads) - FlexPaie is bundled with INFOSET's own platforms (FlexBiz e-commerce, SmartSchool school-fees, FlexEvent ticketing, fp-007), so it reads as an IT group's internal payment gateway monetised across its verticals, **not** a failed assault on the street-merchant market. They appear never to have seriously attempted informal merchants; the segment is unclaimed rather than proven lost.

## How we differ (the honest comparison)

| Dimension | FlexPay/FlexPaie | Us |
|---|---|---|
| Regulatory shape | Instruction-42 aggregator, no custody | Same shape (permit pending; see [[licensing-categories]]) |
| Payer flow | USSD push + PIN, no app | Same |
| Networks | All 4 + international cards | 3 (pawaPay's coverage), no cards |
| Rails | Own direct operator integrations (implied by 2020 vintage + Afrimoney) | Rented (pawaPay) for the MVP; direct deals are Step 2 |
| Target customer | Online collections + formal organizations (churches, events, rent, utilities, chains) | Informal/small physical merchants (the till on the counter) |
| Product center | Payment pages, e-commerce API, dashboard | The merchant till: in-person acceptance, instant confirmation, records/reconciliation, free on-net recording |
| On-net (same-network) payments | No equivalent free-facilitation concept found | Free facilitate-and-record, permanent |
| Distribution | Partner/enterprise deals; no visible street-level merchant acquisition | Feet-on-the-street onboarding is the plan and the bet |
| Traction | ~1K+ app installs after ~6 years authorized | Beta not launched |

## What this proves, threatens, and teaches

1. **Proves (good for us):** the light permit works for exactly our model; pass-through without custody is an accepted posture; USSD push works across all four networks; a local company can integrate operators directly (our Step 2 is not exotic).
2. **Threatens (real but bounded):** if FlexPaie ever pivots down-market with real distribution, it starts years ahead on integrations and the permit. Its card acceptance and Afrimoney coverage are genuine feature advantages today. Watch for: a published fee schedule, street-merchant marketing, agent/sales-force hiring.
3. **Teaches (the uncomfortable one):** mechanics do not sell themselves. A working, authorized, 4-network acceptance product has existed since ~2020 and captured ~nothing of the small-merchant market. Whatever blocked them - distribution cost, merchant education, fees, product fit - is the actual problem our beta must crack. "We can do it" was never the bet; "merchants will adopt and pay" is.

## Sources
- fp-001: FlexPay terms & conditions (INFOSET; BCC auth n° GOUV/D25/00362 6-Apr-2020; no own e-wallet; merchant code/QR; negotiated operator fees) - `https://www.flexpay.cd/accueil/tc/` - accessed 2026-07-03 - primary, High.
- fp-002: FlexPaie product site (payment pages, dashboard, API; targets e-commerce/churches/events/rent; 4 MNOs + Visa/MC/Amex; 13 partners incl. Kin-Marché, Regideso, Monishop) - `https://flexpaie.com/` - accessed 2026-07-03 - self-reported, Medium-High. Note: flexpay.cd now 302-redirects here.
- fp-003: 7sur7.cd - FlexPaie launch, Kinshasa, 30 Oct 2025 (Gabriel Zema, DG INFOSET GROUP; single-interface aggregation; instant transfer to chosen account) - `https://7sur7.cd/2025/10/30/rdc-securisation-des-transactions-financieres-lancement-de-flexpaie-une-solution` - accessed 2026-07-03 - news, Medium.
- fp-004: David Kathoh (Medium) - FlexPay API integration guide (USSD push + PIN flow explicit; 4 networks; npm + webhooks) - `https://davidkathoh.medium.com/int%C3%A9grez-le-paiement-en-une-minute-dans-votre-application-avec-flexpay-649891d6448d` - accessed 2026-07-03 - third-party developer, Medium.
- fp-005: FlexPaie on Google Play (cd.infoset.flexpaie: 1K+ downloads, updated 2026-05-12; "virtual TPE"; PCI-DSS; "powered by VISA") - `https://play.google.com/store/apps/details?id=cd.infoset.flexpaie` - accessed 2026-07-03 - primary listing, Medium-High.
- fp-006: numerico.cd - FlexPaie launch coverage (31 Oct 2025; zero adoption metrics) - `https://numerico.cd/2025/10/31/rdc-infoset-group-lance-flexpaie-une-application-de-paiements-numeriques/` - accessed 2026-07-03 - news, Medium.
- fp-007: INFOSET GROUP portfolio page for FlexPaie (no metrics, no clients/dates; bundled with FlexBiz/SmartSchool/FlexEvent) - `https://infosetgroup.com/portfolio/flexpaie/` - accessed 2026-07-03 - primary (self-reported), Medium.

[[illicocash]] · [[licensing-categories]] · [[fees-and-costs]] · [[on-net-direct-operator-apis]]
