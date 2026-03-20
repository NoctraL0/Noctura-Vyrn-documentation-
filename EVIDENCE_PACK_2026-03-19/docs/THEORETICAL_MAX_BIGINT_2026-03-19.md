# Theoretical Max (BigInt + Larger Blocks) - 2026-03-19

## Headline Result
Fresh theoretical rerun (with much larger blocks) reached:
- `2,191,865,149,215,769,100,288.00 tx/s` (`~2.1919e21`)
- `avg_txs_per_block = 9,000,000,000,000,000,000`

This is a new high-block-size theoretical run, not the previous exact shape.

## Scope and Honesty
- This benchmark is theoretical only (non-launch-safe).
- It uses memory-store profile and disabled durability/safety gates to find ceiling.
- It is not an EVM opcode TPS claim.

## Theoretical Profile Used
- `DAG_STORE=memory`
- `DAG_PERSIST_BLOCKS=0`
- `DAG_LOG_FSYNC_EVERY=0`
- `DAG_CHECKPOINT_EVERY=0`
- `DAG_FAST=1`, `DAG_BENCH=1`, `DAG_AUTO_RUN=0`
- `DAG_RATE_LIMIT_ENABLED=0`
- `NATIVE_SIG_VERIFY=0`, `NATIVE_SIG_REQUIRE=0`
- `DAG_ENFORCE_VALIDATOR_QUORUM=0`, `DAG_ENFORCE_PARENT_QUORUM=0`, `DAG_ENFORCE_CHILD_QUORUM=0`
- `DAG_EXTERNAL_PARENT_QC=1`, `DAG_VALIDATORS_REQUIRE_PRIV=0`
- Big-block limits enabled:
  - `DAG_MAX_TX_PER_BATCH=9000000000000000000`
  - `DAG_MAX_RANGE_COUNT=9000000000000000000`

## Fresh Big-Block Cases

| Case | Workers | Target TPS | Batch Size | Measured TPS | Avg TX/Block |
|---|---:|---:|---:|---:|---:|
| `w4_t1200e18_b6e18` | 4 | `1.2e21` | `6e18` | `1,199,787,582,007,826,710,528.00` | `6e18` |
| `w4_t1800e18_b8e18` | 4 | `1.8e21` | `8e18` | `1,799,153,673,314,329,952,256.00` | `8e18` |
| `w2_t2200e18_b9e18` | 2 | `2.2e21` | `9e18` | `2,191,865,149,215,769,100,288.00` | `9e18` |

Best case in this rerun:
- `theory_w2_t2200e18_b9e18.log`

## Command Pattern Used

```bash
DATA_DIR=/tmp/vyrn_theory_highbatch_20260319_201037 \
DAG_UNIX=/tmp/dag_rpc_theory_highbatch_20260319_201037.sock \
GO_RANGE_STATE=1 DAG_GO_RANGE_ROOTS=1 DAG_GO_RAW_PARSE=1 DAG_GO_RAW_PARSE_VERIFY=0 DAG_GO_RAW_PARSE_FALLBACK=1 \
DAG_FAST=1 DAG_BENCH=1 DAG_AUTO_RUN=0 \
DAG_STORE=memory DAG_PERSIST_BLOCKS=0 DAG_TRACK_RECENT=0 DAG_SIG_STATS=0 DAG_RATE_LIMIT_ENABLED=0 \
DAG_MAX_TX_PER_BATCH=9000000000000000000 DAG_MAX_RANGE_COUNT=9000000000000000000 DAG_RANGE_KEY_MOD=200000 \
DAG_LOG_FSYNC_EVERY=0 DAG_CHECKPOINT_EVERY=0 \
NATIVE_SIG_VERIFY=0 NATIVE_SIG_REQUIRE=0 \
DAG_ENFORCE_VALIDATOR_QUORUM=0 DAG_ENFORCE_PARENT_QUORUM=0 DAG_ENFORCE_CHILD_QUORUM=0 \
DAG_EXTERNAL_PARENT_QC=1 DAG_VALIDATORS_REQUIRE_PRIV=0 \
./run_dag_rpc.sh

./go_dag/stress \
  --rpc msgpack+unix:///tmp/dag_rpc_theory_highbatch_20260319_201037.sock \
  --workers 2 \
  --soak --tps 2200000000000000000000 --duration 12s \
  --batch-size-big 9000000000000000000 \
  --mine-range --range-mode direct --range-root range --assume-new
```

## Evidence Bundle
- `benchmarks/all_runs/bench_runs/bigint_highbatch_refresh_20260319_201037/summary_bigint_highbatch.json`
- `benchmarks/all_runs/bench_runs/bigint_highbatch_refresh_20260319_201037/theory_w4_t1200e18_b6e18.log`
- `benchmarks/all_runs/bench_runs/bigint_highbatch_refresh_20260319_201037/theory_w4_t1800e18_b8e18.log`
- `benchmarks/all_runs/bench_runs/bigint_highbatch_refresh_20260319_201037/theory_w2_t2200e18_b9e18.log`
