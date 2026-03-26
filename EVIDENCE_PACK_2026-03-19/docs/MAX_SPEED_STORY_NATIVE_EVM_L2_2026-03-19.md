# Vyrn Max-Speed Story (Native, EVM, L2) - 2026-03-25 refresh

This is the benchmark story behind the current speed envelope, using the latest logs and summaries in this pack.

## Chapter 1: Native has baseline-safe, peak-safe, strict-integrity, and legacy-theoretical records

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
  - BigInt order sweep e64 shape: `47,988,080,403,867,954,877,001,267,244,386,490,545,343,709,017,019,851,481,418,825,728.00 tx/s` (`~4.799e64`)
  - BigInt order sweep e128 shape: `779,787,948,286,304,052,347,352,079,276,055,395,744,626,439,611,832,122,972,740,039,848,233,739,288,129,295,259,788,203,451,820,604,207,813,044,860,847,078,225,695,735,808.00 tx/s` (`~7.798e128`)
  - BigInt order sweep e512 shape: `10,892,805,346,129,397,837,772,834,905,348,366,311,152,353,739,690,085,357,404,997,837,759,829,549,492,672,436,781,102,673,107,259,257,499,057,706,655,651,857,858,036,431,714,219,237,402,434,597,700,824,135,562,205,686,229,709,285,310,504,301,032,810,825,602,318,065,864,011,744,338,507,977,220,831,737,277,046,902,736,057,460,400,207,343,787,560,383,321,431,620,174,448,005,212,729,502,508,789,216,460,574,540,495,880,540,700,410,803,823,184,783,604,982,596,073,392,129,658,151,798,236,443,367,032,362,870,111,177,095,113,385,883,263,629,590,425,927,788,139,913,723,354,312,780,360,880,815,898,754,637,407,928,177,138,518,084,893,218,890,877,227,837,150,333,355,290,832,014,626,564,82.77 tx/s` (`~1.089e513`, non-node-safe BPS stress shape)
  - evidence:
    - `benchmarks/all_runs/bench_runs/strict_no_doublecount_20260320_031126.log`
    - `benchmarks/all_runs/bench_runs/strict_no_doublecount_saturation_20260320_031157.log`
    - `docs/NATIVE_STRICT_NO_DOUBLECOUNT_VALIDATION_2026-03-20.md`
    - `benchmarks/all_runs/bench_runs/native_bigint_order_sweep_20260325/summary_native_bigint_order_sweep_20260325.json`
    - `docs/NATIVE_STRICT_BIGINT_ORDER_SWEEP_2026-03-25.md`

- **Legacy theoretical archive (deprecated, non-benchmark-grade)**:
  - best measured: `99,427,130,461,936,492,216,320.00 tx/s` (`~9.943e22`)
  - prior e22 guardrails-off reference: `6,973,281,673,524,083,163,136.00 tx/s` (`~6.973e21`)
  - evidence:
    - `benchmarks/all_runs/bench_runs/theory_bigbatch_push_20260319_205422/summary_theory_bigbatch_push.json`
    - `benchmarks/all_runs/bench_runs/theory_bigbatch_push_20260319_205422/theory_w2_t100000e18_b50e18.log`
    - `docs/THEORETICAL_MAX_BIGINT_2026-03-19.md`

Interpretation:
- Safe numbers are no longer represented as one ambiguous value.
- Strict integrity measurements now include e30+, e32, e64, e128, and e513-class native results.
- Legacy theoretical runs are retained for history but excluded from current benchmark-grade headline claims.

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
- Strict integrity (BigInt sweep e64): `~4.799e64 tx/s`
- Strict integrity (BigInt sweep e128): `~7.798e128 tx/s`
- Strict integrity (BigInt sweep e512): `~1.089e513 tx/s` (non-node-safe BPS stress shape)
- Legacy theoretical archive (deprecated): `~9.943e22 tx/s`

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
- "Strict integrity native runs are now documented through `e513` class in BigInt order-sweep stress shape."
- "Legacy theoretical runs are archived and marked deprecated for current benchmark-grade claims."
- "EVM and L2 are measured with lane-level maxima, not blended scoreboard numbers."

## Unit note

These lanes use different units (`tx/s`, `ops/s`, `calls/s`) and should be compared by lane intent, not as a single blended scoreboard.
