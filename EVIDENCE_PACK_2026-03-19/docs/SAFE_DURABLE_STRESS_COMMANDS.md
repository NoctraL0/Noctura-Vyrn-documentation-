# Safe Durable Signed Stress Commands

Use this for repeatable `ed25519` signed stress runs with durable storage.

## 1) Start DAG (durable + signed safety on)

```bash
cd <REPO_ROOT>

DATA_DIR=/tmp/vyrn_safe_run \
DAG_UNIX=/tmp/dag_rpc_safe_run.sock \
DAG_HOST=127.0.0.1 \
DAG_PORT=18705 \
DAG_STORE=log \
DAG_PERSIST_BLOCKS=1 \
DAG_LOG_FSYNC_EVERY=5 \
DAG_CHECKPOINT_EVERY=1500 \
DAG_RATE_LIMIT_ENABLED=0 \
DAG_MAX_TX_PER_BATCH=1000000000000000000 \
DAG_MAX_RANGE_COUNT=1000000000000000000 \
NATIVE_SIG_VERIFY=1 \
NATIVE_SIG_REQUIRE=1 \
DAG_ENFORCE_VALIDATOR_QUORUM=1 \
DAG_ENFORCE_PARENT_QUORUM=1 \
DAG_ENFORCE_CHILD_QUORUM=1 \
./run_dag_profile_lowbps.sh
```

## 2) Run signed stress (new terminal)

```bash
cd <REPO_ROOT>/go_dag

./stress \
  --rpc msgpack+unix:///tmp/dag_rpc_safe_run.sock \
  --workers 2 \
  --sender-base addr:safe_$(date +%s) \
  --soak --tps 550000000000000000000 --duration 10s \
  --batch-size 1000000000000000000 \
  --mine-range --sign --sig-scheme ed25519 \
  --range-mode direct --range-root range --assume-new \
  --stop-on-reject --progress-every 20 --lat-samples 50000
```

## 3) Check result quickly

```bash
# If output is logged to stress.log:
awk -F': ' '/tx_per_second|blocks_per_second|txs_committed/{print}' stress.log
```

## 4) Verify blocks were written (durability check)

```bash
ls -la /tmp/vyrn_safe_run
ls -la /tmp/vyrn_safe_run/dag_blocks | head -n 20
find /tmp/vyrn_safe_run/dag_blocks -type f | wc -l
```

## 5) Optional durable sweeps

```bash
# More workers
./stress --rpc msgpack+unix:///tmp/dag_rpc_safe_run.sock --workers 4 --sender-base addr:safe4_$(date +%s) --soak --tps 550000000000000000000 --duration 10s --batch-size 1000000000000000000 --mine-range --sign --sig-scheme ed25519 --range-mode direct --range-root range --assume-new --stop-on-reject

# Heavier target
./stress --rpc msgpack+unix:///tmp/dag_rpc_safe_run.sock --workers 2 --sender-base addr:safe2h_$(date +%s) --soak --tps 700000000000000000000 --duration 10s --batch-size 1000000000000000000 --mine-range --sign --sig-scheme ed25519 --range-mode direct --range-root range --assume-new --stop-on-reject
```

## 6) Strict no-double-count validation (BigInt, 10-BPS tuned)

Use this when you want stricter counter confidence (nonce sync + larger sender pool).

DAG launch requirement for this profile:
- `DAG_MAX_TX_PER_BATCH >= 650000000000000000000000000000`
- `DAG_MAX_RANGE_COUNT >= 650000000000000000000000000000`

```bash
cd <REPO_ROOT>/go_dag

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

## 7) Stop DAG

```bash
pkill -f "dag_rpc_server.py --host 127.0.0.1 --port 18705" || true
rm -f /tmp/dag_rpc_safe_run.sock
```
