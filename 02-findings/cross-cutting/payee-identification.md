# Payee identification (cross-cutting)

> **⚠️ Pre-pivot framing (2026-06-11 correction applies).** This file was written for the *earlier* model with **no merchant onboarding**, so it treats the payee as a personal mobile-money number (MSISDN). The MVP has since pivoted to **merchant-acquiring**: merchants onboard and are addressed by a **till / short code or the merchant's QR**, not a personal number. Read the MSISDN routing below as the **fallback / historical** path; till-and-QR addressing is now primary. Current direction: [`00-overview/product-summary.md`](../../00-overview/product-summary.md) and [`05-product-spec/README.md`](../../05-product-spec/README.md).

- **Topic / entity:** how a payer designates the merchant to pay (originally scoped for no merchant onboarding; post-pivot the MVP onboards merchants and addresses them by till/QR - see the notice above)
- **Question it addresses:** Can payments be addressed by mobile-money number? Are there merchant or QR codes? Is there a national/regional QR standard?
- **Date researched:** 2026-06-04
- **Overall confidence:** Medium — primary addressing is by mobile-money number, with operator-specific merchant short codes layered on top; **no DRC national QR standard located**; QR exists only as operator/PSP-specific implementations.

## Bottom line
For an MVP that does **not** onboard merchants, payments are addressable to a **merchant's mobile money number (MSISDN)** on the merchant's chosen network. Operator-specific merchant short codes exist (M-Pesa issues 6- or 7-digit codes; Orange has KYA-compliant merchant subscriptions; Airtel similar) but are issued **per operator and per merchant** and so are not exploitable without merchant cooperation. A **national QR standard does not exist** in DRC as of 2026-06-04; **operator/PSP QR is being trialled in Kinshasa hotels and gas stations** but is not interoperable across networks. Practical MVP addressing: **keyed mobile money number + chosen operator selector**, with the network inferred from MSISDN prefix where possible.

---

## What addressing schemes exist in DRC

### 1. Mobile-money number (MSISDN) — primary

