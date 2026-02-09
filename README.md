# spotify-prediction-market

A prediction market for **future Spotify stream outcomes** of a song.

## Status

This repository is currently a **project stub**: it contains documentation only (no application code yet).

## What this project is (intended)

The goal is to let users create and trade markets like:

- **Binary**: “Will track `X` have **≥ 10,000,000** Spotify streams by **2026-06-01 00:00 UTC**?”
- **Scalar**: “How many total Spotify streams will track `X` have by date `D`?” (with a defined payout curve)
- **Range**: “Will the stream count fall within \([A, B]\)?”

At market end, the contract settles based on an **oracle value**: the stream count (or another clearly defined metric) for the referenced track at the specified time.

## Critical design constraint (the “oracle” problem)

Spotify’s public Web API does **not** generally provide a canonical “total streams” number for arbitrary tracks. That means you must decide—up front—what *verifiable* data source you will use for settlement.

Common options:

- **Spotify Charts-based markets**: settle on “daily streams as reported by Spotify Charts” (works if your market is defined over chart-eligible regions/timeframes).
- **Licensed data provider** (e.g., Chartmetric / Soundcharts): settle on their stream-count feed (requires contract + trust in the provider).
- **Your own attested dataset**: you publish snapshots and sign them; users trust your key (simple, centralized).

This repo should explicitly document which oracle is used, how disputes are handled, and what happens if the oracle is unavailable.

## Suggested technology stack (pick one path)

No stack is implemented yet. A practical MVP could be:

- **Web**: Next.js (React + TypeScript)
- **API**: Node (NestJS) *or* Python (FastAPI)
- **DB**: Postgres (markets, orders, trades, balances, settlements)
- **Jobs**: Redis + worker (poll oracle, compute settlements)
- **Auth**: email/OAuth; optionally “Sign-in with Spotify” for UX (not required for oracle)
- **Deployment**: Docker + a simple PaaS (Render/Fly/Heroku-like) to start

Market mechanism options:

- **Order book** (classic exchange) + matching engine
- **AMM (LMSR)** for continuous pricing and easy liquidity bootstrapping

## Roadmap (high level)

- Define market specification (track identifiers, timestamps, payout functions)
- Choose and document the oracle + dispute policy
- Implement core backend: market creation, trading, settlement
- Implement frontend: browse markets, trade, portfolio, settlement history
- Add monitoring, rate limiting, anti-abuse, and security hardening

See `ROADMAP.md` for a staged plan.

## Docs

- `docs/ORACLE.md`: what “streams” means and how settlement works
- `docs/MARKET_DESIGN.md`: contract types and payout functions
- `docs/ARCHITECTURE.md`: proposed MVP architecture and data model

## Contributing

See `CONTRIBUTING.md` for development workflow and project conventions.

## License

Apache-2.0 (see `LICENSE`).
