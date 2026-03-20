# Vyrn-Chain Public Benchmark Evidence (AI-Safe)

Date: 2026-03-18
Benchmark class: theoretical maximum (non-launch-safe)
Execution lane: native range lane (`mineRange` / `mineRangeSigned` style stress path)

## Headline Result
Peak measured throughput in this run set:
- `1,199,912,824,653,410,992,128.00 tx/s` (`~1.1999e21`)

## Scope and Honesty Notes
- This benchmark is for theoretical ceiling only.
- This is not the launch-safe profile.
- This is not EVM opcode TPS.
- This profile uses in-memory state and disables durability and several enforcement gates to find upper bounds.

## Theoretical Profile Used
- `GO_RANGE_STATE=1`
- `DAG_FAST=1`, `DAG_BENCH=1`, `DAG_AUTO_RUN=0`
- `DAG_STORE=memory`
- `DAG_PERSIST_BLOCKS=0`
- `DAG_LOG_FSYNC_EVERY=0`
- `DAG_CHECKPOINT_EVERY=0`
- `DAG_RATE_LIMIT_ENABLED=0`
- `DAG_MAX_TX_PER_BATCH=1000000000000000000`
- `DAG_MAX_RANGE_COUNT=1000000000000000000`
- `DAG_RANGE_KEY_MOD=200000`
- `NATIVE_SIG_VERIFY=0`, `NATIVE_SIG_REQUIRE=0`
- `DAG_ENFORCE_VALIDATOR_QUORUM=0`, `DAG_ENFORCE_PARENT_QUORUM=0`, `DAG_ENFORCE_CHILD_QUORUM=0`
- `DAG_EXTERNAL_PARENT_QC=1`, `DAG_VALIDATORS_REQUIRE_PRIV=0`

## Results (Same Profile)
- `unsigned_w4_t1200e18`: `1,199,912,824,653,410,992,128.00 tx/s`, `1199.91 blocks/s`, `p99=0.000562s` (best overall)
- `unsigned_w2_t1200e18`: `1,199,827,843,901,645,651,968.00 tx/s`, `1199.83 blocks/s`, `p99=0.000361s`
- `signed_w2_t1200e18`: `1,198,227,978,916,485,660,672.00 tx/s`, `1198.23 blocks/s`, `p99=0.000419s`
- `unsigned_w1_t1000e18`: `999,876,694,206,193,729,536.00 tx/s`, `999.88 blocks/s`, `p99=0.000255s`
- `signed_w2_t900e18`: `898,981,658,144,420,003,840.00 tx/s`, `898.98 blocks/s`, `p99=0.000403s`
- `signed_w1_t900e18`: `898,160,915,360,158,842,880.00 tx/s`, `898.16 blocks/s`, `p99=0.000302s`

## Evidence Folder
- `bench_runs/theoretical_max_km200k_20260318_010845/`

## SHA-256 Checksums
- `a7f7c8617df87eb6721002ff17217120fcac5f482cfdd23203fdd9f9f3a5f37f`  `dag.log`
- `05596328823cf739dbfcbe383e128ce9d2f03cae56166b23c0b8870b8fffa453`  `signed_w1_t900e18.log`
- `3c8cf88b48a0340d8d65616921b2fbade507f112a37ec7363b11e5b6738cc0eb`  `signed_w2_t1200e18.log`
- `c7286078a60fca88036702e9e5539f2ec37fd449b6051001df4fe9d66385f845`  `signed_w2_t900e18.log`
- `0edc577ffe6d263278ceb7cbd11c6fb7dbf94c8bb05f436fb0dd6697a913da0d`  `unsigned_w1_t1000e18.log`
- `985de60d4ce48f6d2dbf4b1c306dd7c54d71cd2bb1e518963de269408ecdd208`  `unsigned_w2_t1200e18.log`
- `aed95bde2e838d67195f187bc40936b1970f06a2639d0406ed87a834f62f6398`  `unsigned_w4_t1200e18.log`
- `d3b089f5625ba8eb86180eacbe54186e7302bbe67f80cc4769bced4a24e30065`  `summary.txt`

## Repro Command Pattern (Theoretical)
```bash
cd /path/to/Vyrn-Chain

DATA_DIR=bench_runs/theoretical_repro_data \
DAG_UNIX=/tmp/dag_rpc_theoretical_repro.sock \
DAG_HOST=127.0.0.1 DAG_PORT=19628 \
GO_RANGE_STATE=1 DAG_GO_RANGE_ROOTS=1 DAG_GO_RAW_PARSE=1 DAG_GO_RAW_PARSE_VERIFY=0 DAG_GO_RAW_PARSE_FALLBACK=1 \
DAG_FAST=1 DAG_BENCH=1 DAG_AUTO_RUN=0 \
DAG_STORE=memory DAG_PERSIST_BLOCKS=0 \
DAG_TRACK_RECENT=0 DAG_SIG_STATS=0 DAG_RATE_LIMIT_ENABLED=0 \
DAG_MAX_TX_PER_BATCH=1000000000000000000 DAG_MAX_RANGE_COUNT=1000000000000000000 DAG_RANGE_KEY_MOD=200000 \
DAG_LOG_FSYNC_EVERY=0 DAG_CHECKPOINT_EVERY=0 \
NATIVE_SIG_VERIFY=0 NATIVE_SIG_REQUIRE=0 \
DAG_ENFORCE_VALIDATOR_QUORUM=0 DAG_ENFORCE_PARENT_QUORUM=0 DAG_ENFORCE_CHILD_QUORUM=0 \
DAG_EXTERNAL_PARENT_QC=1 DAG_VALIDATORS_REQUIRE_PRIV=0 \
./run_dag_rpc.sh

# second terminal
./go_dag/stress \
  --rpc msgpack+unix:///tmp/dag_rpc_theoretical_repro.sock \
  --workers 4 \
  --sender-base addr:theory_public \
  --soak --tps 1200000000000000000000 --duration 10s \
  --batch-size 1000000000000000000 \
  --mine-range \
  --range-mode direct --range-root range --assume-new \
  --stop-on-reject --progress-every 20 --lat-samples 50000
```

## Public-Safe Redaction Policy Applied
- No private keys, auth tokens, cookies, wallet seed phrases, or secrets included.
- No personal absolute home directory paths required for reproduction.
- No host-specific identity metadata included.

