# Vyrn Safe Stress Share Pack

- Generated: `2026-03-17T21:02:03`
- Lane: `native signed range`, safe durable profile (`log` store + persisted blocks)
- Safety flags: `NATIVE_SIG_VERIFY=1`, `NATIVE_SIG_REQUIRE=1`, `DAG_ENFORCE_VALIDATOR_QUORUM=1`, `DAG_ENFORCE_PARENT_QUORUM=1`, `DAG_ENFORCE_CHILD_QUORUM=1`

## Peak Result (Best In Sweep)
- Target TPS: `700000000000000000000`
- Measured TPS: `655543655722502193152.00`
- Blocks/s: `655.54`
- TX committed: `6557000000000000000000`
- p99 latency: `0.004256s`
- Source: `<REPO_ROOT>/bench_runs/safe_peak_sweep_20260317_185657/sweep.tsv`

## Confirmation Run (Same Safe Profile)
- elapsed_sec: `10.001923`
- txs_committed: `6471000000000000000000`
- blocks_mined: `6471`
- tx_per_second: `646975584783514468352.00`
- blocks_per_second: `646.98`
- avg_txs_per_block: `1000000000000000000.00`
- p50: `0.002820s`
- p90: `0.003701s`
- p99: `0.004891s`
- Source: `<REPO_ROOT>/bench_runs/safe_peak_final_20260317_190159/stress.log`

## Durability Evidence
- Blocks directory: `<REPO_ROOT>/bench_runs/safe_peak_final_20260317_190159/data/dag_blocks`
- Block files: `6471`

## One Solved Block (Sanitized)
- Sanitized sample JSON: `<REPO_ROOT>/bench_runs/safe_peak_final_20260317_190159/sample_block_sanitized.json`
- Raw source block file: `<REPO_ROOT>/bench_runs/safe_peak_final_20260317_190159/data/dag_blocks/00000000-p00000001.mpk`
- Redacted fields: sender, pubkey, signature, digest, finality proof, transfer proof

## Share Safety Notes
- Safe to share privately: this markdown + sanitized JSON + stress summary logs.
- Avoid sharing raw env dumps from DAG logs publicly (can expose local infra details).
- Raw block files are generally okay for private review, but they include sender/pubkey/signature metadata.
