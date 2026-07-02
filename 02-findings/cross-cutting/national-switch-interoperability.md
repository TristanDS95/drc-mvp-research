# National switch & mandated interoperability (Mosolo / Instruction n°58)

- **Topic / entity:** BCC-mandated interoperability of all electronic payment systems in the DRC, the Switch Monétique National ("Mosolo"), and the interbank body that runs it (GMIC).
- **Question it addresses:** Does the DRC now mandate cross-network mobile money interoperability, through what infrastructure, at what cost, and what does that do to our fee floor, the float-netting idea, and the app's cross-network wedge?
- **Date researched:** 2026-07-01
- **Overall confidence:** High for the instruction's text (primary document read in full); Medium for launch status (news reporting); **pricing = not found**.

## Bottom line
Since March 2025, cross-network interoperability is **not a gap in the DRC market - it is a legal obligation**.
BCC Instruction n°58 (signed 4 Sep 2024, in force 4 Mar 2025) requires every covered institution - banks, e-money issuers, monétique operators, **and aggregators** - to connect to a state-run national switch, and requires every electronic payment instrument to be accepted by every other covered institution.
The switch ("Mosolo") launched in May 2025 with the explicit promise of native M-Pesa-to-Orange-Money transfers; a co-owned interbank body (GMIC) was slated to take over its management around end-March 2026.
Transactions routed through the switch are **cleared on a multilateral net basis** and settled at the central bank - i.e. the netting benefit our [[float-netting-model]] brainstorm wanted to build privately is being delivered centrally, to all participants, without anyone needing to hold custody.
This is simultaneously the biggest **threat** to the app's cross-network wedge (interop becomes native) and the biggest **opportunity** for its cost floor (the cross-network leg should stop costing two full aggregator legs).
The missing number is the switch's transaction pricing, which Instruction 58 leaves to negotiation between the switch manager and participants.

---

## What Instruction n°58 actually says (primary, read in full)

Source: rg-001 (scanned BCC PDF, 10 pages, read page-by-page 2026-07-01).
Issued under Loi 18/019 (payment systems), Loi 22/069 (credit institutions), and Loi 22/068 (AML), citing the National Financial Inclusion Strategy 2023-2028 and "the fragmentation of payment markets" as motivation.

- **Who is covered (art. 2):** credit institutions and sociétés financières offering payment services under art. 168 of Loi 22/069; postal financial services; BCC-approved monétique system operators; **"les agrégateurs"**; and any other establishment the BCC designates.
- **The interoperability mandate (art. 4):** "Tout instrument de paiement électronique émis par un établissement assujetti doit être accepté en paiement par tous les établissements assujettis" - every covered issuer's payment instrument must be accepted by all covered institutions, and every payment platform must be interoperable with all others, organised around a third-party-managed system.
- **Mandatory connection (art. 6):** covered institutions "ont l'obligation de se connecter au Switch Monétique National."
- **Governance (arts. 9-10):** the switch is managed by a **GIE named "Groupement Monétique Interbancaire du Congo" (GMIC)** under OHADA law; covered institutions are required to participate in its co-ownership.
- **Clearing and settlement (arts. 11-12):** operations routed by the switch are **compensated multilaterally** ("compensées sur une base multilatérale"); settlement happens in the BCC's SAREC system; participants without a SAREC settlement account must conclude **indirect participation agreements** with one that has it.
- **Fees (arts. 14-16):** participation and transaction fees are **agreed between the manager and the participants** (not fixed in the instruction). Art. 16 assigns commission incidence: merchant-transaction commission is paid by the acquirer and charged to the merchant; transfer commission between participants is paid by the sender's institution, "notamment... les transferts entre les établissements de monnaie électronique."
- **Timeline (arts. 27-28):** in force 6 months after signature (i.e. 4 Mar 2025), with 6 months for covered institutions to comply; the BCC operates the switch itself until GMIC is created.

## Mosolo and GMIC: launch status

- The Prime Minister announced the imminent launch of the switch, named **"Mosolo"**, on 2 May 2025; press coverage is explicit that it enables an M-Pesa user to transfer to an Orange Money subscriber and vice versa, for ~30M mobile money users (rg-005, rg-006, rg-007). **News reporting, Medium confidence.**
- The **GMIC** (interbank electronic payment group) launch was reported in Jan 2026 as planned for **end-March 2026**, built with **IFC (World Bank Group) support**, covering instant transfers across banks, microfinance institutions, and mobile money (rg-004). **News reporting, Medium confidence.**
- **Whether GMIC actually launched on schedule is not confirmed** - no post-March-2026 primary or news source located in this pass. **Not found; verify.**
- Card-side integrations are already live (Multipay Congo connected its solution to the national switch for Mosolo card transactions, rg-008), so the switch is operational infrastructure, not vapourware. **Medium confidence.**

