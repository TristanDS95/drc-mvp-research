# Glossary

- **Mobile money** — a payment account tied to a phone number, run by a telecom, not requiring a bank account.
- **Operator / MNO** — the telecom running a mobile money service (Vodacom–M-Pesa, Orange–Orange Money, Airtel–Airtel Money, Africell–Afrimoney).
- **Aggregator / PSP** — a provider connecting to many mobile money networks at once through one integration (we rent **pawaPay**).
- **Merchant** — a registered business that accepts payments through the app (the app's primary user). Has a settlement account and a **till / short-code**.
- **Customer** — the person paying a merchant. Needs only a mobile-money-enabled phone; **no app or internet** of their own.
- **Collection (cash-in / debit / pull)** — taking funds from the **customer's** mobile money account.
- **Settlement / payout (disbursement / push)** — sending funds to the **merchant's** mobile-money account. In our model this is **instant, per transaction** (pure pass-through).
- **Cross-network / off-net** — customer and merchant on different mobile money services; we bridge them.
- **MDR (merchant discount rate)** — our fee, **absorbed by the merchant**: the customer pays the sticker price, the merchant nets `amount − fee`. (Placeholder 1%; real pricing must exceed the ~3.5–5% pass-through cost.)
- **Till / short-code** — the code a customer dials to pay a merchant (e.g. `*123*1001#`): our USSD shortcode + the merchant's `short_code`.
- **USSD** — interactive operator menus over the cellular **signalling** channel (no data). Our **primary customer channel**: the customer dials the till (or scans a QR that pre-fills it) to **initiate** a payment, then authorises with their PIN via the operator's own prompt (**STK / USSD push**).
- **QR dial-through** — a merchant QR that encodes a `tel:` USSD string (`tel:*123*1001%23`); scanning it on Android pre-fills the dialer with the till, offline. iOS / feature phones dial the printed till manually.
- **Customer-initiated vs merchant-initiated** — *customer-initiated* (primary): the customer scans/dials the merchant's till. *Merchant-initiated* (fallback): the merchant enters the customer's number and pushes a charge.
- **Pass-through / no custody** — we never hold a balance; money flows customer → pawaPay → merchant.
- **Settlement** — when and how funds actually move and reconcile (here: instant, per transaction).
- **KYC** — identity verification; required for the **merchant** at onboarding (the customer leg rides existing SIM registration).
