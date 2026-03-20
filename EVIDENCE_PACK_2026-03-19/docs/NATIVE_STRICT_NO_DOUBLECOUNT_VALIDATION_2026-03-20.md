# Native Strict No-Double-Count Validation (2026-03-20)

Purpose: validate that the native lane counters are not being inflated by duplicate/non-unique accounting.

## Validation profile

- `miner_mineRangeSigned` with `ed25519`
- strict controls enabled:
  - `--stop-on-reject`
  - `--nonce-sync`
  - `--senders-per-worker 64` (total unique senders: `1280`)
- big-int batch profile:
  - `--batch-size-big 650000000000000000000000000000`

## Results

| Run | Shape | TX committed | Blocks | TX/s | Blocks/s | Proof |
|---|---|---:|---:|---:|---:|---|
| strict 10-bps tuned | `--tps 6500000000000000000000000000000`, `--duration 10s` | `65000000000000000000000000000000` | `100` | `6425421127134743566077883580416.00` | `9.89` | [`strict_no_doublecount_20260320_031126.log`](../benchmarks/all_runs/bench_runs/strict_no_doublecount_20260320_031126.log) |
| strict saturation | `--tps 65000000000000000000000000000000000000000`, `--duration 10s` | `4296500000000000000000000000000000` | `6610` | `428869442090321794019470256111616.00` | `659.80` | [`strict_no_doublecount_saturation_20260320_031157.log`](../benchmarks/all_runs/bench_runs/strict_no_doublecount_saturation_20260320_031157.log) |

## Integrity checks

- No reject/error lines in either log.
- Counter-level consistency holds:
  - 10-bps run: `txs_committed / blocks_mined = 650000000000000000000000000000`
  - saturation run: `txs_committed / blocks_mined = 650000000000000000000000000000`
- `avg_txs_per_block` is printed via floating-point formatting and may show tiny decimal drift; use integer counters for exact verification.

## Reproduce

```bash
cd <REPO_ROOT>/go_dag

# Strict 10-BPS tuned check
./stress \
  --rpc msgpack+unix://<REPO_ROOT>/.runtime_bigint_docs/dag_rpc_safe_run.sock \
  --workers 20 \
  --senders-per-worker 64 \
  --sender-base addr:strict_$(date +%s) \
  --soak --tps 6500000000000000000000000000000 --duration 10s \
  --batch-size-big 650000000000000000000000000000 \
  --mine-range --sign --sig-scheme ed25519 \
  --nonce-sync --range-mode direct --range-root range --assume-new \
  --stop-on-reject --progress-every 20 --lat-samples 50000
```

