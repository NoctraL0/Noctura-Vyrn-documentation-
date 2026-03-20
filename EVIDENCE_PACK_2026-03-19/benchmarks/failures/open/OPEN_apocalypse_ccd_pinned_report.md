# Apocalypse Fullstack Saturation Report (CCD Pinned)

Pinned run: `<REPO_ROOT>/bench_runs/apocalypse_fullstack_ccd_pinned_20260319_140325`
Baseline run: `<REPO_ROOT>/bench_runs/apocalypse_fullstack_20260319_135955`
Date: 2026-03-19
Duration: 60s concurrent load

## CCD Affinity
- Native DAG RPC pinned to: `0,12` (CCD 0)
- Main/L2/aux pinned to: `6-11,18-23` (CCD 1)

## Exit Status
- native lane: `0`
- compat lane: `0`
- bpf lane: `0`
- l2 lane: `0`

## Pinned Run Metrics
### Native
- tx/s: `85006.17`
- committed txs: `5104000`
- blocks/s: `21.25`
- p50/p90/p99: `0.049039s` / `0.062998s` / `0.083607s`

### Compat mix
- calls/s: `100.04111814922382`
- calls_ok/err: `6004` / `0`
- p50/p90/p99 ms: `8.470711996778846` / `335.5444170010742` / `350.7143240130972`

### BPF
- calls/s: `2215.2252700347453`
- calls_ok/err: `132926` / `0`
- p50/p90/p99 ms: `7.079642004100606` / `8.359914005268365` / `9.823714004596695`

### L2 (mixed)
- req/s: `1738.6642204401703`
- app_ok / app_reject: `36011` / `68323`
- top_errors: `{'channel defi at capacity': 68323}`
- p50/p90/p99 ms: `7.664317992748693` / `10.997876001056284` / `16.638087981846184`

## Delta vs Baseline (non-pinned)
- Native tx/s: `73660.83 -> 85006.17` (`15.40%`)
- Compat calls/s: `100.14975226842233 -> 100.04111814922382` (`-0.11%`)
- BPF calls/s: `1812.5256665073307 -> 2215.2252700347453` (`22.22%`)
- L2 req/s: `1575.5113530227843 -> 1738.6642204401703` (`10.36%`)

## Conclusion
- CCD pinning improved overall mixed saturation throughput in this run, especially native and BPF lanes.
- L2 still bottlenecks at channel capacity (`channel defi at capacity`) under this exact mixed stress shape.
