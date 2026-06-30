# Comparables — fintech products / companies

Comparative research on fintech leaders (African core + select global analogs) to learn **how to structure our app, how to derive value, and how to control feature development & rollout.** Added when the workspace scope broadened (2026-06-09) — see `00-overview/research-goals.md`.

## The three lenses (every profile uses these)
1. **Product structure & architecture** — single app vs super-app; consumer vs merchant vs both; channels (smartphone app / USSD-feature-phone / agent network / web); what they built *first* and how the surface grew; held-balance/wallet vs pass-through.
2. **Monetisation & value capture** — where the money actually comes from (P2P fees, cash-out fees, merchant/MDR, float/interest, FX, lending, bill-pay commissions, subscriptions, ads); who bears the fee (payer vs merchant); how pricing evolved.
3. **Feature rollout & sequencing** — what shipped first, in what order features were added, how they controlled scope/expansion, geographic sequencing, and what unlocked each next step.

Every profile ends with **"What it means for us"** — concrete, transferable implications for our DRC consumer cross-network **pass-through** MVP (on pawaPay, MSISDN payee, no merchant onboarding in v1) **and** our phased **feature-phone/USSD** channel.

## Accuracy standards (per `CLAUDE.md`)
- Source every factual claim; prefer primary (company sites, engineering blogs, regulators, filings) over press.
- Label each claim **verified fact / company self-reported / inference**; never present a company's own figure as independently verified.
- Record **confidence (High/Med/Low) and date accessed**.
- "Not found" is valid. Don't invent metrics.
- Flag where a lesson is market-specific and may not transfer to the DRC.

## Profiles (one file per cluster)
| File | Companies | Why included |
|---|---|---|
| `mpesa.md` | M-Pesa (Safaricom, Kenya) | The canonical operator-led mobile money → super-app + lending arc; USSD + app |
| `wave.md` | Wave (Senegal / Côte d'Ivoire) | Closest market + model analog; low-fee disruptor; app + QR + agents |
| `opay-palmpay.md` | OPay, PalmPay (Nigeria) | Agent-network super-apps; aggressive growth + incentives |
| `mtn-airtel-momo.md` | MTN MoMo, Airtel Money | Operator-led MoMo at continental scale; USSD-first; open API platforms |
| `chipper-flutterwave.md` | Chipper Cash, Flutterwave | Consumer P2P/remittance vs B2B payment rails — monetisation contrast |
| `onafriq-strategy.md` | Onafriq (MFS Africa) | Our closest **structural** analog: a cross-network/cross-border hub (strategy lens; coverage already in `../aggregators/onafriq.md`) |
| `bkash.md` | bKash (Bangladesh) | Feature-phone/USSD + app at massive scale; directly informs our phased USSD channel |
| `global-superapps.md` | GCash (PH), Paytm (India), Mercado Pago (LatAm), WeChat Pay (China) | How payments → super-app → monetisation; sequencing lessons |

## Synthesis
Cross-company synthesis (patterns + what we should adopt) lives in `03-reports/comparables-synthesis.md`.
