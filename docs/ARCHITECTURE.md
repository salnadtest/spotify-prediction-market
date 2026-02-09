# Architecture (Proposed)

This is a pragmatic architecture for an MVP that can evolve into a production system.

## High-level components

- **Web app**: browse markets, view charts, place trades, manage account
- **API server**: market creation, order placement, matching/AMM, balances, settlement
- **Worker(s)**: periodic tasks (oracle polling, settlement finalization, notifications)
- **Database**: canonical state (markets, trades, balances, oracle observations)

## Data flow (typical)

1. User creates a market (track + metric + event time + payout spec).
2. Trading happens until market close (or event time).
3. Worker observes the oracle value and records an immutable observation.
4. After the finalization delay, the market is settled and payouts are applied.

## Suggested storage model (tables / entities)

- `users`
- `markets`
  - identifier, type (binary/range/scalar), parameters (thresholds/buckets), event_time_utc, close_time_utc
  - oracle_spec (metric + source + finalization policy)
  - status (open/closed/settling/settled/cancelled)
- `orders` (if order book)
- `trades`
- `positions` / `balances`
- `oracle_observations`
  - market_id, observed_at_utc, value, source, signature/proof (if applicable)
- `settlements`

## MVP tech choices

Pick one of these coherent “lanes”:

### Lane 1: TypeScript end-to-end

- Web: Next.js
- API: NestJS (or Next.js API routes initially)
- DB: Postgres (via Prisma)
- Jobs: BullMQ + Redis

### Lane 2: Python backend

- Web: Next.js
- API: FastAPI
- DB: Postgres (SQLAlchemy)
- Jobs: Celery/RQ + Redis

## Non-functional requirements to plan for early

- **Auditability**: append-only logs for oracle observations + trades
- **Abuse prevention**: rate limits, CAPTCHA for signups, market-creation limits
- **Data integrity**: idempotent settlement jobs; strict event-time handling (UTC)
- **Privacy/security**: secure auth sessions, secret management, least-privileged DB roles
