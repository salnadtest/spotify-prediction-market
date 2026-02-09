# Market Design

This document proposes market types and settlement rules for a “Spotify stream prediction market”.

## Core objects

- **Market**: a contract definition (what is being predicted), trading rules, and settlement rules.
- **Outcome**: what a share pays at settlement (depends on market type).
- **Event time**: the UTC timestamp when the oracle value is observed.
- **Oracle value**: the finalized metric read from the oracle at/after event time.

## Market types (MVP)

### 1) Binary (Yes/No threshold)

**Question**: Will the oracle value \(V\) be \(\ge T\) at event time?

- Parameters: track, event time, threshold \(T\)
- Payout per share:
  - YES pays 1 if \(V \ge T\), else 0
  - NO pays 1 if \(V < T\), else 0

Why it’s good for MVP:

- Easy to explain
- Easy to settle
- Allows markets like “Will this song hit 10M streams by June 1?”

### 2) Range (Bucketed)

**Question**: Which bucket contains \(V\)?

- Define buckets \(B_1, B_2, ..., B_n\) that partition the value range.
- Each bucket token pays 1 if \(V \in B_i\), else 0.

Benefits:

- Reduces oracle precision requirements
- More expressive than binary

### 3) Scalar (Linear payout with caps)

**Question**: Predict the magnitude of \(V\) with a continuous payout.

One simple form is a **capped linear** token:

- Parameters: floor \(L\), cap \(U\)
- Payout:

  - 0 if \(V \le L\)
  - 1 if \(V \ge U\)
  - \((V - L) / (U - L)\) otherwise

Benefits:

- Encourages more granular information aggregation

Tradeoff:

- Requires higher confidence in the oracle’s numeric integrity and finalization policy

## Trading mechanisms

You can implement either:

- **Order book**: users place limit orders; engine matches them.
- **AMM (LMSR)**: an automated market maker that always offers a price.

For an MVP with low liquidity, an AMM is often simpler to provide usable markets immediately.

## Market invariants (things to specify explicitly)

Every market MUST define:

- the oracle metric and data source (see `docs/ORACLE.md`)
- time zone and event time conventions (always UTC)
- settlement rounding rules (if any)
- cancellation conditions (oracle unavailable, track removed, etc.)