- **Universal across all four DRC operators.** Each user's wallet is bound to their SIM/MSISDN.
- DRC country code: **+243**. (MOKO Afrika's documentation explicitly requires `+243` for phone validation.) **Verified** ([moko-africa-documentation.vercel.app](https://moko-africa-documentation.vercel.app/)).
- MSISDN prefixes are operator-specific (Vodacom, Airtel, Orange, Africell each have their own ranges allocated by ARPTC), so **the payer's app can infer the recipient's operator from the number prefix in most cases** — this is an MVP-friendly UX. **Confidence: High (industry-standard structure)** — verifying current prefix ranges would require ARPTC data; flagged as gap.
- **All aggregators researched accept payouts addressed by MSISDN** (pawaPay, MOKO, Onafriq; DPO's mobile-money payout is bulk-file by MSISDN).

### 2. Operator-specific merchant short codes

- **M-Pesa DRC:** Onboarded organisations receive "a 6-digit or 7-digit code used for customers or businesses to send money to you" ([business.m-pesa.com DRC onboarding page](https://business.m-pesa.com/vodacom-drc/onboarding-your-business-in-drc/)). **Verified.**
- **Orange Money DRC:** Merchants subscribe as "Orange Money merchants - fully KYA compliant" and receive a merchant identifier; the Web Payment API also exposes a merchant-side identifier ([developer.orange.com om-webpay](https://developer.orange.com/apis/om-webpay)). **Verified.**
- **Airtel Money DRC / Afrimoney DRC:** Similar merchant-code patterns are industry standard but **not explicitly documented in public DRC materials** for either. **Inference, Low-Medium confidence.**
- **Implication for MVP:** Merchant short codes require merchant onboarding — which the MVP explicitly excludes — so the MVP cannot rely on them. The payee in v1 is therefore addressed as a **consumer mobile money number on the merchant's own network**, and the merchant receives a normal incoming P2P (peer-to-peer) transfer.

### 3. QR codes — partial, not interoperable, not standardised

- **No DRC national QR standard located** in this research. By contrast, Kenya has KE-QR (2023) and Ghana has GhQR; **DRC's BCC has not announced an equivalent in indexed materials**. **Verified absence (negative finding), Medium-High confidence** ([web search for DRC national QR standard, no positive results](https://payatlas.com/countries/congo-the-democratic-republic-of-the-cd) cites mobile-money / QR-trial activity without naming a standard).
- **Operator/PSP QR exists and is being trialled:**
  - "Hotels and gas stations in Kinshasa are testing QR codes" — third-party news ([Libre Grand Lac — "La République du cash" article on DRC mobile money adoption](https://libregrandlac.com/article/8046/la-republique-du-cash-:-pourquoi-la-rdc-resiste-encore-a-la-revolution-du-mobile-money)). **Medium confidence.**
  - **DPO Pay supports mVISA QR codes** through its gateway ([docs.dpopay.com](https://docs.dpopay.com/llms.txt)).
  - **MOKO Afrika lists "QR code payments for retail"** ([mokoafrika.com homepage](https://www.mokoafrika.com/en)).
  - **Orange Money DRC has a Visa-linked card product** ([orange.cd Orange Money page](https://www.orange.cd/fr/orange-money-new.html)) — separate from QR but in the same scan-and-pay theme.
- **None are interoperable across DRC mobile money networks.** A QR code generated by an M-Pesa merchant is not generally scannable by an Airtel/Orange/Africell consumer app. The lack of a national standard means each path is operator- or PSP-specific.

### 4. Consumer-level cross-network transfer — exists with friction

- **M-Pesa DRC ↔ Orange Money is live as a bilateral interop**: announced 2021 by Vodacash MD Hashim Mukudi on LinkedIn — "Money Transfer between m-pesa DRC and Orange-money now live on a bilateral [agreement]" ([Hashim Mukudi LinkedIn post — 2021](https://ci.linkedin.com/posts/hashim-mukudi-99918317_money-transfer-between-m-pesa-drc-and-orange-money-activity-6801869903801409536-mPMP)) — **self-reported by operator executive, Medium confidence**. **M-Pesa Mikili** is the M-Pesa product branding for cross-operator and international transfers ([vodacom.cd M-Pesa Mikili page](https://www.vodacom.cd/particulier/m-pesa/particulier/mpesa-mikili-recevoir-de-l-argent-de-l-etranger)).
- **Airtel DRC has an interoperability page** linking Airtel Money with Orange Money and M-Pesa ([airtel.cd interop page URL](https://www.airtel.cd/airtelmoney/interopearbilite-avec-orange-money-m-pesa) — URL exists; body opaque to fetch).
- **Friction:** "Interoperability between different telecommunications networks remains partial, with an Airtel Money user unable to always pay an Orange Money merchant without paying for a transfer" (third-party DRC reporting — **Medium confidence**). So consumer-level off-net send works but is slower/costlier than aggregator-bridged routing.

### 5. Other DRC aggregators encountered (out-of-scope but for completeness)

The questions file lists pawaPay, DPO, MOKO, Onafriq; research surfaced additional DRC mobile-money aggregators that handle payee addressing:

- **Flexpay** — multi-MNO + cards collection (M-Pesa, Orange Money, Airtel, Africell) ([Flexpay integration article by David Kathoh on Medium](https://davidkathoh.medium.com/int%C3%A9grez-le-paiement-en-une-minute-dans-votre-application-avec-flexpay-649891d6448d); [David Kathoh — mobile money API access in DRC](https://davidkathoh.medium.com/tout-savoir-sur-lacc%C3%A8s-%C3%A0-l-api-de-paiement-mobile-money-en-rd-congo-3197330fa259)).
- **SerdiPay** — DRC aggregator: Airtel Money, Orange Money, M-Pesa ([serdipay.com](https://serdipay.com/)).
- **Flash / Marchand Flash** — multi-payment-method API (Flash PAY, Visa, bank cards, mobile money) ([flash.one Marchand Flash page](https://www.flash.one/marchand-flash.html)).
- **Maxicash** — DRC PSP with sandbox + WordPress/Shopify plugins (cited by Kathoh).

These could each be evaluated as alternative aggregator paths if the four shortlisted ones don't fit. **Flagged for follow-up; not researched in depth.**

---

## What this means for the MVP's payee UX

1. **The payer keys the merchant's mobile money number.** The app infers the operator from the MSISDN prefix; on ambiguity, the payer selects from a dropdown ("Vodacom / Airtel / Orange / Africell").
2. **The chosen aggregator routes the payout** to that number on that network. The payee is treated as a P2P recipient on their existing operator wallet, no merchant-side action required. *(This describes the earlier no-onboarding path; post-pivot the payee is a registered merchant addressed by till/QR - see the pivot notice at the top.)*
3. **QR is a v2 conversation, not v1**, unless we adopt one PSP's proprietary QR (e.g., mVISA via DPO, MOKO QR) and accept it works only for *that PSP's* merchants, which fights the cross-network proposition.
4. **The MVP wallet selector should default to the inferred operator and let the payer confirm**, since a wrong selection routes to the wrong network and either fails or sends to an unintended recipient on a different network.

## Confidence summary
- **High:** MSISDN-based addressing universally supported; +243 country code; no DRC national QR standard.
- **Medium:** consumer-level M-Pesa ↔ Orange interop is real (and Airtel ↔ Orange ↔ M-Pesa per airtel.cd); MOKO is the only researched aggregator naming all four MNOs; QR is a niche/test phenomenon in DRC.
- **Low:** prefix-to-operator mapping (the technical detail) — would need ARPTC data to confirm current allocations.

## Sources
- mp-001 (M-Pesa DRC merchant short code)
- om-001 (Orange Money merchant subscription)
- am-004 (Airtel interop page URL)
- mk-002 (MOKO API docs)
- xo-001, xo-002 (DRC interop + Kathoh article)
- xo-008: Hashim Mukudi LinkedIn post on M-Pesa DRC ↔ Orange Money interop (2021) — `https://ci.linkedin.com/posts/hashim-mukudi-99918317_money-transfer-between-m-pesa-drc-and-orange-money-activity-6801869903801409536-mPMP`
- xo-009: Libre Grand Lac — DRC mobile money adoption article — `https://libregrandlac.com/article/8046/la-republique-du-cash-:-pourquoi-la-rdc-resiste-encore-a-la-revolution-du-mobile-money`
- xo-010: Flexpay integration article by David Kathoh (Medium) — `https://davidkathoh.medium.com/int%C3%A9grez-le-paiement-en-une-minute-dans-votre-application-avec-flexpay-649891d6448d`
- xo-011: Flash Marchand page — `https://www.flash.one/marchand-flash.html`
- xo-012: SerdiPay homepage — `https://serdipay.com/`
- xo-013: M-Pesa Mikili (Vodacom CD) — `https://www.vodacom.cd/particulier/m-pesa/particulier/mpesa-mikili-recevoir-de-l-argent-de-l-etranger`

[[mpesa]] · [[orange-money]] · [[airtel-money]] · [[afrimoney]] · [[pawapay]] · [[moko]]
