# Native Strict BigInt Order Sweep - 2026-03-25

## Purpose
Record the 2026-03-25 strict-integrity native sweep where batch and TPS magnitudes were increased by orders of magnitude while keeping signed + durable profile settings.

## Profile class
- store: `log`
- persist blocks: enabled
- fsync cadence: `DAG_LOG_FSYNC_EVERY=5`
- checkpoints: `DAG_CHECKPOINT_EVERY=1500`
- signature verification and requirement: enabled
- validator/parent/child quorum enforcement: enabled

Note:
- These runs are strict-integrity and durable, but the observed BPS is not node-safe for normal deployment.
- Treat this as a mathematical envelope test for counter integrity and BigInt range handling.

## Result table

| Run | Target TPS (shape) | Batch Size | Measured TPS | BPS | p99 |
|---|---:|---:|---:|---:|---:|
| `native_max_tps_20260325_202953` | `4.8e64` class | `8e64` class | `47,988,080,403,867,954,877,001,267,244,386,490,545,343,709,017,019,851,481,418,825,728.00` (`~4.799e64`) | `599.85` | `0.029403s` |
| `native_max_tps_20260325_203238_e128` | `7.8e128` class | `1.3e128` class | `779,787,948,286,304,052,347,352,079,276,055,395,744,626,439,611,832,122,972,740,039,848,233,739,288,129,295,259,788,203,451,820,604,207,813,044,860,847,078,225,695,735,808.00` (`~7.798e128`) | `599.84` | `0.028049s` |
| `native_max_tps_20260325_203807_e512` | `7.8e512` class | `1.3e512` class | `10,892,805,346,129,397,837,772,834,905,348,366,311,152,353,739,690,085,357,404,997,837,759,829,549,492,672,436,781,102,673,107,259,257,499,057,706,655,651,857,858,036,431,714,219,237,402,434,597,700,824,135,562,205,686,229,709,285,310,504,301,032,810,825,602,318,065,864,011,744,338,507,977,220,831,737,277,046,902,736,057,460,400,207,343,787,560,383,321,431,620,174,448,005,212,729,502,508,789,216,460,574,540,495,880,540,700,410,803,823,184,783,604,982,596,073,392,129,658,151,798,236,443,367,032,362,870,111,177,095,113,385,883,263,629,590,425,927,788,139,913,723,354,312,780,360,880,815,898,754,637,407,928,177,138,518,084,893,218,890,877,227,837,150,333,355,290,832,014,626,564,82.77` (`~1.089e513`) | `837.91` | `0.030738s` |

## Evidence
- `benchmarks/all_runs/bench_runs/native_bigint_order_sweep_20260325/summary_native_bigint_order_sweep_20260325.json`
- `benchmarks/all_runs/bench_runs/native_bigint_order_sweep_20260325/native_max_tps_20260325_202953.log`
- `benchmarks/all_runs/bench_runs/native_bigint_order_sweep_20260325/native_max_tps_20260325_203238_e128.log`
- `benchmarks/all_runs/bench_runs/native_bigint_order_sweep_20260325/native_max_tps_20260325_203807_e512.log`

## Interpretation
- Counter math and signing path held at extreme integer magnitudes.
- Throughput scales with configured magnitude under this synthetic signed-range stress shape.
- This is not a node-safe networking profile and should be paired with a BPS-tuned profile (for example 10-BPS class) for deployment-facing baselines.
