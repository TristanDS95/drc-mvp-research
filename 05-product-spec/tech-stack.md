# Tech stack (v1 MVP)

Concrete stack choices with rationale for the **merchant-acquiring** MVP. One decision is pending (backend language) and laid out in detail below; the **USSD bearer** is a first-class vendor for the customer-USSD channel.

- **Last updated:** 2026-06-11
- **Status:** mobile, infra, and vendor bundle settled (USSD bearer added); backend language pending

> **Direction (2026-06-11): merchant-facing.** The mobile app is the **merchant** app; the **customer pays over USSD** (scan the merchant's QR / dial the till) or via a **merchant-initiated charge-by-number**. This adds one vendor to the bundle — a **USSD bearer** (shortcode + gateway) — and reframes the "our flow" rationale below around merchant acquiring + the customer-USSD channel. The money rails (pawaPay) and the rest of the stack are unchanged. **A working backend reference exists in Python + FastAPI** (`../drc-pay/services/api/`); treat it as a strong reference, not a locked final call (see Pending below).

> **Subject to revision when team grows.** This stack reflects current decisions with the existing 4-person team. New engineers will have language preferences (especially on backend) and may have strong views on framework/ORM/queue choices. The structural decisions (RN+Expo merchant app, AWS Cape Town, Postgres+Redis, the vendor bundle incl. the USSD bearer) are durable; the backend-language decision is intentionally still open precisely because the backend engineer's preference matters more than ours. Smaller within-language choices (Fastify vs NestJS; Prisma vs Drizzle) should defer to the engineer doing the work.

## Settled

### Mobile framework (merchant app): React Native + Expo (managed workflow)
- **Choice:** React Native with Expo managed workflow; EAS Build for releases. This is the **merchant** app (the customer needs no app — they pay over USSD).
- **Why:** single codebase for iOS + Android with one mobile engineer (the 4-person team's resourcing reality); Expo accelerates setup significantly; modern RN performance is more than adequate for the merchant flow (render the payment QR + the live payments feed + the charge-by-number form + receipt display) even on low-end Transsion phones. (QR generation/serving is server-side via `segno`, so the app just displays an SVG.)
- **Trade-off acknowledged:** the [research](../02-findings/app-development/tech-stack.md) found that flagship African fintech apps (Wave, OPay, Kuda) use native (Kotlin + Swift). We're choosing team velocity over the flagship pattern. Revisit if performance becomes a real constraint or if we hire a second mobile engineer.
- **Mitigations:**
  - Use Hermes JavaScript engine (default in modern RN) for faster startup.
  - Use the `react-native-mmkv` library for fast local storage (10x faster than AsyncStorage).
  - Test continuously on a real Tecno or Infinix phone; don't trust simulator performance.

### Cloud region: AWS Cape Town (af-south-1)
- **Choice:** AWS Africa (Cape Town); single region for v1.
- **Why:** ~80–100 ms latency to Kinshasa (geographic inference, not formally measured; vs ~150–200 ms from Europe). Caveat: DRC in-country backhaul is poor (>150 ms cited in-country), so last-mile latency likely dominates the server-region delta — the in-region edge is real in *direction* but smaller in practice than the raw figure implies. Same African-region rationale Wave used (Wave is on GCP Johannesburg, comparable). Fully mature region (opened 2020).
- **Services anticipated:** ECS Fargate for the API (note: AWS App Runner is **not** available in af-south-1 / Cape Town — open roadmap request since 2022 — so Fargate is the managed-compute choice); RDS Postgres; ElastiCache Redis; S3 for receipts/assets; SES or SNS for emails (rare); EventBridge or SQS for webhook retries; CloudWatch + Sentry for observability.
- **Backup posture:** automated RDS snapshots; S3 cross-region replication to Frankfurt for disaster recovery; no multi-region production in v1 (premature).

### Vendor bundle
| Concern | Vendor | Why |
|---|---|---|
| **Payment rail (money movement)** | **pawaPay** | Rented rails for both legs (collect from the customer, settle to the merchant) across Vodacom/Airtel/Orange; webhooks signed RFC-9421/ECDSA-P256; the built integration speaks it ([pawapay-api-deep-dive.md](../02-findings/aggregators/pawapay-api-deep-dive.md)) |
| **USSD bearer (customer channel)** | **Africa's Talking** (front-runner) — **Infobip** (incl. via Telerivet) alternative | The customer-initiated USSD/QR channel. AT confirms **dedicated USSD codes in the DRC** and uses the exact `sessionId/phoneNumber/serviceCode/text` → `CON/END` model our `ussd/` code already speaks; going live is a **thin adapter** at the `/ussd` boundary. Bearer transports the **menu** only — money still moves via pawaPay (so its per-session fee is **additive**). ([ussd-gateway-providers.md](../02-findings/cross-cutting/ussd-gateway-providers.md)) |
| Merchant SMS / OTP | **Africa's Talking** | Merchant-app sign-in OTP. DRC coverage confirmed ([ad-030](../04-sources/sources.md)); Termii does not cover DRC; cheaper for African numbers than Twilio. (Same vendor can supply SMS **and** the USSD bearer — one relationship.) |
| **Business KYC** (when added) | **Smile ID** | Merchant onboarding KYC (business/owner verification — depth flagged). DRC document support confirmed; pan-African coverage; Mastercard partnership signals durability |
| Error tracking | **Sentry** | Cross-platform standard; works for both RN and the backend; battle-tested |
| Product analytics | **PostHog** | Open-source, self-hostable, session replay + feature flags included; cheaper than Mixpanel at scale; product-led |
| Push notifications | **Expo Push** + **Firebase Cloud Messaging** (Android) / **APNS** (iOS) | Free tier covers v1; notifies the **merchant** of incoming payments/state; Expo's push abstraction works for RN |
| Merchant receipt share | System share sheet | No third-party SDK needed for v1; the merchant shares a receipt (e.g. via WhatsApp) as a styled image. (The customer also gets the operator's own confirmation SMS independently.) |
| QR generation | **segno** (server-side) | Renders each merchant's `tel:` USSD dial-through as a printable SVG QR; the app just displays it |
| CI/CD | **GitHub Actions** + **EAS Build** | Standard; GitHub Actions for backend, EAS for mobile builds |
| Secrets management | **AWS Secrets Manager** | Native to chosen cloud; integrated with IAM |

### Database & data layer
- **Primary store:** Postgres (RDS managed). Strong fit for the financial ledger; the SQL pattern for double-entry bookkeeping is well-known.
- **Cache:** Redis (ElastiCache managed) for sessions, rate limits, and operator-status windows.
- **Migrations:** language-specific tooling — chosen with backend language (see Pending below).
- **Backups:** automated daily snapshots; PITR enabled; restore tested every quarter.

### Observability targets (non-functional)
- API p95 latency under 300 ms for non-pawaPay endpoints; under 2 s for endpoints that call pawaPay.
- Error rate under 0.5% of requests at steady state.
- Mobile cold start under 2 s on a mid-range Android (e.g., Tecno Spark).
- Crash-free sessions above 99.5%.
- Reconciliation lag under 1 hour at steady state; under 15 minutes within a year.

---

## Pending decision: Backend language

Two strong options. The choice has real consequences but no wrong answer. Below: an honest side-by-side, then a recommendation, then a "factors that would tip the choice" list.

### Option A — Node.js + TypeScript (Fastify or NestJS)

**What it looks like:**
- Fastify for high-throughput HTTP (faster than Express; smaller than NestJS); or NestJS if we want more structure (decorators, DI, modules).
- Zod or Valibot for runtime validation.
- Prisma or Drizzle for the ORM; raw SQL for the ledger.
- Vitest for testing.
- Same TypeScript across mobile and backend — types defined once and imported on both sides.

**Strengths:**
- **Single-language stack with React Native.** Type contracts, validation schemas, and shared utilities all in one language. For a small team, this is meaningful velocity.
- **Excellent webhook / I/O performance.** Node's event loop is built for the workload (lots of HTTP in/out, mostly waiting on pawaPay or the database).
- **Largest hireable pool globally.** TypeScript backend is the dominant pattern in modern fintech startups.
- **Familiar to the mobile engineer.** They can review backend PRs sensibly; backend engineer can fix small mobile bugs.
- **Speed to ship.** Plausibly 10–15% faster MVP timeline because of language consolidation.

**Weaknesses:**
- **No language-native runtime type safety.** Zod / Valibot are the workaround; they add boilerplate and a small runtime cost.
- **Less ergonomic for future fraud / ML work.** If we ever add fraud scoring or anomaly detection in-house, Python is the natural home.
- **The flagship-fintech precedent doesn't go this way.** Wave runs Python; OPay runs Java; Kuda runs Java/Kotlin. None publishes a Node backend.

**Hireability in DRC / francophone Africa:** good (TypeScript is taught in many CS programs and bootcamps), but less established than Python.

---

### Option B — Python + FastAPI (with mypy)

**What it looks like:**
- FastAPI as the framework: ASGI, Pydantic-first, async-native, auto-generates OpenAPI spec.
- mypy in strict mode for static type checking (the Wave pattern).
- Pydantic v2 for request/response models, validation, serialization.
- SQLAlchemy 2.0 + Alembic for ORM and migrations; raw SQL for the ledger.
- uvicorn + gunicorn for production workers.
- pytest for testing.
- **OpenAPI → TypeScript codegen** to reproduce the type-sharing benefit Node would give us "for free" — generated client types in the RN app stay in sync with backend changes.

**Strengths:**
- **Matches Wave's publicly documented stack** — Python + mypy + GraphQL (we'd likely choose REST + Pydantic over GraphQL, but the language pattern is the same). Strong precedent in the closest-to-us franchise.
- **Best-in-class type safety in practice.** Pydantic enforces types at runtime on every API boundary; mypy enforces them at code-review time. The combination catches an entire class of bugs (malformed webhooks, mistyped DTOs) that Node requires Zod-style discipline to avoid.
- **Auto-OpenAPI generation** — FastAPI hands you the spec for free. Useful for clients, tests, and contracts.
- **Strongest ecosystem for future fraud / ML / data work.** If we want to score risk, detect velocity-anomalies, or build a fraud-rules engine, Python is the obvious home and avoids a polyglot future.
- **Hireability in francophone Africa skews Python** — Python is heavily taught across French-speaking African universities.

**Weaknesses:**
- **Not type-shared with React Native.** Mitigated by generating a TypeScript client from the OpenAPI spec — but it's a build-step rather than a free import. Recovers ~70% of the type-sharing benefit.
- **Slower runtime than Node** for CPU-bound work; comparable for I/O-bound (which is most of our workload).
- **Two languages on the team.** Mobile and backend engineers can't trivially review each other's PRs.

**Hireability in DRC / francophone Africa:** very strong (Python is widely taught and popular).

---

### Honest side-by-side

| Factor | Node.js + TypeScript | Python + FastAPI |
|---|---|---|
| Speed to ship MVP | **Faster (by ~10–15%)** | Slightly slower (OpenAPI codegen setup adds 2–3 days) |
| Single-language stack with RN | **Yes** | No (mitigated by codegen) |
| Runtime type safety | Add Zod (boilerplate) | **Built-in (Pydantic)** |
| Static type safety | TypeScript (compile-time) | **mypy strict (compile-time)** |
| Webhook performance | **Excellent** | Excellent (FastAPI async) |
| Future fraud / ML | Awkward (polyglot or rewrite) | **Best-in-class** |
| Closest flagship precedent | OPay (Java), Kuda (Java) — neither is Node | **Wave (Python+mypy)** |
| Hireability in DRC | Good | **Stronger** |
| Team conceptual integrity (mobile + backend) | **High (one language)** | Medium (two languages, both common) |

### Recommendation

**Python + FastAPI** edges out Node/TS for this specific project — and the **built reference backend is already Python + FastAPI** (`../drc-pay/services/api/`: channel-agnostic `domains/` + `application/`, thin `http/` and `ussd/`, Pydantic schemas, the simulator rail). That is a working reference, **not** a final lock-in: the decision still defers to the hired backend engineer (see triggers below). The reasons Python edges out:
1. The closest-to-us flagship precedent (**Wave**, the francophone-Africa unicorn we'd most want to emulate operationally) runs **Python + mypy**. That's not a coincidence — Pydantic's runtime validation prevents an entire class of bugs that webhook-heavy fintech work generates.
2. **Pydantic + mypy together** provide stronger end-to-end type safety than TypeScript + Zod, with less hand-rolled boilerplate.
3. **Fraud / ML work** is much more natural in Python. The MVP doesn't need it; v2/v3 likely will, and the rewrite cost is real if we start with Node.
4. **Francophone-Africa hireability** is meaningfully better for Python than TypeScript.

**Node/TypeScript is the right answer if any of these are true for you:**
- "Ship MVP as fast as possible" is the absolute top priority — Node saves ~1–3 weeks.
- The backend engineer is much stronger in TypeScript than Python.
- You expect the mobile engineer to do meaningful backend work (rare but possible on a 4-person team).
- The team feels more confident shipping a Node stack than a Python stack.

If those don't apply, lean Python.

### Decision triggers (revisit if these change)
- If a strong Python-skewing backend engineer joins → Python becomes obvious.
- If a strong TypeScript-skewing engineer is already on the team → Node becomes obvious.
- If timeline pressure intensifies and shaving 1–3 weeks matters → Node.
- If we hire a francophone-Africa-based engineer for the backend → Python becomes obvious.
- If the v2 roadmap acquires explicit fraud-ML or data work → Python is the clear right answer.

---

## Open / smaller decisions
These are detail-level and can be settled by the backend engineer in Phase 0 Week 2 once backend language is chosen.

- **Specific framework within the chosen language** (Fastify vs NestJS; FastAPI is already specified for Python).
- **ORM choice** (Prisma vs Drizzle for Node; SQLAlchemy 2.0 for Python).
- **Queue / job runner** (BullMQ for Node; Celery or Arq for Python).
- **GraphQL vs REST.** Default: REST. GraphQL is an option (Wave uses it) but it's complexity for the v1 surface.
- **Authentication library** (Passport / Lucia for Node; Authlib / fastapi-users for Python).
- **USSD bearer pick + DRC rate card.** Africa's Talking is the front-runner (Infobip the alternative); confirm per-operator DRC coverage and a real rate card on contact (the only DRC figures found are secondary), and confirm the aggregator shortcode lead time. ([ussd-gateway-providers.md](../02-findings/cross-cutting/ussd-gateway-providers.md))

## Sources
- [tech-stack.md](../02-findings/app-development/tech-stack.md) — research on Wave's Python stack, OPay/Kuda stacks, mobile framework distribution
- [ussd-gateway-providers.md](../02-findings/cross-cutting/ussd-gateway-providers.md) — USSD bearer providers (Africa's Talking / Infobip), DRC coverage, cost, `CON/END` model, thin `/ussd` adapter
- [pawapay-api-deep-dive.md](../02-findings/aggregators/pawapay-api-deep-dive.md) — pawaPay rail capabilities + webhook signing (RFC-9421/ECDSA-P256)
- [network-and-devices.md](../02-findings/app-development/network-and-devices.md) — Transsion dominance + AMOLED considerations
- [customer-support.md](../02-findings/app-development/customer-support.md) — WhatsApp + Africa's Talking validation
