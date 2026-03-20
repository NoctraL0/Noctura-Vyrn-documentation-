# Apocalypse Fullstack Saturation (CCD Pinned + Go L2 + Binary RPC Pass)

Run: `<REPO_ROOT>/bench_runs/apocalypse_fullstack_ccd_pinned_go_l2_binary_20260319_153311`
Date: 2026-03-19

## Run Config
- CCD pinning enabled (`dag_rpc` on `0,12`; rest on `6-11,18-23`)
- `L2_ENGINE=go`
- Guardrails: WAL/fsync/snapshot/checkpoints ON
- Duration: 60s
- Transport in this pass:
  - Native lane: `msgpack+http`
  - Compat mix lane: `msgpack+http`
  - BPF lane: `http/json` (unchanged harness)
  - L2 lane: `http/json` (unchanged harness)

## Results
### Native lane
- tx/s: `76798.24`
- committed txs: `4612000`
- blocks/s: `19.22`
- p50/p90/p99: `0.047102s` / `0.072658s` / `0.082251s`

### Compat mix lane (EVM+WASM)
- calls/s: `93.04220798531678`
- calls_ok/err: `5584` / `0`
- p50/p90/p99 ms: `10.06463702651672` / `362.3898930090945` / `384.55635801074095`

### BPF lane
- calls/s: `1942.5399202648193`
- calls_ok/err: `116565` / `0`
- p50/p90/p99 ms: `8.110156981274486` / `9.610578999854624` / `11.472520011011511`

### L2 lane (mixed)
- req/s: `2134.667324347796`
- app_ok/app_reject: `128090` / `0`
- p50/p90/p99 ms: `7.243161991937086` / `10.601143992971629` / `14.154125994537026`

## Comparison vs Prior CCD-Pinned Go L2 Baseline
Baseline reference: `bench_runs/apocalypse_fullstack_ccd_pinned_go_l2_20260319_141306/APOCALYPSE_FULLSTACK_CCD_PINNED_GO_L2_REPORT.md`

- Native tx/s: `78476.64 -> 76798.24` (`-2.14%`)
- Compat calls/s: `109.54093082282314 -> 93.04220798531678` (`-15.06%`)
- BPF calls/s: `2247.2852570004543 -> 1942.5399202648193` (`-13.56%`)
- L2 req/s: `1920.9263786755441 -> 2134.667324347796` (`11.13%`)

## Artifacts
- Status: `<REPO_ROOT>/bench_runs/apocalypse_fullstack_ccd_pinned_go_l2_binary_20260319_153311/status.env`
- Native log: `<REPO_ROOT>/bench_runs/apocalypse_fullstack_ccd_pinned_go_l2_binary_20260319_153311/logs/native_stress.log`
- Compat JSON: `<REPO_ROOT>/bench_runs/apocalypse_fullstack_ccd_pinned_go_l2_binary_20260319_153311/logs/compat_mix.json`
- BPF JSON: `<REPO_ROOT>/bench_runs/apocalypse_fullstack_ccd_pinned_go_l2_binary_20260319_153311/logs/bpf_lane.json`
- L2 JSON: `<REPO_ROOT>/bench_runs/apocalypse_fullstack_ccd_pinned_go_l2_binary_20260319_153311/logs/l2_stress.json`
- Summary JSON: `<REPO_ROOT>/bench_runs/apocalypse_fullstack_ccd_pinned_go_l2_binary_20260319_153311/summary_binary.json`
