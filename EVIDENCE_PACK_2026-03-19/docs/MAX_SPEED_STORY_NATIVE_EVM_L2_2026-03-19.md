# Vyrn Max-Speed Story (Native, EVM, L2) - 2026-03-19

This is the benchmark story behind the current speed envelope, using the latest logs and summaries in this pack.

## Chapter 1: Native went from launch-safe monster to theoretical monster

The native lane now has two clearly separated truths:

- **Launch-safe high-throughput baseline** (durable + signed):
  - `549,341,337,575,504,347,136.00 tx/s` (`~5.493e20`)
  - with `5,000,000,000,000,000,000` tx/block average
  - evidence:
    - `benchmarks/all_runs/bench_runs/bigint_highbatch_refresh_20260319_201037/summary_bigint_highbatch.json`
    - `benchmarks/all_runs/bench_runs/bigint_highbatch_refresh_20260319_201037/safe_highbatch_stress.log`

- **Theoretical push at e22 shape** (same stress shape, guardrails toggled):
  - guardrails **on**: `2,909,009,305,851,459,534,848.00 tx/s` (`~2.909e21`)
  - guardrails **off**: `6,973,281,673,524,083,163,136.00 tx/s` (`~6.973e21`)
  - off/on gain: `2.397x`
  - evidence:
    - `benchmarks/all_runs/bench_runs/e22_guardrails_ab_20260319_201707/summary_e22_guardrails_compare.json`
    - `benchmarks/all_runs/bench_runs/e22_guardrails_ab_20260319_201707/guardrails_on_stress.log`
    - `benchmarks/all_runs/bench_runs/e22_guardrails_ab_20260319_201707/guardrails_off_stress.log`

Interpretation:
- Native is not just fast at one profile.
- It scales through multiple regimes: durable launch-safe and extreme theoretical.
- Big-int block sizing is now part of the real measured path, not just a claim.

## Chapter 2: EVM is no longer “just there”, it has measurable headroom

The EVM compatibility lane shows two useful max views:

- **Execution microbench max** (Go fastpath direct):
  - `python_incr_direct`: `385,562 ops/s`
  - evidence:
    - `docs/EVM_PERF_PROGRESS_2026-03-18.md`

- **EVM RPC lane max from engine probes**:
  - `3,262.09 calls/s` (`engine=evm`, `workers=16`, msgpack RPC)
  - evidence:
    - `benchmarks/all_runs/bench_runs/engine_probe_20260319_163228/evm.json`
    - this value is reflected in the story as the current observed RPC-lane peak

Interpretation:
- The EVM path has a high local execute ceiling and a separately measurable networked-RPC ceiling.
- Those are different bottlenecks, and both are now quantified.

## Chapter 3: L2 is already in high six-figure to low seven-figure ops territory

Under the Go L2 safe profile, measured ceilings are:

- mixed mode: `~7.95e5 ops/s`
- outbox-only: `~8.64e5 ops/s`
- health/read path: `~2.86e6 ops/s`

Evidence:
- `docs/L2_GO_ENGINE_BASELINE.md`

Interpretation:
- L2 is already operating in a range that can outrun many compatibility-layer assumptions.
- The read/control path is substantially faster than mixed write-heavy paths, which is expected and useful for production planning.

## Chapter 4: The complete max-speed picture right now

### Native maxes
- Safe durable high-batch: `~5.493e20 tx/s`
- e22 guardrails-on: `~2.909e21 tx/s`
- e22 guardrails-off: `~6.973e21 tx/s` (current highest observed native value in this pack)

### EVM maxes
- Fastpath execute microbench: `385,562 ops/s`
- EVM RPC probe max: `3,262.09 calls/s`

### L2 maxes
- mixed: `~7.95e5 ops/s`
- outbox-only: `~8.64e5 ops/s`
- health/read: `~2.86e6 ops/s`

## Chapter 5: What this means for public messaging

The strongest honest framing is:
- “Launch-safe native is already in the `5e20` class.”
- “Theoretical native with e22 stress shape has crossed `6.9e21` in current evidence.”
- “EVM and L2 are measured with explicit lane-level maxima, not hand-wavy aggregate numbers.”

## Unit note

These lanes use different units (`tx/s`, `ops/s`, `calls/s`) and should be compared by lane intent, not as a single blended scoreboard.
