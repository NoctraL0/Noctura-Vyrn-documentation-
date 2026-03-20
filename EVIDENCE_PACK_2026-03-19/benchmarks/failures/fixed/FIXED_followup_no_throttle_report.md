# No-Throttle All-Lane Throughput Run (WAL + fsync + Snapshot Settings ON)

Date: 2026-03-19
Run dir: `<REPO_ROOT>/bench_runs/all_lanes_no_throttle_walfsync_snapshot_1m_20260319_014353`

## Profile
- `STACK_PROFILE=strict`
- Rate limits: OFF (`DAG_RATE_LIMIT_ENABLED=0`, `RPC_RATE_LIMIT_ENABLED=0`, `L2_RATE_LIMIT_ENABLED=0`)
- Persistence safety toggles kept ON:
  - `DAG_STORE=log`
  - `DAG_PERSIST_BLOCKS=1`
  - `DAG_LOG_FSYNC_EVERY=1`
  - `DAG_BLOCK_FSYNC=1`
  - `DAG_CHECKPOINT_EVERY=200`
  - `MEMPOOL_INMEM=0`
  - `MEMPOOL_JOURNAL=1`
  - `MEMPOOL_SNAPSHOT_SECS=30`
  - `WAL_ENABLED=1`
  - `WAL_FSYNC_EVERY=1`
  - `WAL_CHECKPOINT_EVERY=200`
  - `SNAPSHOT_EVERY=200`
  - `SNAPSHOT_FSYNC=1`

## Lane Results (Concurrent, 60s)

### Native (GoDAG signed range lane)
Source: `logs/native_stress.log`
- `txs_committed`: `4,180,000`
- `blocks_mined`: `1,045`
- `tx_per_second`: `69,608.52`
- `blocks_per_second`: `17.40`
- `avg_txs_per_block`: `4000.00`
- latency: p50 `56.755ms`, p90 `78.368ms`, p99 `95.252ms`
- status: clean completion (no nonce abort)

### EVM+WASM compat mix
Source: `logs/compat_mix.json`
- `calls_ok`: `5,782`
- `calls_err`: `0`
- `calls_per_sec`: `96.34`
- latency: p50 `11.63ms`, p90 `349.37ms`, p99 `372.53ms`

### BPF
Source: `logs/bpf_lane.json`
- `calls_ok`: `103,256`
- `calls_err`: `0`
- `calls_per_sec`: `1,720.69`
- latency: p50 `9.19ms`, p90 `10.99ms`, p99 `13.30ms`

### L2 mixed
Source: `logs/l2_stress.json`
- `requests_total`: `34,055`
- `req_per_sec`: `567.31`
- `transport_err`: `0`
- `app_ok`: `18,446`
- `app_reject`: `15,609`
- top app reject: `channel defi at capacity` (not RPC throttling)
- latency: p50 `26.75ms`, p90 `33.75ms`, p99 `43.93ms`

## What Changed vs Throttled Run
- No `HTTP 429` throttling pattern from RPC/L2 rate-limiters.
- Native lane remained stable for full minute with multi-sender config.
- Remaining L2 rejects are functional/channel-capacity limits, not global RPC throttling.

## Persistence Artifacts
- Present:
  - `chain_data/main_wal.log` (exists)
  - `chain_data/dag_parents.log` + snapshots/backups
  - `chain_data/dag_blocks/*.mpk` (persisted block files)
  - `chain_data/inbox.log`, `chain_data/outbox.log`, `chain_data/batches.log`
- Observed in this run:
  - `main_wal.log` exists but remained `0` bytes during this specific workload window.
  - `main_wal_checkpoint.json` and `main_snapshot.json` were not emitted in-window.

Interpretation:
- WAL/fsync/snapshot knobs were enabled, and DAG persistence is clearly active.
- Main-RPC WAL/snapshot artifacts did not advance under this particular call mix/window.

## Machine-Readable
- `summary.json`:
  - `<REPO_ROOT>/bench_runs/all_lanes_no_throttle_walfsync_snapshot_1m_20260319_014353/summary.json`
