# Stress Results Ledger

Canonical record of proven stress results with evidence paths.

## Session: 2026-03-15 (America/Los_Angeles)

| Run ID | Approx Time | Mode | Storage | Signed Safety Flags | Workers | Target TPS | Measured TX/s | Proof Log |
|---|---|---|---|---|---:|---:|---:|---|
| `vyrn_hit_5p4e20_20260315_000346/signed_memory_w2_push` | 00:03 | `miner_mineRangeSigned` (`ed25519`) | `memory` (non-durable) | `NATIVE_SIG_VERIFY=1`, `NATIVE_SIG_REQUIRE=1`, validator quorum checks all `=1` | 2 | `550000000000000000000` | `549300094809466208256.00` | [/tmp/vyrn_hit_5p4e20_20260315_000346/signed_memory_w2_push/stress.log](/tmp/vyrn_hit_5p4e20_20260315_000346/signed_memory_w2_push/stress.log) |
| `vyrn_safe_durable_20260315_000630/safe_w2_550` | 00:06 | `miner_mineRangeSigned` (`ed25519`) | `log` + persisted blocks (durable) | `NATIVE_SIG_VERIFY=1`, `NATIVE_SIG_REQUIRE=1`, validator quorum checks all `=1` | 2 | `550000000000000000000` | `448299458311436435456.00` | [/tmp/vyrn_safe_durable_20260315_000630/safe_w2_550/stress.log](/tmp/vyrn_safe_durable_20260315_000630/safe_w2_550/stress.log) |

## Command Context

- Durable launch/run commands are tracked in:
  - [SAFE_DURABLE_STRESS_COMMANDS.md](<REPO_ROOT>/SAFE_DURABLE_STRESS_COMMANDS.md)

## Safety Evidence (selected)

- Non-durable hit run DAG env line:
  - [/tmp/vyrn_hit_5p4e20_20260315_000346/signed_memory_w2_push/dag.log](/tmp/vyrn_hit_5p4e20_20260315_000346/signed_memory_w2_push/dag.log)
- Durable best run DAG env line:
  - [/tmp/vyrn_safe_durable_20260315_000630/safe_w2_550/dag.log](/tmp/vyrn_safe_durable_20260315_000630/safe_w2_550/dag.log)

## Session: 2026-03-20 (America/Los_Angeles) - Strict Counter-Integrity Validation

| Run ID | Approx Time | Mode | Storage | Signed Safety Flags | Workers | Target TPS | Measured TX/s | Proof Log |
|---|---|---|---|---|---:|---:|---:|---|
| `strict_no_doublecount_20260320_031126` | 03:11 | `miner_mineRangeSigned` (`ed25519`) | `log` + persisted blocks | `NATIVE_SIG_VERIFY=1`, `NATIVE_SIG_REQUIRE=1`, validator quorum checks all `=1`, `nonce-sync=on`, `senders-per-worker=64`, `stop-on-reject=on` | 20 | `6500000000000000000000000000000` | `6425421127134743566077883580416.00` | [`../benchmarks/all_runs/bench_runs/strict_no_doublecount_20260320_031126.log`](../benchmarks/all_runs/bench_runs/strict_no_doublecount_20260320_031126.log) |
| `strict_no_doublecount_saturation_20260320_031157` | 03:11 | `miner_mineRangeSigned` (`ed25519`) | `log` + persisted blocks | `NATIVE_SIG_VERIFY=1`, `NATIVE_SIG_REQUIRE=1`, validator quorum checks all `=1`, `nonce-sync=on`, `senders-per-worker=64`, `stop-on-reject=on` | 20 | `65000000000000000000000000000000000000000` | `428869442090321794019470256111616.00` | [`../benchmarks/all_runs/bench_runs/strict_no_doublecount_saturation_20260320_031157.log`](../benchmarks/all_runs/bench_runs/strict_no_doublecount_saturation_20260320_031157.log) |

## Row Template

Use this row format for future additions:

| `run_id/case` | HH:MM | `miner_mineRangeSigned` (`ed25519`) | `log`/`memory` | safety flags summary | N | target | measured | `/tmp/.../stress.log` |
