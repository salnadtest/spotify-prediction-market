# Contributing

Thanks for your interest in contributing to **spotify-prediction-market**.

## Current state

This repository is currently **documentation-only**. The most valuable contributions right now are:

- clarifying the **oracle** (what exactly “Spotify streams” means and how it’s sourced)
- tightening the **market spec** (what contracts exist, how they pay out, how they settle)
- proposing an **MVP architecture** that can be implemented incrementally

## How to contribute

- **Open an issue** describing the change and why it’s needed.
- **Keep PRs small** and focused (one concept per PR).
- Prefer **documents + examples** over vague statements. If you propose a market type, include:
  - its settlement metric
  - its payout function
  - at least one worked example

## Suggested docs to read first

- `README.md`
- `docs/ORACLE.md`
- `docs/MARKET_DESIGN.md`
- `docs/ARCHITECTURE.md`

## Conventions (for now)

- Use Markdown for docs.
- Keep terminology consistent:
  - “**event time**” = the UTC timestamp at which settlement is measured
  - “**oracle value**” = the finalized metric used for settlement
  - “**market**” = a contract definition + trading rules + settlement rules

## Security

If you find a security issue, please follow `SECURITY.md`.