## What this means for us

1. **The cross-network wedge erodes.** The MVP's core value proposition (cross-network acceptance) is being built into the market's plumbing by the regulator. The app's durable value must sit in what the switch will not do: merchant-side acceptance UX, confirmation, records, reconciliation, refunds. **Inference.**
2. **The float-netting model is largely obsoleted.** Private per-MNO floats plus net settlement replicate what the switch does multilaterally for everyone - except the private version requires an EMI licence and treasury operations, and deprives MNOs of visible revenue (making them hostile). Building it now would mean paying the market's heaviest regulatory toll for infrastructure the market just built. **Inference**, contingent on switch pricing (below).
3. **The cost floor should drop, by an unknown amount.** Once mobile money flows route through Mosolo, a cross-network payment should cost an MNO leg plus a switch commission instead of two full aggregator legs (the 3.5-5.0% envelope in [[fees-and-costs]]). But arts. 14-15 leave pricing to negotiation and nothing is published. A regional analogy: GIM-UEMOA caps interoperable withdrawal fees at a flat ~500 FCFA, suggesting switches price flat and low - **regional proxy, not a DRC fact.**
4. **Aggregators are inside the fence.** Instruction 58 explicitly lists aggregators among covered institutions, so a BCC-authorized aggregator (see [[licensing-categories]]) is both obliged and entitled to connect - via indirect settlement through a bank if it holds no SAREC account. Whether "connection" gives an aggregator the same economic access as an issuer is **open**.

## Confidence summary
- **High:** the text of Instruction n°58 (primary, read in full): the mandate, covered institutions incl. aggregators, GMIC governance, multilateral clearing, SAREC settlement, fee-incidence rules.
- **Medium:** Mosolo launch (May 2025) and GMIC timeline (end-March 2026 target, IFC support) - consistent multi-outlet news reporting.
- **Not found / open:** switch transaction pricing; GMIC's actual go-live status as of mid-2026; participation categories and costs for aggregators; whether MNO wallet-to-wallet interop via Mosolo is fully live for consumers today vs progressive rollout.

## Sources
- rg-001: BCC Instruction n°58 (4 Sep 2024) - interoperability & National Switch participation (scanned PDF via MicroSave mirror) - `https://www.microsave.net/fr/wp-content/uploads/2024/09/Instruction-n%C2%B0-58_Interope%CC%81rabilite%CC%81-des-Syste%CC%80mes-de-paiement-et-a%CC%80-la-participation-au-Switch-Mone%CC%81tique-National-1.pdf`
- rg-004: Bankable - GMIC launch planned end-March 2026 (IFC support) - `https://bankable.africa/fr/actualites/2101-2275-paiement-electronique-lancement-du-groupement-interbancaire-de-la-rdc-d-ici-fin-mars-2026`
- rg-005: Actualite.cd - PM announces Mosolo switch launch (2 May 2025) - `https://actualite.cd/2025/05/04/rdc-le-gouvernement-sapprete-lancer-le-switch-monetique-de-la-banque-centrale-du-congo`
- rg-006: Radio Okapi - "Switch Musolo: la fin des barrières entre opérateurs de mobile money" - `https://www.radiookapi.net/2025/05/07/emissions/echos-deconomie/switch-musolo-en-rdc-la-fin-des-barrieres-entre-operateurs-de`
- rg-007: Zoom Eco - BCC establishes national switch "Mosolo" - `https://zoom-eco.net/a-la-une/rdc-la-bcc-met-en-place-le-switch-monetique-national-denomme-mosolo/`
- rg-008: Zoom Eco - Multipay Congo connects to the national switch (Mosolo card transactions) - `https://zoom-eco.net/banques/rdc-multipay-congo-connecte-sa-solution-multipay-au-switch-national-pour-des-transactions-de-la-carte-mosolo/`

[[licensing-categories]] · [[float-netting-model]] · [[fees-and-costs]] · [[own-aggregator]] · [[kyc-and-onboarding]]
