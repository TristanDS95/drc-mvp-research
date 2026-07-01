# Float-netting interoperability model (v2 design brainstorm)

- **Topic / entity:** a **v2 alternative** to routing every transaction through pawaPay: hold a prefunded wallet per MNO and net-settle, to cut the MVP's per-transaction fees.
- **Status:** **Design brainstorm, not yet researched.**
The reasoning below is **inference**, not verified fact.
The "Unknowns to answer" section is the research agenda that decides whether this is viable.
- **Date:** 2026-07-01
- **Relation to MVP:** this **breaks** our v1 "never hold funds / pure pass-through" guideline on purpose.
It is a possible **v2** path *if* the MVP proves out and per-transaction fees become the binding constraint.

## The problem it addresses
The MVP on pawaPay costs **~3.5-5% per cross-network round-trip** (see [fees-and-costs.md](fees-and-costs.md)).
The merchant absorbs that as MDR, which is heavy for low-value tickets.
Negotiating cheaper rates directly with each MNO is slow and is a bottleneck.
This model is an attempt to minimise fees **without** depending on winning bespoke MNO price deals.

## The idea
Hold one "global wallet" per MNO we support (Vodacom M-Pesa, Airtel, Orange).
When a customer pays a merchant, the money lands in **our** wallet on the **customer's** MNO.
We then pay the merchant out of **our** wallet on the **merchant's** MNO.
No cross-network transfer happens per transaction.
Instead, our per-MNO wallets run surpluses and deficits, and we settle only the **net imbalance** periodically (intraday or end-of-day) via pawaPay or a bank/FX desk.

This is a standard **prefunded-float + net-settlement** design - the same mechanism card networks, correspondent banking, and mobile-money clearing switches use.

### Worked example
Customer on Airtel buys from a merchant on Orange.

1. Customer pays into our **Airtel** global wallet (on-net Airtel collection).
2. We pay the merchant from our **Orange** global wallet (on-net Orange payout).
3. Our Airtel wallet is now +X, our Orange wallet is -X.
4. We never did an Airtel-to-Orange transfer for this payment.
5. At settlement time we move only the **net** Airtel/Orange imbalance across, once, in bulk.

## Where the savings come from
1. **Same-network payments collapse from two legs to one.**
pawaPay always does two operator-charged legs even when payer and payee share a network (an M-Pesa-to-M-Pesa payment costs 2.5% + 2% = 4.5% today).
Holding that MNO's wallet lets an on-net payment be a single cheap hop.
With M-Pesa at roughly half the market, same-network pairs are a large share of volume.
2. **pawaPay's ~1%-per-leg margin disappears** on everything (about 2% off a round-trip).
3. **Cross-network bridging shrinks from gross volume to net imbalance.**
If flows across MNOs are roughly balanced, the net that must actually cross networks is far smaller than gross throughput.
4. **Staying in-float avoids cash-out fees.**
We keep money as mobile-money e-float and re-transact it rather than cashing out, dodging the most expensive operation in mobile money.

