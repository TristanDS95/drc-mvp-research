# Licensing categories: the DRC authorization ladder for payment players

- **Topic / entity:** the three rungs of BCC authorization relevant to us - payment **aggregator** authorization, **payment institution** (établissement de paiement) agrément, and the **EMI** licence - with the hoops (capital, timeline, local entity) for each.
- **Question it addresses:** What authorization does our app need at each stage of the cost-reduction path, and which path has the fewest / most minor regulatory hoops?
- **Date researched:** 2026-07-01
- **Overall confidence:** High for the aggregator authorization regime (primary instructions read); Medium for the payment-institution tier (primary read but capital amount not found); High for the four licensed EMIs (BCC's own register fetched directly, vp-016) but Medium-High for the EMI *capital* figure (secondary sources; Instruction n°24 text itself not read).

## Bottom line
There is a **live, lightweight BCC authorization category that fits our app exactly**: the "agrégateur des systèmes de paiement," an **autorisation** (not a full agrément) under Instruction n°42, requiring a locally incorporated company and a document dossier but **no stated minimum capital**, with a **60-day statutory decision clock** under Loi 18/019.
The BCC publishes a registry of dozens of authorized aggregators - **pawaPay itself operates in the DRC as "Kerry Payments SARLU" on this registry**, and Paymetrust obtained the same approval in July 2025.
One rung up, the 2022 banking law (Loi 22/069, art. 168) created **"établissements de paiement"** as a société financière category (with EMIs as a subset), agréé under Instruction n°53: local company, notarized dossier, 90-day clock, minimum capital "as fixed by the BCC" in a separate text - **amount not found; the single most valuable question for counsel**, since if modest it may permit holding funds in transit without full EMI status.
The top rung, the **EMI licence** (~US$2.5M paid-up capital), is confirmed by a second source - and notably, **the only four licensed EMIs are the four MNOs' subsidiaries**; there is no independent-startup-EMI precedent.
Implication for the corpus's standing legal flag: the "thin pass-through wrapper inherits the rail's posture" inference looks **weak** - the BCC authorizes aggregators and e-commerce platform managers by name, and our merchant-facing app plausibly fits the aggregator definition itself.
The good news: that hoop is the small one.

---

## Rung 1 - Aggregator authorization (autorisation, Instruction n°42)

Source: rg-002 (scanned BCC PDF; title page + arts. 1-12 read page-by-page 2026-07-01). Instruction n°42 of 9 Mar 2020, "relative aux règles applicables à la monétique en RDC."

- **Definition (art. 1(4)):** an aggregator is a "personne morale, prestataire de service technique de paiement qui offre des services de paiements et des solutions aux institutions financières dans le cadre des systèmes de paiement."
- **Regime (art. 9):** aggregators are "prestataires des services connexes" (alongside card manufacturers, personalisation centres, acquisition-channel providers, monétique software editors, and **e-commerce platform managers**) and "désirant s'établir en RDC doivent obtenir une **autorisation** de la Banque Centrale."
- **The dossier (art. 9):** company statutes; legal representatives' powers; principal shareholders and directors; identities of anyone holding capital directly or indirectly; proof of directors' good character; certified financials (or projections if under a year old); business plan; technical certificates; standard SLA; technical/functional specs; settlement procedures for ordinary and crisis situations; risk-management procedures. **No minimum capital appears in the connected-services provisions.**
- **Decision clock (art. 7):** the BCC decides "dans un délai de soixante jours à compter de la réception du dossier complet" (60 days), extensible if it needs more information or a foreign regulator's opinion.
- **Statutory backing (Loi 18/019, arts. 108-110, rg-009):** no one may operate a payment system or issue payment instruments without BCC agrément; the application requires an activity programme, a 3-year business plan, proof of the minimum initial capital **"fixé par la Banque centrale,"** fund-protection measures (for issuers), and governance/internal controls; art. 108 also sets a 60-day decision window.
- **Interoperability obligations attach (arts. 11-12 of Instruction 42):** every payment platform open to the public must be able to exchange transaction data with all others; e-money holders must be able to pay any acceptor and load/withdraw at any issuer's agents. This 2020 obligation is what Instruction n°58 operationalised via the national switch (see [[national-switch-interoperability]]).

**Precedents (verified, High confidence):**
- The BCC's public registry of authorized aggregators lists dozens of entities, including **KERRY PAYMENTS SARLU (pawaPay)**, MFS AFRICA SARL, PLURITONE SAS (Maxicash), FRESHPAY, CINETPAY RDC, and MULTIPAY (rg-003). So the rail we currently rent holds **this** authorization in the DRC - not an EMI licence.
- **Paymetrust** received BCC approval to operate as a financial aggregator on **4 July 2025**, with the reported compliance obligation to connect to the national e-money switch (rg-010). The route is being actively granted.

## Rung 2 - Payment institution (agrément, Instruction n°53 / Loi 22/069)

Source: rg-011 (scanned BCC PDF; arts. 1-11 read page-by-page 2026-07-01). Instruction n°53, "relative aux conditions d'agrément des sociétés financières, de leurs dirigeants et de la modification de leurs situations statutaires," issued under Loi 22/069 of 27 Dec 2022.

- **The category (art. 2):** sociétés financières include "les **établissements de paiement** fournissant les services de paiement visés à l'article 168 de la Loi n°22/069... **dont les établissements de monnaie électronique**, les messageries financières." So under the 2022 law, **EMIs are a subset of payment institutions**, which are a subset of sociétés financières - a PSD-style ladder.
- **Local incorporation is required (arts. 3-4):** a société financière is a "personne morale **de droit congolais**"; agrément is for locally incorporated companies. This resolves (in the strict sense) the corpus's open flag about foreign operators: the licensed entity must be Congolese; plan a DRC subsidiary for any rung of this ladder.
- **Dossier (art. 11):** notarized statutes and constitutive assembly minutes, RCCM registration, **"attestation de dépôt, auprès d'une banque locale, du capital social libéré à hauteur d'un montant au moins égal au capital minimum fixé par la Banque Centrale du Congo"** (paid-up capital deposited at a local bank, amount fixed elsewhere), shareholder and director honorability declarations (AML), casier judiciaire extracts, etc.
- **Decision clock (art. 9):** 90 days from the letter confirming the dossier is complete (clock pauses for requested additions; extends for foreign-regulator consultations).
- **The missing number:** the instruction fixing the **minimum capital for établissements de paiement** was **not found** in this pass (the BCC fixes capital per subcategory in separate instructions; bank capital is $50M, microfinance $50k-$300k, EMI $2.5M - nothing located for non-EMI payment institutions). **Flag as the #1 counsel question:** if this figure is modest, a payment-institution agrément may permit holding funds in transit (acquiring-style custody, merchant settlement) without the $2.5M EMI licence.

## Rung 3 - EMI licence (the custody licence)

- **US$2.5M minimum paid-up capital** under BCC Instruction n°24, previously sourced from findevgateway/PayAtlas in [[kyc-and-onboarding]] and [[own-aggregator]], now **confirmed by an additional independent source** (rg-012: "un capital social minimum libéré en numéraire équivalent en CDF à 2.500.000 USD"). **Confidence upgraded to Medium-High.**
- **The only four approved EMIs are the MNO subsidiaries:** Vodacash SA, Airtel Money RDC SA, Orange Money RDC SA, Afrimobile Money SA - now **confirmed directly against the BCC's own live EMI register** (vp-016, fetched 2026-07-01, with each entity's registration decision + date: Vodacash D.03/01111 2012-07-30; Airtel Money RDC D.03/0200 2012-02-06; Orange Money RDC D.03/0438 2012-03-26; Afrimobile Money D.03/1061 2015-09-21), not just the secondary source (rg-012). **No independent startup EMI precedent** - the route is even heavier in practice than on paper. **Confidence: High (primary regulator register).**
- Under Loi 22/069 the EMI now sits inside the société financière/payment-institution framework (Instruction 53 applies to its agrément process).

