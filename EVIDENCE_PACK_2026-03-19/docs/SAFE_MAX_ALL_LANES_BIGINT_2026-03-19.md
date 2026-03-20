# Safe Max Snapshot (All Lanes, Big-Block Refresh) - 2026-03-19

## Headline
Launch-safe native has two validated safe classes in this pack:
- stable baseline around `5.456e20 tx/s` (1 worker shape)
- high-throughput peak up to `2.7496e22 tx/s` (2 workers, max big-block shape)

## Scope
- This file is launch-safe baseline evidence.
- Native lane includes both baseline-safe and peak-safe profiles.
- Non-native lane values are the highest safe baselines currently recorded in this public pack.

## Highest Safe Numbers (Current Baseline)

| Lane | Best Safe Metric | Evidence |
|---|---:|---|
| Native signed range (durable baseline, 1 worker) | `545,623,145,378,860,630,016.00 tx/s` | `benchmarks/all_runs/bench_runs/native_safe_1w_15bps_bigint_hi_20260319_175137.log` |
| Native signed range (durable peak, 2 workers, non-unique) | `27,495,991,062,506,292,379,648.00 tx/s` | `benchmarks/all_runs/bench_runs/native_2w_550bps_bigblk_FINAL_20260319_180047.log` |
| Native signed range (durable peak, unique senders) | `27,348,041,002,392,110,497,792.00 tx/s` | `benchmarks/all_runs/bench_runs/native_unique_2w550bps_big_20260319_182803.log` |
| Native signed range (durable high-batch refresh) | `549,341,337,575,504,347,136.00 tx/s` | `benchmarks/all_runs/bench_runs/bigint_highbatch_refresh_20260319_201037/safe_highbatch_stress.log` |
| L2 Go (safe profile, mixed) | `~7.95e5 ops/s` | `docs/L2_GO_ENGINE_BASELINE.md` |
| L2 Go (safe profile, outbox) | `~8.64e5 ops/s` | `docs/L2_GO_ENGINE_BASELINE.md` |
| L2 Go read path (`health`) | `~2.86e6 ops/s` | `docs/L2_GO_ENGINE_BASELINE.md` |
| EVM compat (Go fastpath direct) | `385,562 ops/s` | `docs/EVM_PERF_PROGRESS_2026-03-18.md` |
| Compat mix RPC lane (WASM+EVM path) | `1,013.11 calls/s` | `benchmarks/all_runs/bench_runs/limit_hunt_5p45e20_20260319_015446/summary.json` |
| BPF lane | `2,260.59 calls/s` | `benchmarks/all_runs/bench_runs/limit_hunt_5p45e20_20260319_015446/summary.json` |

## Fresh Native Safe Rerun (Higher Block Size)

Profile:
- `DAG_STORE=log`
- `DAG_PERSIST_BLOCKS=1`
- `DAG_LOG_FSYNC_EVERY=1`
- signed native enabled (`ed25519`, verifier/require on)
- batch/range ceiling raised to `5e18`

Command shape used:

```bash
DATA_DIR=/tmp/vyrn_safe_highbatch_20260319_201037 \
DAG_UNIX=/tmp/dag_rpc_safe_highbatch_20260319_201037.sock \
DAG_STORE=log DAG_PERSIST_BLOCKS=1 DAG_LOG_FSYNC_EVERY=1 DAG_CHECKPOINT_EVERY=1500 \
DAG_MAX_TX_PER_BATCH=5000000000000000000 DAG_MAX_RANGE_COUNT=5000000000000000000 \
NATIVE_SIG_VERIFY=1 NATIVE_SIG_REQUIRE=1 \
./run_dag_profile_lowbps.sh

./go_dag/stress \
  --rpc msgpack+unix:///tmp/dag_rpc_safe_highbatch_20260319_201037.sock \
  --workers 2 \
  --soak --tps 550000000000000000000 --duration 15s \
  --batch-size-big 5000000000000000000 \
  --mine-range --sign --sig-scheme ed25519 \
  --range-mode direct --range-root range --assume-new
```

## Evidence Bundle
- `benchmarks/all_runs/bench_runs/native_safe_1w_15bps_bigint_hi_20260319_175137.log`
- `benchmarks/all_runs/bench_runs/native_2w_550bps_bigblk_FINAL_20260319_180047.log`
- `benchmarks/all_runs/bench_runs/native_unique_2w550bps_big_20260319_182803.log`
- `benchmarks/all_runs/bench_runs/bigint_highbatch_refresh_20260319_201037/summary_bigint_highbatch.json`
- `benchmarks/all_runs/bench_runs/bigint_highbatch_refresh_20260319_201037/safe_highbatch_stress.log`

## Note
Metrics are not apples-to-apples across lanes (`tx/s`, `ops/s`, `calls/s`, `rps`) and are reported per-lane in their own safe profile.
