# Vyrn Public Evidence Pack (2026-03-19)

Lean, public-safe baseline evidence set (curated for validity and low bloat).

## Current footprint
- 61 files
- ~1.1 MB

## Validity checks (passed)
- All `*.json` parse cleanly with `jq`.
- All `*.svg` parse cleanly with `xmllint`.

## Included evidence
- `docs/`: architecture, performance, compatibility, and safe command baselines.
  - includes fresh BigInt refresh docs:
    - `docs/SAFE_MAX_ALL_LANES_BIGINT_2026-03-19.md`
    - `docs/THEORETICAL_MAX_BIGINT_2026-03-19.md`
    - `docs/MAX_SPEED_STORY_NATIVE_EVM_L2_2026-03-19.md`
- `benchmarks/all_runs/bench_runs/`: curated high-signal run set only:
  - `safe_peak_final_20260317_190159`
  - `limit_hunt_5p45e20_20260319_015446`
  - `full_saturation_dual_20260319_092040`
  - `apocalypse_fullstack_ccd_pinned_go_l2_binary_20260319_153311`
  - `engine_probe_wasm_lazy_20260319_165307`
  - `theoretical_max_km200k_20260318_010845`
  - `bigint_highbatch_refresh_20260319_201037`
  - `e22_guardrails_ab_20260319_201707`
  - `engine_probe_20260319_163228`
  - `native_unique_1w15bps_big_20260319_182744.log`
  - `native_unique_2w550bps_big_20260319_182803.log`
- `benchmarks/failures/`: fixed/open issue evidence and status ledger.
- `benchmarks/profiles/evm/`: EVM hot-path profile artifacts (before + latest-after).

## Excluded on purpose
- Raw chain payload/state trees (`chain_data/*`, `phase*_data/*`, and similar).
- Startup/misinput-only incidents.
- Duplicate run folders, noisy sweeps, raw health-monitor logs, and intermediate drafts.

## Redaction policy
- Local absolute paths replaced with `<REPO_ROOT>` / `<REDACTED_PATH>`.
- Secret-like fields redacted when present (`private_key`, signing/admin tokens).
