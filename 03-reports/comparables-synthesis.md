# What we learned from other fintech companies — in plain language

- **Date:** 2026-06-09
- **What this is:** the plain-English takeaways from studying 8 fintech companies (in `02-findings/comparables/`), with a source on every claim.
- **How sure we are:** **Medium.** Our researchers could search the web but couldn't open full pages, so they worked from search-result summaries. Most company figures are self-reported, not audited. Each claim links to its source so you can check it.

> **Reframed 2026-06-11 (team pivot to a merchant-facing MVP).** The comparables and their lessons are unchanged, but two framing points are corrected to match the pivot: (1) **our fee is a merchant discount rate (MDR) the *merchant* absorbs** — the customer pays the sticker price — so our closest comparables are **merchant-fee** models (Flutterwave, MoMo/MTN merchant, M-Pesa Pochi/Lipa), **not** Wave's send-fee; and (2) **merchant acceptance IS the MVP** (the ~40 gas stations are the launch wedge), not a deferred "version 2." The USSD-first / feature-phone lesson already matches the pivot and is kept. See [`00-overview/product-summary.md`](../00-overview/product-summary.md).

## A few words you'll see (defined once)
- **Pass-through** — our chosen model: the app **never holds your money.** It flows straight from sender → our payments partner (pawaPay) → receiver. We're a messenger, not a bank.
- **Float** — interest a company earns on customer money that's sitting in its accounts.
- **Cash-out** — turning mobile-money into physical cash at a local agent.
- **Agent** — a corner shop/kiosk that swaps cash for mobile money and back.
- **Merchant fee** — the cut a payment company takes when a *shop* receives a payment.

---

## The one big lesson

**Sending money earns almost nothing. The money is in what comes *after* sending.**

Every one of the 8 companies treats the basic "send money" feature as a way to win customers and gather data — **not** as a way to make profit. They make their real money on three things that come later: **shops paying to accept payments, lending, and currency exchange.**

