# National switch & mandated interoperability (Mosolo / Instruction n°58)

- **Topic / entity:** BCC-mandated interoperability of all electronic payment systems in the DRC, the Switch Monétique National ("Mosolo"), and the interbank body that runs it (GMIC).
- **Question it addresses:** Does the DRC now mandate cross-network mobile money interoperability, through what infrastructure, at what cost, and what does that do to our fee floor, the float-netting idea, and the app's cross-network wedge?
- **Date researched:** 2026-07-01 (revised same day after challenge - see "The delivery record")
- **Overall confidence:** High for the instruction's text (primary document read in full); Medium for launch announcements (news reporting); **Low for actual delivery of mobile-money interop via the switch**; **pricing = not found**.

## Bottom line
On paper, cross-network interoperability in the DRC is **a legal obligation, not a market gap**: BCC Instruction n°58 (signed 4 Sep 2024, in force 4 Mar 2025) requires every covered institution - banks, e-money issuers, monétique operators, **and aggregators** - to connect to a state-run national switch, with **multilateral net clearing** settled at the central bank.
In practice, the delivery record argues for heavy discounting: the "Switch Monétique National" was **first launched on 23 December 2020** and described in 2021 press as operational and covering mobile money, then **re-announced as "Mosolo" in May 2025**; Instruction 42 already mandated interoperability in **March 2020**; and operator-level wallet-to-wallet interop (Orange's "Kabola na voisin" with M-Pesa) has existed **since ~2021** at ~1% consumer rates - yet the June-2026 fieldwork in this corpus still found a fragmented market where cross-network merchant acceptance costs 3.5-5% via aggregators.
Two conclusions follow.
First, **a mandate is not a product**: mandated interop is neither free (Instruction 58's arts. 14-16 institutionalise fees at every layer) nor self-executing, and five years of nominal P2P interop did not close the merchant-acceptance gap this app targets - the switch serves *institutions*; merchants still need an *acquirer*, which is exactly our seat.
Second, the switch is still strategically important **as a conditional**: if GMIC materialises and wallet-to-wallet routing actually works, it becomes both a cheaper cross-network rail for an authorized aggregator and a (partial) threat to a routing-only value proposition - so it should be **monitored with leading indicators** (GMIC's legal creation, published switch tariffs, operator documentation of switch-routed transfers), not treated as either fact or fiction.
The missing numbers remain switch pricing (negotiated, not public) and any post-launch volume evidence (**none found**).

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

## Mosolo and GMIC: launch announcements

- The Prime Minister announced the imminent launch of the switch, named **"Mosolo"**, on 2 May 2025; press coverage is explicit that it enables an M-Pesa user to transfer to an Orange Money subscriber and vice versa, for ~30M mobile money users (rg-005, rg-006, rg-007). **News reporting of an announcement, Medium confidence that the announcement happened; no confidence attaches to delivery.**
- The **GMIC** (interbank electronic payment group) launch was reported in Jan 2026 as planned for **end-March 2026**, built with **IFC (World Bank Group) support**, covering instant transfers across banks, microfinance institutions, and mobile money (rg-004). **News reporting, Medium confidence.**
- **Whether GMIC was actually created is not confirmed** - three targeted searches (2026-07-01) found no post-March-2026 evidence of its creation, no switch transaction statistics, and no consumer-facing documentation of switch-routed wallet-to-wallet transfers. **Not found.** Under Instruction 58 art. 27, the BCC itself operates the switch until GMIC exists.
- Card-side integrations exist (Multipay Congo connected for Mosolo card transactions, Dec 2024, rg-008), so the switch is real infrastructure for **cards**. Evidence of the **mobile-money leg** functioning: **none found.**

## The delivery record (why to discount the announcements)

Added after challenge (2026-07-01). Three pieces of in-corpus and primary evidence argue that DRC interop mandates and switch announcements systematically outrun delivery:

1. **The switch has been "launched" before.** The Switch Monétique National was launched on **23 December 2020**, and July-2021 press credited its "operationalization" to outgoing governor Mutombo as a legacy achievement, nominally covering cards **and mobile money** (M-Pesa, Orange Money, Airtel Money, Afrimoney) (rg-015). Four years later the same switch was re-announced as "Mosolo" (rg-005). **Verified (press), High confidence in the 2020 launch date; the gap between the two announcements is the finding.**
2. **The interoperability mandate is not new either.** Instruction 42 (March 2020) art. 11 already required every public payment platform to be interoperable (see [[licensing-categories]], rg-002). Six years on, the corpus's own fieldwork found the market still fragmented. A second, stronger instruction (n°58) is evidence the first one did not execute itself.
3. **Operator-level P2P interop has existed for ~5 years without closing our gap.** Orange's "Kabola na voisin" lets an Orange Money customer send directly to M-Pesa (and other networks) at standard transfer rates (~1% off-net per [[fees-and-costs]]), gated on an upgraded-KYC account (rg-016; corpus payee-identification finding dates M-Pesa↔Orange bilateral interop to 2021). Despite this, cross-network **merchant acceptance** still costs 3.5-5% through aggregators and remains rare. **P2P transfer interop and merchant acceptance are different products** - the existence of the former has demonstrably not produced the latter.

## What this means for us

1. **A mandate is not free, and not a product.** Instruction 58 makes *refusing* interop illegal; it does not make interop free - arts. 14-16 explicitly institutionalise commissions at every layer (switch participation fees, transfer commissions, and merchant commissions charged by acquirers). Even a fully functioning switch leaves fee stacks and an open acquirer seat. **Primary text + inference.**
2. **The merchant-acceptance value proposition survives a working switch.** The switch interconnects *institutions*; a merchant still needs an acquirer for acceptance UX, confirmation, records, reconciliation, refunds - art. 16's fee-incidence rule presupposes acquirers exist and charge merchants. The delivery record (above) shows even live P2P interop did not close this gap. The app's moat was already the merchant layer; this strengthens rather than weakens that framing. **Inference from primary text + corpus evidence.**
3. **The wedge risk is conditional, not current.** *If* GMIC materialises and wallet-to-wallet switch routing becomes real and frictionless, a routing-only value proposition erodes. Leading indicators to watch: GMIC's legal creation (OHADA GIE registration), published switch tariffs, operator help pages documenting switch-routed transfers. Until those appear, treat the threat as monitored, not material. **Inference.**
4. **The float-netting verdict is likewise conditional.** If the switch delivers, private netting duplicates central infrastructure at the market's heaviest regulatory price (see [[float-netting-model]]); if the switch stalls, the model's *original* obstacles stand unchanged (EMI gate, MNO hostility to fee circumvention, treasury ops). Neither branch favours building it. **Inference.**
5. **The cost-floor upside is real but unpriced and unproven.** A working switch should turn a cross-network payment into an MNO leg plus a switch commission instead of two full aggregator legs (the 3.5-5.0% envelope in [[fees-and-costs]]). Arts. 14-15 leave pricing to negotiation; nothing is published; and the mobile-money leg itself is unevidenced. A regional analogy (GIM-UEMOA's flat ~500 FCFA cap on interoperable withdrawals) suggests switches price flat and low - **regional proxy, not a DRC fact.**
6. **Aggregators are inside the fence.** Instruction 58 explicitly lists aggregators among covered institutions, so a BCC-authorized aggregator (see [[licensing-categories]]) is both obliged and entitled to connect - via indirect settlement through a bank if it holds no SAREC account. Whether "connection" gives an aggregator the same economic access as an issuer is **open**. Enforcement of the connection obligation is also untested - consistent with the delivery record.

## Confidence summary
- **High:** the text of Instruction n°58 (primary, read in full): the mandate, covered institutions incl. aggregators, GMIC governance, multilateral clearing, SAREC settlement, fee-incidence rules. Also High: the switch's first launch in Dec 2020 and its re-announcement in May 2025 (the delivery-record finding).
- **Medium:** Mosolo announcement details (May 2025) and GMIC timeline (end-March 2026 target, IFC support) - consistent multi-outlet news reporting of *plans*.
- **Low:** that the switch's mobile-money leg functions for consumers today; that GMIC exists; that the connection mandate is enforced.
- **Not found / open:** switch transaction pricing; any post-launch volume or adoption evidence; GMIC's creation; participation categories and costs for aggregators.

## Sources
- rg-001: BCC Instruction n°58 (4 Sep 2024) - interoperability & National Switch participation (scanned PDF via MicroSave mirror) - `https://www.microsave.net/fr/wp-content/uploads/2024/09/Instruction-n%C2%B0-58_Interope%CC%81rabilite%CC%81-des-Syste%CC%80mes-de-paiement-et-a%CC%80-la-participation-au-Switch-Mone%CC%81tique-National-1.pdf`
- rg-004: Bankable - GMIC launch planned end-March 2026 (IFC support) - `https://bankable.africa/fr/actualites/2101-2275-paiement-electronique-lancement-du-groupement-interbancaire-de-la-rdc-d-ici-fin-mars-2026`
- rg-005: Actualite.cd - PM announces Mosolo switch launch (2 May 2025) - `https://actualite.cd/2025/05/04/rdc-le-gouvernement-sapprete-lancer-le-switch-monetique-de-la-banque-centrale-du-congo`
- rg-006: Radio Okapi - "Switch Musolo: la fin des barrières entre opérateurs de mobile money" - `https://www.radiookapi.net/2025/05/07/emissions/echos-deconomie/switch-musolo-en-rdc-la-fin-des-barrieres-entre-operateurs-de`
- rg-007: Zoom Eco - BCC establishes national switch "Mosolo" - `https://zoom-eco.net/a-la-une/rdc-la-bcc-met-en-place-le-switch-monetique-national-denomme-mosolo/`
- rg-008: Zoom Eco - Multipay Congo connects to the national switch (Mosolo card transactions) - `https://zoom-eco.net/banques/rdc-multipay-congo-connecte-sa-solution-multipay-au-switch-national-pour-des-transactions-de-la-carte-mosolo/`
- rg-015: DeskEco (6 Jul 2021) - "L'opérationnalisation du Switch Monétique National, une des réalisations majeures léguées par Déogratias Mutombo" (switch launched 23 Dec 2020; nominal mobile-money coverage claimed in 2021) - `https://deskeco.com/2021/07/06/rdc-loperationnalisation-du-switch-monetique-national-une-des-realisations-majeures-leguees-par`
- rg-016: Orange RDC - "Interopérabilité Orange Money - qu'est-ce que le service Mpesa (Kabola na voisin)?" (live operator-level wallet-to-wallet interop at standard transfer rates, gated on upgraded-KYC account) - `https://www.orange.cd/fr/interoperabilite-orange-money-quest-ce-que-le-service-mpesa-kabola-na-voisin.html`

[[licensing-categories]] · [[float-netting-model]] · [[fees-and-costs]] · [[own-aggregator]] · [[kyc-and-onboarding]]
