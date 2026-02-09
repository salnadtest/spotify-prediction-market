# Oracle / Settlement Data

The single most important design decision for this project is the **oracle**: the data source and rules used to compute the final settlement value for a market.

## Why this matters

Many people assume “Spotify streams” is a simple API call. In practice:

- Spotify’s public Web API is primarily designed for **catalog metadata** and does **not** reliably expose “total streams to date” for arbitrary tracks.
- If settlement is ambiguous, a market becomes **disputable** and users lose trust quickly.

Therefore every market type MUST specify, in a machine-checkable way:

- the **metric** (what number are we measuring?)
- the **event time** (when is the measurement taken, in UTC?)
- the **data source** (where does the number come from?)
- the **finalization policy** (when is the value considered final?)
- the **fallback/dispute** process

## Proposed settlement metrics (choose one, document it)

### Option A: “Spotify Charts daily streams”

Define markets against a metric that Spotify already publishes in a stable, report-like format, such as:

- “Daily streams in region R on date D”
- “Cumulative daily streams across region set S from date A..D”

Pros:

- More likely to be reproducible and time-indexed.

Cons:

- Not all tracks/regions/timeframes are covered.
- You’re building markets on a *subset* of possible songs.

### Option B: “Licensed provider feed”

Use a third-party provider with contractual access to stream-count data.

Pros:

- Broad coverage, potentially true “total streams”.

Cons:

- Requires a paid contract.
- Centralizes trust in the provider.

### Option C: “Operator-attested snapshots”

The operator publishes periodic snapshots of stream totals (or other metrics) and signs them. Settlement uses the signed snapshot closest after event time.

Pros:

- Easiest to ship as an MVP.
- Clear operational control.

Cons:

- Fully centralized trust.

## Track identification

Markets should identify a song using immutable identifiers where possible:

- **Spotify Track ID** (for UX / deep linking)
- **ISRC** (preferred for identity across platforms; not always available everywhere)

Recommendation: store both when possible, and treat ISRC as the canonical identity if present.

## Finalization policy

Even with a defined data source, values can be revised. You should define:

- **Observation window**: at what timestamp do we read the value?
- **Finalization delay**: wait N hours/days after event time before finalizing to allow revisions.
- **Reorg policy**: whether “late corrections” can change a settled market (usually “no”; instead create a corrective market or manual adjustment policy).

## Fallback and disputes

At minimum, document:

- what happens if the oracle is unavailable (pause settlement, extend event time, or cancel market)
- who can propose a value (operator only vs. community)
- how disputes are resolved (operator decision, committee vote, etc.)

For an MVP, a reasonable starting point is:

- operator-only oracle posting
- transparent logs of oracle values
- an explicit cancellation policy if data cannot be obtained