- WeChat Pay's payment fee is tiny (~0.6%) and it earns from lending and wealth products instead ([global-superapps.md](../02-findings/comparables/global-superapps.md)).
- M-Pesa, GCash, Paytm, and Mercado Pago all make their biggest profits from **lending**, not transfers ([global-superapps.md](../02-findings/comparables/global-superapps.md)).
- bKash makes its money on **cash-out fees** (1.85% every time someone withdraws cash), not on sending ([bKash cash-out page](https://www.bkash.com/en/products-services/cashout)).

**Why this matters for us:** our pass-through model gives up the money-makers that need a *held balance or agents* — float, cash-out fees (we hold no money and run no agents). But the pivot puts us **squarely on the one lucrative line a pass-through *can* charge: the merchant fee (MDR)** — the cut a payment company takes when a *shop* receives a payment. That is now our **primary** revenue line, not a deferred one, because merchant acceptance is the MVP. We still start with fewer levers than the deposit-taking super-apps (no float/lending of our own money), so currency exchange and later partner-bank lending remain the growth levers — but the merchant fee is the day-one model, and most of this document is about pricing and defending it.

---

## Where each company *actually* makes its money — and can we copy it?

| Company | How it really makes money | Can our merchant-acceptance pass-through app copy it? |
|---|---|---|
| **M-Pesa** (Kenya) | Cash-out fees, float, and lending (run with partner banks) ([CGAP on M-Shwari](https://www.cgap.org/sites/default/files/Forum-How-M-Shwari-Works-Apr-2015.pdf)); merchant acceptance via **Lipa na M-Pesa / Pochi la Biashara** | **Not the cash-out/float** (we hold no money). **The merchant-fee line — YES, that's our model.** **Lending — yes, later**, via a partner bank (see below). |
| **bKash** (Bangladesh) | Cash-out fee 1.85% every withdrawal; float ([bKash](https://www.bkash.com/en/products-services/cashout)) | **No** — needs a held balance + agent network we don't have. |
| **OPay / PalmPay** (Nigeria) | Interest on deposits, lending, shop card-machines — free transfers are a loss-leader paid for by investors ([Technext: "the real price of free transfers"](https://technext24.com/2025/11/11/real-price-of-free-fintech-transfers/)) | Float/lending **no** (deposit licence + agents). The **shop card-machine / merchant-fee** part **yes — it's our model.** |
| **MTN / Airtel MoMo** | Lending/advances + **merchant fees**; plain transfers earn ~0 ([mtn-airtel-momo.md](../02-findings/comparables/mtn-airtel-momo.md)) | **The merchant-fee part — YES, that's our model** (a close comparable: MoMo merchant acceptance). Lending later. |
| **Chipper Cash** | **Currency-exchange margin (~55% of revenue)**, plus crypto/stock trading and card fees — sending is free ([how Chipper makes money](https://productmint.com/how-does-chipper-cash-make-money/)) | **Currency exchange — YES** (see below). |
| **Flutterwave** | Charges the **merchant** ~2% to accept payments ([Flutterwave breakdown](https://research.contrary.com/company/flutterwave)) | **YES — this is the closest model to ours:** a merchant fee (MDR) the shop absorbs on each payment. (Our cost floor is higher — 3.5–5% on DRC mobile money — so our MDR must be higher than Flutterwave's ~2%.) |
| **Wave** (Senegal) | Charges **~1% to send**; cash-out is **free** ([Wave site](https://www.wave.com/en/); [Rest of World](https://restofworld.org/2022/how-wave-is-disrupting-francophone-africas-mobile-money-market/)) | A **consumer send-fee**, which is **not** our model — we charge the *merchant* (MDR), not the sender. Useful only as a pricing-discipline datapoint, not our analog. |
| **Onafriq** (cross-border hub) | Many tiny fees across huge volume + currency exchange + card services ([onafriq-strategy.md](../02-findings/comparables/onafriq-strategy.md)) | The "stack several small fees" idea — but they have scale we won't for years. |

> **Correction worth noting:** an earlier brief said Wave is "free to send, 1% to cash out." It's the **opposite** — Wave charges **~1% to send** and makes **cash-out free** (free withdrawals are how it lured people off the expensive incumbents) ([Wave site](https://www.wave.com/en/)).

---

## So how would *we* make money?

Given we hold no money and run no agents, our levers are:

1. **The merchant fee (MDR) — our primary, day-one revenue line.** The merchant absorbs a fee on each payment they accept; the customer pays the exact sticker price. This is the lucrative line a pass-through *can* charge (Flutterwave and MoMo/MTN merchant acceptance both run on it), and merchant acceptance is the MVP, so it's the business from launch — not a customer-acquisition give-away. **Pricing is the open question:** our cost floor is the pawaPay round-trip (3.5–5%), the current MDR placeholder is **1% — below cost** — so a viable MDR is ~5–7%+. Whether merchants accept that take rate (vs. cash) is the thing to test with the launch wedge.
2. **Currency exchange (US$ ↔ Congolese franc).** A strong near-term opportunity we *uniquely* have: the DRC uses both currencies daily, and our flow already crosses currencies. Chipper makes most of its money this way ([Chipper](https://productmint.com/how-does-chipper-cash-make-money/)). **Cross-currency payments — not same-currency ones — may be where extra margin lives.** (Needs checking: how big a margin is realistic, and what the DRC's central bank allows.)
3. **Lending later, using a partner bank's money — not ours.** M-Pesa and bKash don't lend their own money; banks (NCBA/KCB for M-Pesa; City Bank for bKash) carry the risk and the licence, while the app provides the customer and the data ([CGAP on M-Shwari](https://www.cgap.org/sites/default/files/Forum-How-M-Shwari-Works-Apr-2015.pdf)). So we should **keep clean records of which customers pay which merchants now**, so a merchant-lending / cash-advance product is possible later without us becoming a bank.

**One thing we should NOT do: race to the bottom on the MDR to win merchants fast.** OPay, PalmPay, Wave, and Chipper all subsidised pricing to grow — but they could afford it because investors funded it *and* they had other money-makers to fall back on ([Technext](https://technext24.com/2025/11/11/real-price-of-free-fintech-transfers/)). We have neither, and our cost floor is high (3.5–5%), so pricing the MDR *below* cost to land merchants would be far riskier for us than it was for them.

---

## How they build the app

- **Start simple, add later. Don't build a "do-everything" app on day one.** OPay built a sprawling super-app (even ride-hailing) in 2019, then **shut most of it down in 2020** to refocus on payments ([opay-palmpay.md](../02-findings/comparables/opay-palmpay.md)). Focus first.
- **Keep the basic phone (USSD) version working — it's not a second-class option.** M-Pesa launched in 2007 with no app at all (just the phone menu), and the app only arrived in **2021 — 14 years later** — and even now most people don't use it ([M-Pesa history, World Bank](https://documents1.worldbank.org/curated/en/638851468048259219/pdf/543380WP0M1PES1BOX0349405B01PUBLIC1.pdf)). bKash ran on the basic-phone menu (`*247#`) for ~7 years before its app ([bKash app launched, Daily Star](https://www.thedailystar.net/business/bkash-app-launched-1576999)). **Lesson:** make "send to a number" work fully on a basic phone; the smartphone app should *add* extras, not be the only door in.
- **A close cousin of our idea already exists and works.** M-Pesa's "Pochi la Biashara" lets a small seller receive customer payments separated from personal funds via a **lightweight till**, with the customer paying by the operator's own menu ([Safaricom: Pochi la Biashara](https://www.safaricom.co.ke/media-center-landing/frequently-asked-questions/pochi-la-biashara)). Our model is the same shape — **merchant gets a till, customer pays by USSD, money settles to the merchant** — with a *light* merchant onboarding step (settlement account + till/short-code) rather than zero sign-up. (Note: this corrects the earlier "no merchant onboarding" framing — onboarding merchants is now the MVP.)

---

## The order they add features

They almost all follow the same sequence:

**1. Send money** (win users) → **2. Let shops accept payments** → **3. Lending** (the real profit) → **4. Savings/insurance/more.**

**Where we enter differs from the template:** we **start at step 2 (merchant acceptance)** rather than step 1 (consumer send), because merchant acceptance is our MVP and the ~40 gas stations are the wedge. A consumer send/wallet product is the *much-later* step for us, not the opener. Steps 3–4 (partner-bank lending, more) still come after. Two rules from the comparables that still apply to us:
- **Lending comes last, and through a partner bank** (capital-light, less regulation) — M-Pesa and bKash both did this ([CGAP](https://www.cgap.org/sites/default/files/Forum-How-M-Shwari-Works-Apr-2015.pdf)).
- **Win one market deeply before expanding.** Wave's home market (Senegal) looks like a real business; its rush into other countries lost money and led to layoffs ([wave.md](../02-findings/comparables/wave.md)). Depth before breadth.

---

## The biggest danger for us

**We depend on the very companies we'd compete with.** Every payment we move rides on Vodacom/Airtel/Orange's rails. If an operator decides we're a threat, it can make our life hard.

This already happened to Wave: when Wave undercut Orange in Senegal, **Orange not only cut its own prices but blocked Wave from selling Orange airtime for over a year**, until regulators stepped in ([Rest of World](https://restofworld.org/2022/how-wave-is-disrupting-francophone-africas-mobile-money-market/)). **We're more exposed than Wave was**, because we need operator access for *both* halves of every payment.

A related warning: if the operators (or the DRC central bank) make cross-network payments easy on their own, our main selling point weakens. So our staying power has to come from **a better app experience, basic-phone reach, and bundling** — not from "owning" the connection between networks ([onafriq-strategy.md](../02-findings/comparables/onafriq-strategy.md)).

**One silver lining:** renting rails instead of owning them is also *safer*. India's Paytm built its own bank, lost the licence in 2026, and survived only because it could fall back on partner banks ([global-superapps.md](../02-findings/comparables/global-superapps.md)). Our "we don't own the rails" choice can be pitched as smart risk-reduction, not just a shortcut.

---

## The short version (if you read nothing else)

1. **The merchant fee (MDR) is our day-one model** — the merchant absorbs it, the customer pays the sticker price. (Flutterwave / MoMo merchant acceptance are the close analogs; Wave's *consumer* send-fee is not our model.)
2. **Our realistic earners:** the **merchant MDR** now (price it *above* the 3.5–5% cost floor — the 1% placeholder is below cost), **currency exchange (US$↔CDF)** now, and **partner-bank lending** later (a merchant cash-advance is the natural fit).
3. **Don't price the MDR below cost to grab merchants** — we can't subsidise it the way the venture-funded players could, and our cost floor is high.
4. **Treat the basic-phone (USSD) version as first-class** — the customer pays a merchant by scanning a `tel:` QR or dialing a till, no app needed. (This lesson already matched the pivot.)
5. **We enter at merchant acceptance, not consumer send** — that's the MVP and the 40 gas stations are the wedge; lending comes later via a bank partner; a consumer app is much-later.
6. **Biggest risk:** we depend on the operators we compete with (ask Wave). Defend with experience, reach, and bundling — and keep clean records of who pays which merchant now, for a future lending product.

## Where the full detail and sources live
- Plain claims above link to the source directly.
- Full company profiles (with all ~165 sources): [`02-findings/comparables/`](../02-findings/comparables/).
- Source index: [`04-sources/sources.md`](../04-sources/sources.md) → "Comparables."
