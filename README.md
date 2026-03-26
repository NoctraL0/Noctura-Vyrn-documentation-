# Vyrn Chain

Vyrn is a high-throughput Layer 1 focused on signed native execution, durability guardrails, and practical compatibility lanes (EVM, WASM, BPF, and Go L2) without turning performance claims into black-box marketing.

## Why This Repo Exists

This repository is both:
- active chain/runtime development code
- a public evidence trail for benchmark and compatibility progress

If you only want proof first, jump to the public evidence bundle:
- `public_release/README.md`
- `public_release/EVIDENCE_PACK_2026-03-19/README.md`

## Current Performance Snapshot

These are lane-specific metrics from published artifacts.

- Native safe baseline (durable + signed): `~5.456e20 tx/s`
- Native safe peak (durable + signed): `~2.7496e22 tx/s`
- Native strict integrity (10-BPS tuned): `~6.425e30 tx/s`
- Native strict integrity (saturation shape): `~4.289e32 tx/s`
- Native strict BigInt order sweep (signed + durable, non-node-safe BPS stress shape): `~1.089e513 tx/s`
- EVM Go fastpath execute microbench: `385,562 ops/s`
- EVM RPC probe ceiling: `3,262.09 calls/s`
- L2 Go safe profile: `~2.86e6 ops/s` (read), `~8.64e5 ops/s` (outbox), `~7.95e5 ops/s` (mixed)

Important:
- Do not compare `tx/s`, `ops/s`, and `calls/s` as one blended scoreboard.
- Node-safe deployment profiles and extreme stress-shape profiles are reported separately.

## Benchmark Integrity Model

Vyrn benchmark docs intentionally separate run classes:
- launch-safe baselines (durable, signed, checkpointed)
- strict-integrity stress (signed + durable with stronger counter discipline)
- legacy theoretical archives (kept for history, not headline benchmarking)

This split is deliberate so claims remain verifiable and honest.

## Start Here (Evidence)

- Pack index: `EVIDENCE_PACK_2026-03-19/INDEX.md`
- Safe max summary: `EVIDENCE_PACK_2026-03-19/docs/SAFE_MAX_ALL_LANES_BIGINT_2026-03-19.md`
- Strict no-double-count validation: `EVIDENCE_PACK_2026-03-19/docs/NATIVE_STRICT_NO_DOUBLECOUNT_VALIDATION_2026-03-20.md`
- Strict BigInt order sweep (latest): `EVIDENCE_PACK_2026-03-19/docs/NATIVE_STRICT_BIGINT_ORDER_SWEEP_2026-03-25.md`
- Legacy theoretical archive (deprecated for headline claims): `EVIDENCE_PACK_2026-03-19/docs/THEORETICAL_MAX_BIGINT_2026-03-19.md`
- Failure ledger (fixed/open): `EVIDENCE_PACK_2026-03-19/benchmarks/failures/FAILURE_STATUS.md`

## Public Media Proof

- Website transaction demo (native): https://youtu.be/nxjUi2ojjC4?si=Tmnr4fr0umv39C9T
- Terminal tx/sec benchmark demo (native): https://youtu.be/3i62ka8ZrmM?si=P9Fccij9cmuSNRfo

## Repository Layout

- `dag_rpc_server.py`: native DAG RPC server path and range mining surface
- `rpc_server_v1.py`: main RPC entry surface
- `evm_runtime.py`, `go_evm_fastpath.py`: EVM compatibility path
- `wasm_runtime.py`, `go_wasm/`: WASM compatibility path
- `bpf_runtime.py`: BPF compatibility path
- `go_l2/`: Go L2 engine and benchmarks
- `public_release/`: GitHub-safe evidence and buyer-facing bundles

## Data / Redaction Policy

Public bundles are curated to stay verifiable and safe:
- includes reproducible logs, summaries, and selected profiles
- excludes raw chain payload/state trees and noisy intermediate artifacts
- redacts local absolute paths and obvious secret-like fields where present

## Status

Vyrn is in active development. Numbers and compatibility lanes are continuously revalidated, and all major updates are expected to land with evidence artifacts, not just claims.