## What this changes in our standing assessments

1. **The fewest-hoops path to Avenue 1 (direct MNO deals) is the aggregator authorization**, not an EMI licence: DRC subsidiary (OHADA SARL), dossier, 60-day clock, no stated capital floor. pawaPay's own posture proves sufficiency for running multi-MNO collect/payout rails.
2. **The corpus's "custody = EMI" line needs a middle step**: payment-institution agrément may cover funds-in-transit at far lower capital. Unverified until the capital instruction is found or counsel confirms.
3. **Our current model may already need the autorisation.** Instruction 42 requires operators of monétique systems to verify that local connected-services providers hold BCC authorization before contracting them; a merchant-facing app aggregating MNO payments plausibly **is** an aggregator (or e-commerce platform manager) in the BCC's taxonomy. Treat "do we need it for the beta?" as a counsel question, and the authorization itself as table stakes rather than a future decision. **Inference from primary texts; not legal advice.**

## Confidence summary
- **High:** Instruction 42's aggregator definition, autorisation dossier, 60-day clock; Loi 18/019 arts. 108-110; the BCC aggregator registry contents (pawaPay = Kerry Payments SARLU); Instruction 53's scope, local-incorporation requirement, dossier, 90-day clock; **the four licensed EMIs and their registration decisions (BCC's own EMI register fetched directly, vp-016).**
- **Medium-High:** EMI capital US$2.5M (two independent secondary sources quoting Instruction 24; BCC primary text of Instr. 24 itself still not directly read - bcc.cd TLS issues).
- **Medium:** Paymetrust approval details (two news outlets, rg-010 + vp-015; not yet on the BCC's published aggregator register).
- **Not found / open:** minimum capital for non-EMI établissements de paiement; whether the current pawaPay-fronted merchant app requires the autorisation pre-beta; aggregator economic-participation terms in GMIC.

## Sources
- rg-002: BCC Instruction n°42 (9 Mar 2020) - règles applicables à la monétique (scanned PDF, bcc.cd) - `https://www.bcc.cd/system/files_force/dsif/bcc_instruction_gouv_ndeg42_20_regles_applicables_a_la_monetique_en_rdc.pdf/?download=1`
- rg-003: BCC - registry "Les Agrégateurs des systèmes de paiement" (prestataires de services connexes autorisés) - `https://www.bcc.cd/systemes-et-instruments-de-paiement/cartographie-des-systemes-de-paiement-en-rdc/les-prestataires-de-services-connexes-de-paiement-autorises-par-la-bcc/les-agregateurs-des-systemes-de-paiement`
- rg-009: Loi n°18/019 du 9 juillet 2018 relative aux systèmes de paiement et de règlement-titres (full text PDF, droitcongolais.info) - `https://www.droitcongolais.info/files/221.07.18-Loi-du-9-juillet-2018_Systemes-de-paiement.pdf`
- rg-010: Bankable - "Paymetrust gains Central Bank approval to operate in DRC" (4 Jul 2025) - `https://bankable.africa/en/digital/2309-1725-paymetrust-gains-central-bank-approval-to-operate-in-drc`
- rg-011: BCC Instruction n°53 - conditions d'agrément des sociétés financières (scanned PDF, bcc.cd) - `https://www.bcc.cd/sites/default/files/dsif/inst._53.pdf`
- rg-012: Me Maxence Kiyana - "La monnaie électronique en RDC" (EMI capital $2.5M; the 4 approved EMIs) - `https://maxencekiyana.com/la-monnaie-electronique-en-rdc/`
- rg-013: avocats.cd - "Les agrégateurs des systèmes de paiement en RDC" (secondary overview of Instruction 42 aggregator regime) - `https://avocats.cd/blog/les-agregateurs-des-systemes-de-paiement-en-rdc`
- rg-014: village-justice - "Le cadre légal et règlementaire des activités financières en RDC" (Loi 22/069 category structure) - `https://www.village-justice.com/articles/cadre-legal-reglementaire-des-activites-financieres-rdc,46461.html`
- vp-015: Financial Afrik - Paymetrust BCC agrément as prestataire de services connexes/agrégateur (4 Jul 2025; second independent outlet) - `https://www.financialafrik.com/2025/09/22/paymetrust-consolide-sa-presence-en-afrique-avec-un-agrement-dagregateur-en-republique-democratique-du-congo/`
- vp-016: BCC - live register of EMIs (exactly 4: Vodacash D.03/01111 2012-07-30; Airtel Money RDC D.03/0200 2012-02-06; Orange Money RDC D.03/0438 2012-03-26; Afrimobile Money D.03/1061 2015-09-21) - `https://www.bcc.cd/surveillance-des-intermediaires-financiers/intermediaires-financiers-assujettis/etablissements-de-credit/societes-financieres/ee`

[[national-switch-interoperability]] · [[kyc-and-onboarding]] · [[own-aggregator]] · [[float-netting-model]] · [[fees-and-costs]]
