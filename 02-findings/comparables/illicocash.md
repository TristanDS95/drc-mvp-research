# Finding: IllicoCash (Rawbank) — DRC bank-backed consumer super-wallet

- **Topic / entity:** IllicoCash — the mobile-money wallet / super-app operated by Rawbank in the DRC.
- **Question it addresses:** Is IllicoCash a competitor to our merchant-acquiring cross-network app, and if so how does it actually differ? (It is the DRC product most likely to be *called* our competitor.)
- **Date researched:** 2026-07-01
- **Confidence:** **Medium** — the official site was read directly (primary); several feature and positioning claims come from search-indexed summaries of the Rawbank and app-store pages rather than a direct re-fetch, and the Thunes press release returned HTTP 403. Pricing is **not found**.
- **Claim type:** mix — labelled inline. Company/marketing framing is flagged; nothing here is independently audited.

## Claim
IllicoCash is a **consumer-facing** digital wallet run by **Rawbank** (a major DRC commercial bank). It is a broad "super-wallet" — domestic and cross-border transfers, virtual cards, bill pay, currency exchange, agent cash-out, and **merchant QR payments** — but it is a **closed-loop, bank-backed wallet**: to pay a merchant, the *payer* must be an IllicoCash user with funds loaded in that wallet. That makes it a **different-customer, different-model** product from ours, not a like-for-like rival.

## Evidence
**What it is / who runs it.**
- A mobile banking / "fintech" app for the DRC that lets customers transact without visiting a branch; operated by **Rawbank** ("illicocash and Rawbank" throughout the official site). **[verified fact, Med]** — [illicocash.com](https://illicocash.com/en/).
- Rawbank is the DRC's largest commercial bank, which is why IllicoCash can hold customer balances and operate as a licensed bank product (unlike a pure pass-through). **[inference from Rawbank being the operator, Med]**

**Features (from the official site, direct read).**
- **Domestic transfer** to "anyone with a simple phone number." **International transfer** to recipients worldwide, plus receiving funds. **[self-reported, Med]**
- **Virtual cards** for online shopping / subscriptions; **currency exchange** (buy/sell forex); **bill pay** incl. phone recharge and TV; **agency-banking** network for deposits/withdrawals; **e-ticket** service. **[self-reported, Med]**
- **Multi-currency** accounts in **USD and CDF**; balance checks and statements; instant account creation. Interface in **English and French**. **[self-reported, Med]** — [illicocash.com](https://illicocash.com/en/).

**Additional features (from search-indexed Rawbank / app-store summaries — not directly re-fetched).**
- **Scan a QR to pay at merchants** — the feature that overlaps with us. **[self-reported, Med-Low]**
- **Cardless ATM withdrawal** (geolocation), **cash-out at agent points**, **load wallet via any Visa/Mastercard**, **link Rawbank accounts** to the wallet, **currency exchange between wallets**. **[self-reported, Med-Low]**
- Described as **"the first mobile wallet in the DRC to allow cross-border mobile money transfers in and out of the DRC."** **[company self-reported, Low]** — cross-border rails reportedly powered by a **Thunes** partnership (Thunes press release could not be fetched — HTTP 403 — so this is via search summary only).
- Service available in **French and Kituba (Kikongo)** per one summary (the site itself shows EN/FR). **[conflict — see caveats]**

**Positioning.** The site frames the product for **general consumers**; **no merchant-onboarding-as-a-product** detail was found (merchant QR is a consumer feature, not a merchant acquiring product). **[verified absence, Med]**

## Competitive read — how it relates to us
- **Why it looks like a competitor:** it offers merchant QR payments, so a shop *can* be paid through it, and it is a polished, bank-backed, well-known DRC brand.
- **Why it isn't a like-for-like rival (the key difference):** IllicoCash is a **closed-loop wallet** — the paying customer must **be an IllicoCash user with money loaded in the wallet.** Our model is the inverse: the customer pays from **whatever they already have** (their M-Pesa / Airtel / Orange balance) with **no new app**. IllicoCash extends a shop's reach only among IllicoCash users; we extend it across *everyone with any mobile-money account.*
- **Different customer:** IllicoCash sells to the **shopper** and treats acceptance as a side feature; we sell to the **merchant** and make cross-network acceptance the whole product.
- **Different regulatory model — and directly relevant to our float brainstorm:** IllicoCash **holds customer funds** as a bank product. That is exactly the licensed, funds-holding posture our current pass-through design avoids — and a live DRC example of a bank operating that model. Worth studying if we ever explore a **held-float / global-wallet** architecture (see [[fees-and-costs]] and the float-model discussion). **[inference, Med]**
- **Threat vector to watch:** if Rawbank pushed IllicoCash into **open, network-agnostic merchant acceptance**, it has the bank licence, brand, and consumer base to do it. Monitor; not an immediate threat. **[inference, Med]**

## Source(s)
- [IllicoCash official site (EN)](https://illicocash.com/en/) — accessed 2026-07-01 — primary, direct read — reliability Med (marketing copy) — **ic-001**
- [Rawbank — illicocash product page](https://rawbank.com/en/banque-a-distance/illicocash/) — accessed 2026-07-01 — primary (operator) — via search index, not directly re-fetched — Med-Low — **ic-002**
- [IllicoCash on Google Play](https://play.google.com/store/apps/details?id=com.s2m.rawbank.views) — accessed 2026-07-01 — primary/self-reported (app listing) — Med-Low — **ic-003**
- [IllicoCash on the App Store](https://apps.apple.com/cd/app/illicocash/id1245099231) — accessed 2026-07-01 — primary/self-reported — Med-Low — **ic-004**
- [Thunes × Rawbank partnership press release](https://www.thunes.com/news/thunes-rawbank-partner-to-power-international-mobile-money-transfers-with-illicocash/) — accessed 2026-07-01 — self-reported — **HTTP 403, not fetched**; cross-border claim rests on the search summary only — Low — **ic-005**

## Notes / caveats / contradictions
- **Direct-read vs. search-summary:** only [illicocash.com](https://illicocash.com/en/) was read in full. The **merchant-QR, cardless-ATM, "first cross-border wallet," and Kituba-language** claims come from search-indexed summaries and should be re-verified against the primary pages before any decision leans on them.
- **Language conflict:** the official site presents an **English + French** interface; a separate summary claims **French + Kituba (Kikongo)**. Both may be true (app UI vs. marketed languages) — unresolved.
- **Pricing: not found.** No consumer or merchant fee schedule was located.
- **Scope note:** this profile sits in `comparables/` but is a genuine **in-market DRC competitor-adjacent** product, not only a "learn-from" analog — read the "Competitive read" section as its main deliverable.

[[fees-and-costs]] · [[payee-identification]] · [[mpesa]] · [[pawapay]]
