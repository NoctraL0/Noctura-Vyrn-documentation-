# Vyrn Max-Speed Story (Native, EVM, L2) - 2026-03-20

This is the benchmark story behind the current speed envelope, using the latest logs and summaries in this pack.

## Chapter 1: Native has baseline-safe, peak-safe, and theoretical peak evidence

The native lane now has four clearly separated truths:

- **Launch-safe baseline** (durable + signed, 1 worker):
  - `545,623,145,378,860,630,016.00 tx/s` (`~5.456e20`)
  - evidence:
    - `benchmarks/all_runs/bench_runs/native_safe_1w_15bps_bigint_hi_20260319_175137.log`

- **Launch-safe throughput peak** (durable + signed, max big-block shape):
  - non-unique sender peak: `27,495,991,062,506,292,379,648.00 tx/s` (`~2.7496e22`)
  - unique sender peak: `27,348,041,002,392,110,497,792.00 tx/s` (`~2.7348e22`)
  - unique penalty vs non-unique: `-0.538%`
  - evidence:
    - `benchmarks/all_runs/bench_runs/native_2w_550bps_bigblk_FINAL_20260319_180047.log`
    - `benchmarks/all_runs/bench_runs/native_unique_2w550bps_big_20260319_182803.log`
    - `docs/UNIQUE_SENDER_BENCHMARK_2026-03-19.md`

- **Strict counter-integrity class** (durable + signed + `nonce-sync` + unique sender pool):
  - tuned 10-BPS shape: `6,425,421,127,134,743,566,077,883,580,416.00 tx/s` (`~6.425e30`)
  - saturation shape: `428,869,442,090,321,794,019,470,256,111,616.00 tx/s` (`~4.289e32`)
  - evidence:
    - `benchmarks/all_runs/bench_runs/strict_no_doublecount_20260320_031126.log`
    - `benchmarks/all_runs/bench_runs/strict_no_doublecount_saturation_20260320_031157.log`
    - `docs/NATIVE_STRICT_NO_DOUBLECOUNT_VALIDATION_2026-03-20.md`

- **Theoretical peak** (guardrails off, big-batch push):
  - best measured: `99,427,130,461,936,492,216,320.00 tx/s` (`~9.943e22`)
  - prior e22 guardrails-off reference: `6,973,281,673,524,083,163,136.00 tx/s` (`~6.973e21`)
  - evidence:
    - `benchmarks/all_runs/bench_runs/theory_bigbatch_push_20260319_205422/summary_theory_bigbatch_push.json`
    - `benchmarks/all_runs/bench_runs/theory_bigbatch_push_20260319_205422/theory_w2_t100000e18_b50e18.log`

Interpretation:
- Safe numbers are no longer represented as one ambiguous value.
- Strict integrity measurements now include e30+ and e32-class native results with nonce-sync and sender uniqueness controls.
- Theoretical numbers are now above the safe peaks and explicitly marked non-launch-safe.

## Chapter 2: EVM is measurable on two ceilings

The EVM compatibility lane shows two useful max views:

- **Execution microbench max** (Go fastpath direct):
  - `python_incr_direct`: `385,562 ops/s`
  - evidence:
    - `docs/EVM_PERF_PROGRESS_2026-03-18.md`

- **EVM RPC lane max from engine probes**:
  - `3,262.09 calls/s` (`engine=evm`, `workers=16`, msgpack RPC)
  - evidence:
    - `benchmarks/all_runs/bench_runs/engine_probe_20260319_163228/evm.json`

Interpretation:
- Local execute ceiling and RPC-lane ceiling are different bottlenecks.
- Both are measured and documented.

## Chapter 3: L2 is in high six-figure to low seven-figure ops territory

Under the Go L2 safe profile, measured ceilings are:

- mixed mode: `~7.95e5 ops/s`
- outbox-only: `~8.64e5 ops/s`
- health/read path: `~2.86e6 ops/s`

Evidence:
- `docs/L2_GO_ENGINE_BASELINE.md`

## Chapter 4: Complete max-speed picture right now

### Native maxes
- Safe baseline: `~5.456e20 tx/s`
- Safe peak (non-unique): `~2.7496e22 tx/s`
- Safe peak (unique): `~2.7348e22 tx/s`
- Strict integrity (10-BPS tuned): `~6.425e30 tx/s`
- Strict integrity (saturation): `~4.289e32 tx/s`
- Theoretical peak: `~9.943e22 tx/s`

### EVM maxes
- Fastpath execute microbench: `385,562 ops/s`
- EVM RPC probe max: `3,262.09 calls/s`

### L2 maxes
- mixed: `~7.95e5 ops/s`
- outbox-only: `~8.64e5 ops/s`
- health/read: `~2.86e6 ops/s`

## Chapter 5: Public messaging that stays accurate

The strongest honest framing is:
- "Safe native baseline is in the `5e20` class."
- "Safe native throughput peak has crossed `2.7e22` in this evidence pack."
- "Strict integrity native runs are in the `e30+` and `e32` classes."
- "Theoretical native peak has crossed `9.9e22` with guardrails off."
- "EVM and L2 are measured with lane-level maxima, not blended scoreboard numbers."

## Unit note

These lanes use different units (`tx/s`, `ops/s`, `calls/s`) and should be compared by lane intent, not as a single blended scoreboard.
