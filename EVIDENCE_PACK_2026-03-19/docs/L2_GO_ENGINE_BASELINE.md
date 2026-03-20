# L2 Go Engine Baseline

Use this as the default template for automation runs.

## 1) Start Stack (Go L2 + Python fallback sidecar)

```bash
cd <REPO_ROOT>
DATA_DIR=chain_data \
L2_ENGINE=go \
START_L2=1 \
START_L2_FALLBACK=1 \
L2_FALLBACK_PORT=8661 \
L2_FALLBACK_UNKNOWN=0 \
./run_stack.sh
```

Notes:
- `L2_FALLBACK_UNKNOWN=0` keeps fallback to critical/internal failure paths only.
- Fallback sidecar runs on `127.0.0.1:8661` by default when enabled.

## 2) Health Checks

```bash
curl -sS -X POST http://127.0.0.1:8660 \
  -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"health","params":[]}'
```

```bash
curl -sS -X POST http://127.0.0.1:8660 \
  -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":2,"method":"rpc_metrics","params":[]}'
```

## 3) Fastlane Throughput Probe

```bash
cd <REPO_ROOT>
./run_l2_fastbench_go.sh \
  -transport fastlane \
  -unix /tmp/vyrn_l2_fastlane.sock \
  -mode health \
  -workers 8 \
  -duration 10s
```

## 4) Max Safe Throughput Profile (Go L2 Only)

Launch-safe profile used for high-throughput soak while keeping WAL + fsync + snapshot enabled:

```bash
cd <REPO_ROOT>
DATA_DIR=chain_data \
L2_ENGINE=go \
START_L2=1 \
START_L2_FALLBACK=1 \
L2_FALLBACK_PORT=8661 \
L2_FALLBACK_UNKNOWN=0 \
L2_KEEP_HISTORY=0 \
L2_WAL_SHARDS=4 \
L2_WAL_FSYNC_EVERY=16 \
L2_WAL_FSYNC_INTERVAL_MS=20 \
L2_WAL_COMMIT_WINDOW_MS=2 \
L2_WAL_COMMIT_MAX_OPS=131072 \
L2_WAL_COMMIT_MAX_BYTES=268435456 \
L2_WAL_PENDING_MAX=4000000 \
L2_MAX_INFLIGHT=8192 \
./run_stack.sh
```

Mixed-mode stress probe (safe profile):

```bash
cd <REPO_ROOT>
./run_l2_fastbench_go.sh \
  -transport fastlane \
  -wire packed \
  -id-mode blank \
  -unix /tmp/vyrn_l2_fastlane.sock \
  -mode mixed \
  -workers 2048 \
  -inflight-per-worker 48 \
  -duration 20s
```

Observed ceiling on this host with the safe profile:
- Mixed: ~`7.95e5` ops/sec
- Outbox-only: ~`8.64e5` ops/sec
- Health (read path): ~`2.86e6` ops/sec

## 5) Optional Compatibility Fallback for Unknown Methods

Only use if needed during migration:

```bash
L2_FALLBACK_UNKNOWN=1 ./run_stack.sh
```

## 6) Stop Stack

```bash
cd <REPO_ROOT>
./stop_stack.sh
```
