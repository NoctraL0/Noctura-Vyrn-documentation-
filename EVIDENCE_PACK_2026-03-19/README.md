# Vyrn Public Evidence Pack

Public-safe, curated benchmark and architecture evidence for Vyrn-Chain.

## Snapshot
- Date: 2026-03-19
- Scope: baseline evidence + profiling + documented failures (fixed/open)
- Footprint: lean pack (curated to avoid raw-data bloat)

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
- [Pack index](./INDEX.md)
- [Failure status ledger](./benchmarks/failures/FAILURE_STATUS.md)
- [Stress results ledger](./docs/STRESS_RESULTS_LEDGER.md)
- [Safe durable stress commands](./docs/SAFE_DURABLE_STRESS_COMMANDS.md)
- [Safe Max (All Lanes, BigInt refresh)](./docs/SAFE_MAX_ALL_LANES_BIGINT_2026-03-19.md)
- [Theoretical Max (BigInt + larger blocks)](./docs/THEORETICAL_MAX_BIGINT_2026-03-19.md)
- [Max-Speed Story (Native, EVM, L2)](./docs/MAX_SPEED_STORY_NATIVE_EVM_L2_2026-03-19.md)

## Reproducibility note
This pack is intended as public evidence, not full raw telemetry export.  
For private deep-dive analysis, keep raw local artifacts outside the public bundle.
