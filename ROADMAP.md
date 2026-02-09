# Roadmap

This roadmap is intentionally staged from “shippable MVP” to “more trustworthy / more decentralized”.

## Phase 0: Decide what “streams” means

- Pick and document the **oracle** (`docs/ORACLE.md`)
- Lock down market specifications:
  - identifiers (Spotify Track ID + ISRC when available)
  - event time rules (UTC)
  - finalization delay and cancellation rules

## Phase 1: MVP (centralized but auditable)

- Backend API + database schema for:
  - markets, trading, balances, settlement
  - append-only oracle observations
- One trading mechanism (AMM *or* order book)
- Minimal frontend:
  - browse markets, market details, trade, portfolio
- Admin tooling:
  - create/close/cancel markets
  - post oracle values (with logs)

## Phase 2: Reliability and safety

- Rate limiting, abuse prevention, audit logs
- Monitoring/alerting and data integrity checks
- Dispute workflow + documented operator policies

## Phase 3: More trust-minimized oracle

- Signed oracle feed and public archive of signed observations
- Multi-signer or committee-based oracle posting
- Formalize governance around disputes and cancellations
