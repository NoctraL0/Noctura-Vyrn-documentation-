# Failure Status Ledger

This ledger tracks engineering-relevant failures only.

Startup/misinput-only incidents are intentionally excluded from `failures/`.

## Fixed
1. `engine_probe_opt_20260319_164349` and `engine_probe_nodelay_20260319_164601`
- Issue: compat mix dropped to ~383 calls/s in optimization passes.
- Evidence (problem):
  - `fixed/FIXED_engine_probe_opt_mix.json`
  - `fixed/FIXED_engine_probe_nodelay_mix.json`
- Evidence (recovery):
  - `fixed/FIXED_engine_probe_wasm_lazy_mix.json`
- Status: `Fixed`

## Open
1. `full_saturation_dual_20260319_092040`
- Issue: mixed-lane saturation still shows compat latency blowout and L2 channel-capacity rejects under this stress shape.
- Evidence:
  - `open/OPEN_full_saturation_dual_report.md`
  - `open/OPEN_full_saturation_dual_summary.json`
- Status: `Open`

2. `apocalypse_fullstack*` mixed-lane family
- Issue: throughput tradeoffs remain under full-stack + durability + pinning + binary transport combinations.
- Evidence:
  - `open/OPEN_apocalypse_fullstack_report.md`
  - `open/OPEN_apocalypse_ccd_pinned_report.md`
  - `open/OPEN_apocalypse_go_l2_binary_report.md`
  - `open/OPEN_apocalypse_go_l2_binary_summary.json`
- Status: `Open`
