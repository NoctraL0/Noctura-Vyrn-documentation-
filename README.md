# Vyrn Public Evidence Hub

Public benchmark evidence for Vyrn, with reproducible artifacts and clear separation between safe baseline runs and strict stress classes.

Core principle: **evidence over hype**.

## Headline Metrics

These are lane-specific and should not be blended.

- Native safe baseline (durable + signed): `~5.456e20 tx/s`
- Native safe peak (durable + signed): `~2.7496e22 tx/s`
- Native strict integrity (10-BPS tuned): `~6.425e30 tx/s`
- Native strict integrity (saturation shape): `~4.289e32 tx/s`
- Native strict BigInt order sweep (signed + durable, non-node-safe BPS stress shape): `~1.089e513 tx/s`
- EVM Go fastpath execute microbench: `385,562 ops/s`
- EVM RPC probe max: `3,262.09 calls/s`
- L2 Go safe profile: `~2.86e6 ops/s` (read), `~8.64e5 ops/s` (outbox), `~7.95e5 ops/s` (mixed)

## Evidence Entry Points

1. [Evidence Pack Overview](./EVIDENCE_PACK_2026-03-19/README.md)
2. [Evidence Pack Index](./EVIDENCE_PACK_2026-03-19/INDEX.md)
3. [Safe Max (All Lanes)](./EVIDENCE_PACK_2026-03-19/docs/SAFE_MAX_ALL_LANES_BIGINT_2026-03-19.md)
4. [Strict No-Double-Count Validation](./EVIDENCE_PACK_2026-03-19/docs/NATIVE_STRICT_NO_DOUBLECOUNT_VALIDATION_2026-03-20.md)
5. [Strict BigInt Order Sweep](./EVIDENCE_PACK_2026-03-19/docs/NATIVE_STRICT_BIGINT_ORDER_SWEEP_2026-03-25.md)
6. [Failure Status (Fixed + Open)](./EVIDENCE_PACK_2026-03-19/benchmarks/failures/FAILURE_STATUS.md)
7. [Max-Speed Story (Native, EVM, L2)](./EVIDENCE_PACK_2026-03-19/docs/MAX_SPEED_STORY_NATIVE_EVM_L2_2026-03-19.md)
8. [Stress Results Ledger](./EVIDENCE_PACK_2026-03-19/docs/STRESS_RESULTS_LEDGER.md)
9. [Safe Durable Stress Commands](./EVIDENCE_PACK_2026-03-19/docs/SAFE_DURABLE_STRESS_COMMANDS.md)

## Public Bundle Scope

- Included: curated benchmark artifacts, reports, JSON summaries, selected profile SVGs.
- Excluded: private chain payload/state trees and noisy startup/misinput-only incidents.
- Redaction: local absolute paths and secret-like values are stripped from public artifacts.

## Media Proof

- [Website transaction demo (native)](https://youtu.be/nxjUi2ojjC4?si=Tmnr4fr0umv39C9T)
- [Terminal tx/sec benchmark demo (native)](https://youtu.be/3i62ka8ZrmM?si=P9Fccij9cmuSNRfo)

## Bundle

- [EVIDENCE_PACK_2026-03-19](./EVIDENCE_PACK_2026-03-19/)
