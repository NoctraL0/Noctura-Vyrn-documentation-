# Unique Sender Native Benchmarks (2026-03-19)

This document records the two native stress tests rerun with unique sender pools.

## Definition Used

"All unique senders" for this run means no sender reuse during each benchmark window:
- Test 1 used `--senders-per-worker 1000` (1 worker).
- Test 2 used `--senders-per-worker 4000` (2 workers).

All runs used safe launch settings (durable log store, signed lane, quorum checks on, persisted blocks enabled).

## Runner

- RPC socket: `/tmp/dag_rpc_safe_run_unique_1773970052.sock`
- Data dir: `/tmp/vyrn_safe_run_unique_1773970052`
- Persisted blocks: `/tmp/vyrn_safe_run_unique_1773970052/dag_blocks` (5620 files)

## Test 1: 1 Worker, ~15 BPS, Big Block

Command:

```bash
cd <REPO_ROOT>/go_dag
./stress \
  --rpc msgpack+unix:///tmp/dag_rpc_safe_run_unique_1773970052.sock \
  --workers 1 \
  --senders-per-worker 1000 \
  --sender-base addr:uniq1_20260319_182744 \
  --soak --tps 545700000000000000000 --duration 10s \
  --batch-size-big 36380000000000000000 \
  --mine-range --sign --sig-scheme ed25519 \
  --range-mode direct --range-root range --assume-new \
  --stop-on-reject --progress-every 20 --lat-samples 20000
```

Result:
- `tx_per_second`: `544883412544347439104.00` (~`5.4488e20`)
- `blocks_per_second`: `14.98`
- `txs_committed`: `5457000000000000000000`
- Log: `<REPO_ROOT>/bench_runs/native_unique_1w15bps_big_20260319_182744.log`

## Test 2: 2 Workers, ~550 BPS, Max Big Block

Command:

```bash
cd <REPO_ROOT>/go_dag
./stress \
  --rpc msgpack+unix:///tmp/dag_rpc_safe_run_unique_1773970052.sock \
  --workers 2 \
  --senders-per-worker 4000 \
  --sender-base addr:uniq2_20260319_182803 \
  --soak --tps 27500000000000000000000 --duration 10s \
  --batch-size-big 50000000000000000000 \
  --mine-range --sign --sig-scheme ed25519 \
  --range-mode direct --range-root range --assume-new \
  --stop-on-reject --progress-every 50 --lat-samples 20000
```

Result:
- `tx_per_second`: `27348041002392110497792.00` (~`2.7348e22`)
- `blocks_per_second`: `546.96`
- `txs_committed`: `273500000000000000000000`
- Log: `<REPO_ROOT>/bench_runs/native_unique_2w550bps_big_20260319_182803.log`

## Comparison vs Non-Unique Sender Baselines

1) 1-worker test:
- Non-unique baseline: `545623145378860630016.00`
- Baseline log: `<REPO_ROOT>/bench_runs/native_safe_1w_15bps_bigint_hi_20260319_175137.log`
- Unique sender run: `544883412544347439104.00`
- Delta: `-0.136%`

2) 2-worker test:
- Non-unique baseline: `27495991062506292379648.00`
- Baseline log: `<REPO_ROOT>/bench_runs/native_2w_550bps_bigblk_FINAL_20260319_180047.log`
- Unique sender run: `27348041002392110497792.00`
- Delta: `-0.538%`

Conclusion: enforcing large unique-sender pools caused only a small throughput reduction.