## Where the savings do NOT come from (be honest about this)
Each on-net collect and each on-net payout **still carries the MNO's own C2B/B2C fee**.
Holding the wallet does not make the operator's tariff free.
Our own [own-aggregator.md](own-aggregator.md) finding put it directly: going direct "eliminates only ~2% pawaPay margin, not operator fees."
So this model reliably kills pawaPay's margin and the same-network double-charge and converts real-time bridging into cheap netting, but it does **not** by itself zero out the base MNO fees.
Whether the net result is compelling depends almost entirely on the **on-net B2C disbursement rate** (see Unknowns #2).

## The "do we need MNO agreements?" question
At scale, yes - but likely not the painful kind.
There are two different "agreements," and only one is the bottleneck:

| Agreement | What it is | Bottleneck? |
|---|---|---|
| **Pricing negotiation** | A bespoke discounted rate per MNO | Yes - the painful part we want to avoid |
| **Business / disbursement onboarding** | Standard KYB to get a B2C account + API at all | Table stakes - a process, not a fight |

Two reasons vanilla consumer wallets do not work at scale:
- The collect-from-many / pay-to-many pattern is the signature of a payment processor, and MNO AML systems **flag and freeze** accounts behaving that way.
- Consumer wallets cap at the KYC tiers (~$3,000 per [kyc-and-onboarding.md](kyc-and-onboarding.md)) and cannot hold the needed float, and B2C payout at volume requires the MNO's disbursement product (an API behind KYB).

The plausible good news: the model may work on **standard published business rates** (no bespoke deal), because the win comes from cutting pawaPay's margin and the double-leg, not from a discount.
And the **EMI licence we need anyway may itself turn MNO onboarding into a routine process** rather than a hard sell - a licensed e-money issuer is a normal counterparty for a disbursement product.

## What we take on
- **Regulatory:** holding third-party funds makes us an e-money issuer.
Per [kyc-and-onboarding.md](kyc-and-onboarding.md) and [own-aggregator.md](own-aggregator.md): BCC EMI licence, roughly **$2.5M paid-up capital**, a **6-18 month** timeline, AML/CFT obligations (CENAREF), and **fund safeguarding** (customer float must be ring-fenced, not lent or commingled).
- **Working capital:** we must **prefund every wallet** before inflows arrive; the float buffer scales with flow imbalance times settlement lag.
- **FX:** DRC runs dual CDF/USD, so held balances carry FX risk - and an FX-spread revenue opportunity.
- **Operations:** we become the switch, owning liquidity management, reconciliation, and settlement risk (a failed payout after we accepted the customer's money is now our exposure, not pawaPay's).

## The upside beyond fee savings
Holding float and touching FX moves us from a thin pass-through toward wallet/bank economics: **float income + FX spread**.
Our [comparables-synthesis](../../03-reports/comparables-synthesis.md) flagged exactly these as the levers a pass-through forgoes.
**IllicoCash** (Rawbank) is a live DRC example of a licensed player monetising held funds (see [illicocash.md](../comparables/illicocash.md)).
The regulation is not only a cost; it is the gate to a higher-margin business.

## Unknowns to answer (the research agenda)
Ranked by how much each one moves the decision.

1. **On-net B2C disbursement rate per MNO - the make-or-break number.**
What does each MNO charge us to pay out on-net to a merchant's wallet from a business/disbursement account?
If low (well under ~1%), the model is compelling.
If ~2%, the edge over pawaPay collapses to roughly its margin.
*Where to look:* MNO business/disbursement product terms; direct sales contact; cross-check against pawaPay's disclosed "MMO portion." Currently partner-gated and **not public** (see [on-net-direct-operator-apis.md](on-net-direct-operator-apis.md)).
2. **On-net C2B collection cost per MNO.**
What does it cost to receive a customer payment into our business wallet, and who bears it (payer vs us)?
Often the payer bears it, which could make our collection leg near-zero.
*Where to look:* MNO merchant/till terms; [fees-and-costs.md](fees-and-costs.md) for the consumer-tariff ceiling.
3. **Float ceiling / account limits.**
Can a licensed business hold the per-MNO balances this needs on standard terms, and what are the transaction/velocity caps on a business disbursement account?
*Where to look:* MNO corporate account terms; BCC rules on EMI float holding.
4. **Net-settlement / rebalancing cost.**
What does it cost to move the net cross-MNO imbalance periodically (via pawaPay, bank, or FX desk), and how often must we do it?
This is the residual "real" fee the model still pays.
5. **Does the EMI licence unlock standard disbursement onboarding** without per-MNO price fights, and does it remove the AML-freeze risk?
*Where to look:* BCC EMI regime; how existing licensed EMIs/PSPs (e.g. IllicoCash) onboard to MNO disbursement rails.
6. **Regulatory scope and timeline confirmation.**
Confirm the EMI capital requirement, licensing timeline, safeguarding rules, and whether a foreign-incorporated entity needs a local partner (carried over from the standing open questions in `_status/progress.md`).
7. **FX exposure and spread.**
Quantify CDF/USD FX risk on held float and whether the spread is a net revenue line or just a hedged cost.

## Decision gate / next step
This is worth pursuing to a **fee model + targeted rate research**, not to build.
Two immediate options:
- **(a)** Answer Unknowns #1 and #2 first (the on-net B2C and C2B rates) - a focused research push, likely requiring direct MNO contact since the numbers are not public.
- **(b)** Build the fee math now: netting savings as a function of monthly volume, cross-MNO imbalance, and the on-net B2C rate, to find the break-even where this beats pawaPay.
Option (b) can proceed immediately from assumptions and shows *how sensitive* the answer is to Unknown #1, which then focuses the research in (a).

[[fees-and-costs]] · [[own-aggregator]] · [[on-net-direct-operator-apis]] · [[kyc-and-onboarding]] · [[illicocash]]
