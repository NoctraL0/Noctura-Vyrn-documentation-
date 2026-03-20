# Apocalypse Fullstack Saturation Report

Run directory: `<REPO_ROOT>/bench_runs/apocalypse_fullstack_20260319_135955`
Date: 2026-03-19
Duration: 60s concurrent load (native + compat mix + BPF + L2)

## Guardrails Profile
- WAL enabled
- fsync enabled (`WAL_FSYNC_EVERY=1`, `DAG_LOG_FSYNC_EVERY=1`, `DAG_BLOCK_FSYNC=1`)
- snapshot/checkpoint enabled (`SNAPSHOT_EVERY=200`, `WAL_CHECKPOINT_EVERY=200`, `DAG_CHECKPOINT_EVERY=200`)
- strict stack profile (`STACK_PROFILE=strict`)

## Exit Status
- native lane: `0`
- compat lane: `0`
- bpf lane: `0`
- l2 lane: `0`

## Results
### Native (Go stress, range-signed)
- tx/s: `73660.83`
- committed txs: `4424000`
- blocks/s: `18.42`
- p50: `0.054170s`
- p90: `0.074297s`
- p99: `0.096422s`

### Compat Mix (EVM+WASM path)
- calls/s: `100.14975226842233`
- calls_ok: `6011`
- calls_err: `0`
- p50 ms: `10.902661975705996`
- p90 ms: `336.17908402811736`
- p99 ms: `358.7428939936217`

### BPF Lane
- calls/s: `1812.5256665073307`
- calls_ok: `108765`
- calls_err: `0`
- p50 ms: `8.681268023792654`
- p90 ms: `10.369416006142274`
- p99 ms: `12.566249002702534`

### L2 Lane (mixed)
- req/s (transport): `1575.5113530227843`
- requests_total: `94542`
- app_ok: `33683`
- app_reject: `60859`
- top_errors: `{'channel defi at capacity': 60859}`
- p50 ms: `8.51433799834922`
- p90 ms: `11.849814996821806`
- p99 ms: `17.38309601205401`

## Interpretation
- The stack stayed up under concurrent 60s saturation; all lane processes exited cleanly.
- The primary visible bottleneck in this profile was L2 app-level rejection from channel capacity (`channel defi at capacity`).
- Native throughput remained stable under mixed pressure with durability guardrails on.
