# Safe Max Snapshot (All Lanes, Big-Block Refresh) - 2026-03-25 refresh

## Headline
Launch-safe native has two validated safe classes in this pack:
- stable baseline around `5.456e20 tx/s` (1 worker shape)
- high-throughput peak up to `2.7496e22 tx/s` (2 workers, max big-block shape)
- strict counter-integrity class up to `4.289e32 tx/s` (nonce-sync + unique sender pool, saturation shape)
- strict BigInt order-sweep class up to `~1.089e513 tx/s` (signed + durable profile, non-node-safe BPS stress shape)

## Scope
- This file is launch-safe baseline evidence.
- Native lane includes baseline-safe, peak-safe, and strict-integrity profiles.
- Non-native lane values are the highest safe baselines currently recorded in this public pack.

## Highest Safe Numbers (Current Baseline + Strict Integrity)

| Lane | Best Safe Metric | Evidence |
|---|---:|---|
| Native signed range (durable baseline, 1 worker) | `545,623,145,378,860,630,016.00 tx/s` | `benchmarks/all_runs/bench_runs/native_safe_1w_15bps_bigint_hi_20260319_175137.log` |
| Native signed range (durable peak, 2 workers, non-unique) | `27,495,991,062,506,292,379,648.00 tx/s` | `benchmarks/all_runs/bench_runs/native_2w_550bps_bigblk_FINAL_20260319_180047.log` |
| Native signed range (durable peak, unique senders) | `27,348,041,002,392,110,497,792.00 tx/s` | `benchmarks/all_runs/bench_runs/native_unique_2w550bps_big_20260319_182803.log` |
| Native signed range (durable strict integrity, 10-BPS tuned) | `6,425,421,127,134,743,566,077,883,580,416.00 tx/s` (`~6.425e30`) | `benchmarks/all_runs/bench_runs/strict_no_doublecount_20260320_031126.log` |
| Native signed range (durable strict integrity, saturation) | `428,869,442,090,321,794,019,470,256,111,616.00 tx/s` (`~4.289e32`) | `benchmarks/all_runs/bench_runs/strict_no_doublecount_saturation_20260320_031157.log` |
| Native signed range (durable strict integrity, BigInt sweep e64 class) | `47,988,080,403,867,954,877,001,267,244,386,490,545,343,709,017,019,851,481,418,825,728.00 tx/s` (`~4.799e64`) | `benchmarks/all_runs/bench_runs/native_bigint_order_sweep_20260325/native_max_tps_20260325_202953.log` |
| Native signed range (durable strict integrity, BigInt sweep e128 class) | `779,787,948,286,304,052,347,352,079,276,055,395,744,626,439,611,832,122,972,740,039,848,233,739,288,129,295,259,788,203,451,820,604,207,813,044,860,847,078,225,695,735,808.00 tx/s` (`~7.798e128`) | `benchmarks/all_runs/bench_runs/native_bigint_order_sweep_20260325/native_max_tps_20260325_203238_e128.log` |
| Native signed range (durable strict integrity, BigInt sweep e512 class) | `10,892,805,346,129,397,837,772,834,905,348,366,311,152,353,739,690,085,357,404,997,837,759,829,549,492,672,436,781,102,673,107,259,257,499,057,706,655,651,857,858,036,431,714,219,237,402,434,597,700,824,135,562,205,686,229,709,285,310,504,301,032,810,825,602,318,065,864,011,744,338,507,977,220,831,737,277,046,902,736,057,460,400,207,343,787,560,383,321,431,620,174,448,005,212,729,502,508,789,216,460,574,540,495,880,540,700,410,803,823,184,783,604,982,596,073,392,129,658,151,798,236,443,367,032,362,870,111,177,095,113,385,883,263,629,590,425,927,788,139,913,723,354,312,780,360,880,815,898,754,637,407,928,177,138,518,084,893,218,890,877,227,837,150,333,355,290,832,014,626,564,82.77 tx/s` (`~1.089e513`) | `benchmarks/all_runs/bench_runs/native_bigint_order_sweep_20260325/native_max_tps_20260325_203807_e512.log` |
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
- `benchmarks/all_runs/bench_runs/strict_no_doublecount_20260320_031126.log`
- `benchmarks/all_runs/bench_runs/strict_no_doublecount_saturation_20260320_031157.log`
- `benchmarks/all_runs/bench_runs/native_bigint_order_sweep_20260325/summary_native_bigint_order_sweep_20260325.json`
- `benchmarks/all_runs/bench_runs/native_bigint_order_sweep_20260325/native_max_tps_20260325_202953.log`
- `benchmarks/all_runs/bench_runs/native_bigint_order_sweep_20260325/native_max_tps_20260325_203238_e128.log`
- `benchmarks/all_runs/bench_runs/native_bigint_order_sweep_20260325/native_max_tps_20260325_203807_e512.log`
- `benchmarks/all_runs/bench_runs/bigint_highbatch_refresh_20260319_201037/summary_bigint_highbatch.json`
- `benchmarks/all_runs/bench_runs/bigint_highbatch_refresh_20260319_201037/safe_highbatch_stress.log`
- `docs/NATIVE_STRICT_NO_DOUBLECOUNT_VALIDATION_2026-03-20.md`
- `docs/NATIVE_STRICT_BIGINT_ORDER_SWEEP_2026-03-25.md`

## Note
Metrics are not apples-to-apples across lanes (`tx/s`, `ops/s`, `calls/s`, `rps`) and are reported per-lane in their own safe profile.
