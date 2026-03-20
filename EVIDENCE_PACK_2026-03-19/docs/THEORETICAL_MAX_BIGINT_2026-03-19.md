# Theoretical Max (BigInt + Larger Blocks) - 2026-03-19

## Headline Result
Latest theoretical max in this pack is:
- `99,427,130,461,936,492,216,320.00 tx/s` (`~9.943e22`)
- `avg_txs_per_block = 50,000,000,000,000,000,000`
- case: `theory_w2_t100000e18_b50e18`, guardrails OFF

This supersedes the earlier `~6.973e21` e22 guardrails-off result in this file.

Strict integrity reference (separate from theoretical methodology):
- `428,869,442,090,321,794,019,470,256,111,616.00 tx/s` (`~4.289e32`)
- case: `strict_no_doublecount_saturation_20260320_031157`
- profile class: durable + signed + `nonce-sync` + unique sender pool

## Scope and Honesty
- This benchmark is theoretical only (non-launch-safe).
- It uses memory-store profile and disabled durability/safety gates to find ceiling.
- It is not an EVM opcode TPS claim.
- Pack-level maximum magnitude can be higher under strict launch-safe saturation profiles; those are tracked in `SAFE_MAX_ALL_LANES_BIGINT_2026-03-19.md` and `NATIVE_STRICT_NO_DOUBLECOUNT_VALIDATION_2026-03-20.md`.

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
  - `DAG_MAX_TX_PER_BATCH=50000000000000000000`
  - `DAG_MAX_RANGE_COUNT=50000000000000000000`

## Fresh Big-Block Cases

| Case | Workers | Target TPS | Batch Size | Measured TPS | Avg TX/Block | Notes |
|---|---:|---:|---:|---:|---:|---|
| `theory_w2_t100000e18_b50e18` | 2 | `1.0e23` | `5e19` | `99,427,130,461,936,492,216,320.00` | `5e19` | **best overall in this pack** |
| `theory_w2_t55000e18_b50e18` | 2 | `5.5e22` | `5e19` | `54,688,541,410,705,190,944,768.00` | `5e19` | same profile family |
| `theory_w2_t27500e18_b50e18` | 2 | `2.75e22` | `5e19` | `27,341,212,203,569,106,649,088.00` | `5e19` | same profile family |
| `e22_guardrails_off` | 2 | `1.0e22` | `9e18` | `6,973,281,673,524,083,163,136.00` | `9e18` | prior theoretical reference |
| `w4_t1200e18_b6e18` | 4 | `1.2e21` | `6e18` | `1,199,787,582,007,826,710,528.00` | `6e18` | prior BigInt sweep |
| `w4_t1800e18_b8e18` | 4 | `1.8e21` | `8e18` | `1,799,153,673,314,329,952,256.00` | `8e18` | prior BigInt sweep |
| `w2_t2200e18_b9e18` | 2 | `2.2e21` | `9e18` | `2,191,865,149,215,769,100,288.00` | `9e18` | prior BigInt sweep |

Best case right now:
- `theory_w2_t100000e18_b50e18.log` from `theory_bigbatch_push_20260319_205422`

## Command Pattern Used

```bash
DATA_DIR=/tmp/vyrn_theory_bigbatch_20260319_205422 \
DAG_UNIX=/tmp/dag_rpc_theory_bigbatch_20260319_205422.sock \
GO_RANGE_STATE=1 DAG_GO_RANGE_ROOTS=1 DAG_GO_RAW_PARSE=1 DAG_GO_RAW_PARSE_VERIFY=0 DAG_GO_RAW_PARSE_FALLBACK=1 \
DAG_FAST=1 DAG_BENCH=1 DAG_AUTO_RUN=0 \
DAG_STORE=memory DAG_PERSIST_BLOCKS=0 DAG_TRACK_RECENT=0 DAG_SIG_STATS=0 DAG_RATE_LIMIT_ENABLED=0 \
DAG_MAX_TX_PER_BATCH=50000000000000000000 DAG_MAX_RANGE_COUNT=50000000000000000000 DAG_RANGE_KEY_MOD=200000 \
DAG_LOG_FSYNC_EVERY=0 DAG_CHECKPOINT_EVERY=0 \
NATIVE_SIG_VERIFY=0 NATIVE_SIG_REQUIRE=0 \
DAG_ENFORCE_VALIDATOR_QUORUM=0 DAG_ENFORCE_PARENT_QUORUM=0 DAG_ENFORCE_CHILD_QUORUM=0 \
DAG_EXTERNAL_PARENT_QC=1 DAG_VALIDATORS_REQUIRE_PRIV=0 \
./run_dag_rpc.sh

./go_dag/stress \
  --rpc msgpack+unix:///tmp/dag_rpc_theory_bigbatch_20260319_205422.sock \
  --workers 2 \
  --senders-per-worker 4000 \
  --soak --tps 100000000000000000000000 --duration 10s \
  --batch-size-big 50000000000000000000 \
  --mine-range --sign --sig-scheme ed25519 \
  --range-mode direct --range-root range --assume-new \
  --stop-on-reject --progress-every 100 --lat-samples 20000
```

## Evidence Bundle
- `benchmarks/all_runs/bench_runs/theory_bigbatch_push_20260319_205422/summary_theory_bigbatch_push.json`
- `benchmarks/all_runs/bench_runs/theory_bigbatch_push_20260319_205422/theory_w2_t100000e18_b50e18.log`
- `benchmarks/all_runs/bench_runs/theory_bigbatch_push_20260319_205422/theory_w2_t55000e18_b50e18.log`
- `benchmarks/all_runs/bench_runs/theory_bigbatch_push_20260319_205422/theory_w2_t27500e18_b50e18.log`
- `benchmarks/all_runs/bench_runs/e22_guardrails_ab_20260319_201707/summary_e22_guardrails_compare.json`
- `benchmarks/all_runs/bench_runs/e22_guardrails_ab_20260319_201707/guardrails_on_stress.log`
- `benchmarks/all_runs/bench_runs/e22_guardrails_ab_20260319_201707/guardrails_off_stress.log`
- `benchmarks/all_runs/bench_runs/strict_no_doublecount_saturation_20260320_031157.log` (strict integrity reference)
- `docs/NATIVE_STRICT_NO_DOUBLECOUNT_VALIDATION_2026-03-20.md`
