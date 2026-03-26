# Vyrn Public Evidence Pack

## Proven Speed, Not Hype
This pack contains reproducible evidence for Vyrn performance, centered on launch-safe and strict-integrity benchmark classes.

### Speed at a glance
- Native safe baseline (durable + signed, 1 worker): `545,623,145,378,860,630,016.00 tx/s` (`~5.456e20`)
- Native safe peak (durable + signed, 2 workers, max batch): `27,495,991,062,506,292,379,648.00 tx/s` (`~2.7496e22`)
- Native safe peak (unique senders, durable + signed): `27,348,041,002,392,110,497,792.00 tx/s` (`~2.7348e22`)
- Native strict integrity run (10-BPS tuned, nonce-sync, 1280 unique senders): `6,425,421,127,134,743,566,077,883,580,416.00 tx/s` (`~6.425e30`) at `9.89 blocks/s`
- Native strict integrity run (saturation shape, same strict checks): `428,869,442,090,321,794,019,470,256,111,616.00 tx/s` (`~4.289e32`) at `659.80 blocks/s`
- Native strict integrity run (BigInt order sweep, signed + durable, 2026-03-25): `10,892,805,346,129,397,837,772,834,905,348,366,311,152,353,739,690,085,357,404,997,837,759,829,549,492,672,436,781,102,673,107,259,257,499,057,706,655,651,857,858,036,431,714,219,237,402,434,597,700,824,135,562,205,686,229,709,285,310,504,301,032,810,825,602,318,065,864,011,744,338,507,977,220,831,737,277,046,902,736,057,460,400,207,343,787,560,383,321,431,620,174,448,005,212,729,502,508,789,216,460,574,540,495,880,540,700,410,803,823,184,783,604,982,596,073,392,129,658,151,798,236,443,367,032,362,870,111,177,095,113,385,883,263,629,590,425,927,788,139,913,723,354,312,780,360,880,815,898,754,637,407,928,177,138,518,084,893,218,890,877,227,837,150,333,355,290,832,014,626,564,82.77 tx/s` (`~1.089e513`) at `837.91 blocks/s` (non-node-safe BPS stress shape)
- Legacy theoretical archive (deprecated, non-benchmark-grade): `99,427,130,461,936,492,216,320.00 tx/s` (`~9.943e22`)
- L2 Go safe profile ceiling: `~2.86e6 ops/s` (health/read), `~8.64e5 ops/s` (outbox), `~7.95e5 ops/s` (mixed)
- EVM Go-fastpath peak: `385,562 ops/s` (execute microbench), `3,262.09 calls/s` (RPC probe)

## Snapshot
- Date: 2026-03-25
- Scope: baseline evidence + profiling + documented failures (fixed/open) + unique-sender reruns + strict no-double-count validation + strict BigInt order sweep refresh + deprecated theoretical archive retained for history
- Footprint: 90 files (~1.48 MB)
- Current headline metrics:
  - Native safe baseline: `5.456e20 tx/s`
  - Native safe peak: `2.7496e22 tx/s`
  - Native strict integrity saturation run: `4.289e32 tx/s`
  - Native strict integrity BigInt order sweep run: `1.089e513 tx/s` (non-node-safe BPS stress shape)

## What this pack contains
- `docs/`
  - architecture and compatibility notes
  - safe launch/stress command baselines
  - EVM and L2 progress docs
- `benchmarks/all_runs/bench_runs/`
  - curated high-signal run artifacts only
  - summaries/reports plus selected logs needed for verification
  - includes `native_bigint_order_sweep_20260325` (strict signed+durable BigInt sweep)
- `benchmarks/failures/`
  - `fixed/`: issues with recovery evidence
  - `open/`: known stress-shape limitations still tracked
- `benchmarks/profiles/evm/`
  - EVM hot-path profiles (before/after optimization passes)

## Data policy
- Included:
  - reproducible run evidence
  - summaries and reports
  - selected inferno SVG profiles
- Excluded:
  - raw chain state/payload directories (`chain_data/*`, `phase*_data/*`, etc.)
  - startup/misinput-only incidents
  - duplicate/intermediate/noisy artifacts

## Validation and safety
- JSON artifacts parse cleanly (`jq`).
- SVG artifacts parse cleanly (`xmllint`).
- Local absolute paths are redacted (`<REPO_ROOT>`, `<REDACTED_PATH>`).
- Obvious secret-like values are redacted when present.

## Entry points
- [Pack index](./INDEX.md)
- [Failure status ledger](./benchmarks/failures/FAILURE_STATUS.md)
- [Stress results ledger](./docs/STRESS_RESULTS_LEDGER.md)
- [Safe durable stress commands](./docs/SAFE_DURABLE_STRESS_COMMANDS.md)
- [Safe Max (All Lanes, BigInt refresh)](./docs/SAFE_MAX_ALL_LANES_BIGINT_2026-03-19.md)
- [Native Strict BigInt Order Sweep (2026-03-25)](./docs/NATIVE_STRICT_BIGINT_ORDER_SWEEP_2026-03-25.md)
- [Legacy Theoretical Archive (deprecated)](./docs/THEORETICAL_MAX_BIGINT_2026-03-19.md)
- [Unique Sender Native Benchmarks](./docs/UNIQUE_SENDER_BENCHMARK_2026-03-19.md)
- [Native Strict No-Double-Count Validation](./docs/NATIVE_STRICT_NO_DOUBLECOUNT_VALIDATION_2026-03-20.md)
- [Max-Speed Story (Native, EVM, L2)](./docs/MAX_SPEED_STORY_NATIVE_EVM_L2_2026-03-19.md)

## Reproducibility note
This pack is intended as public evidence, not full raw telemetry export.  
For private deep-dive analysis, keep raw local artifacts outside the public bundle.
