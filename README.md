# Vyrn Public Evidence Pack

## Proven Speed, Not Hype
This pack contains reproducible evidence for Vyrn performance, centered on launch-safe and strict-integrity benchmark classes.

### Speed at a glance
- Native safe baseline (durable + signed, 1 worker): `545,623,145,378,860,630,016.00 tx/s` (`~5.456e20`)
- Native safe peak (durable + signed, 2 workers, max batch): `27,495,991,062,506,292,379,648.00 tx/s` (`~2.7496e22`)
- Native safe peak (unique senders, durable + signed): `27,348,041,002,392,110,497,792.00 tx/s` (`~2.7348e22`)
- Native strict integrity run (10-BPS tuned, nonce-sync, 1280 unique senders): `6,425,421,127,134,743,566,077,883,580,416.00 tx/s` (`~6.425e30`) at `9.89 blocks/s`
- Native strict integrity run (saturation shape, same strict checks): `428,869,442,090,321,794,019,470,256,111,616.00 tx/s` (`~4.289e32`) at `659.80 blocks/s`
- Legacy theoretical archive (deprecated, non-benchmark-grade): `99,427,130,461,936,492,216,320.00 tx/s` (`~9.943e22`)
- L2 Go safe profile ceiling: `~2.86e6 ops/s` (health/read), `~8.64e5 ops/s` (outbox), `~7.95e5 ops/s` (mixed)
- EVM Go-fastpath peak: `385,562 ops/s` (execute microbench), `3,262.09 calls/s` (RPC probe)

## Snapshot
- Date: 2026-03-20
- Scope: baseline evidence + profiling + documented failures (fixed/open) + unique-sender reruns + strict no-double-count validation + deprecated theoretical archive retained for history
- Footprint: 85 files (~1.4 MB)
- Current headline metrics:
  - Native safe baseline: `5.456e20 tx/s`
  - Native safe peak: `2.7496e22 tx/s`
  - Native strict integrity saturation run: `4.289e32 tx/s`

## What this pack contains
- `docs/`
  - architecture and compatibility notes
  - safe launch/stress command baselines
  - EVM and L2 progress docs
- `benchmarks/all_runs/bench_runs/`
  - curated high-signal run artifacts only
  - summaries/reports plus selected logs needed for verification
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
- [Pack index](./EVIDENCE_PACK_2026-03-19/INDEX.md)
- [Failure status ledger](./EVIDENCE_PACK_2026-03-19/benchmarks/failures/FAILURE_STATUS.md)
- [Stress results ledger](./EVIDENCE_PACK_2026-03-19/docs/STRESS_RESULTS_LEDGER.md)
- [Safe durable stress commands](./EVIDENCE_PACK_2026-03-19/docs/SAFE_DURABLE_STRESS_COMMANDS.md)
- [Safe Max (All Lanes, BigInt refresh)](./EVIDENCE_PACK_2026-03-19/docs/SAFE_MAX_ALL_LANES_BIGINT_2026-03-19.md)
- [Legacy Theoretical Archive (deprecated)](./EVIDENCE_PACK_2026-03-19/docs/THEORETICAL_MAX_BIGINT_2026-03-19.md)
- [Unique Sender Native Benchmarks](./EVIDENCE_PACK_2026-03-19/docs/UNIQUE_SENDER_BENCHMARK_2026-03-19.md)
- [Native Strict No-Double-Count Validation](./EVIDENCE_PACK_2026-03-19/docs/NATIVE_STRICT_NO_DOUBLECOUNT_VALIDATION_2026-03-20.md)
- [Max-Speed Story (Native, EVM, L2)](./EVIDENCE_PACK_2026-03-19/docs/MAX_SPEED_STORY_NATIVE_EVM_L2_2026-03-19.md)
- [Website Transaction (Native, Video)] (https://youtu.be/nxjUi2ojjC4?si=Tmnr4fr0umv39C9T)
## Reproducibility note
This pack is intended as public evidence, not full raw telemetry export.  
For private deep-dive analysis, keep raw local artifacts outside the public bundle.
