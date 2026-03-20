# Compat 90 Baseline (CosmWasm + BPF) - 2026-03-19

## Scope
- Build both compatibility lanes to a "90% done" baseline before heavy optimization.
- Keep native lane behavior untouched while doing compat completion work.

## Native Guardrail
- Do not modify native hotpath files for this workstream:
  - `dag_rpc_server.py`
  - `go_dag/*`
  - native DAG launch profiles
- Compat work is limited to:
  - `wasm_runtime.py`
  - `bpf_runtime.py`
  - `bpf_layer.py`
  - `contract_runtime_stub.py`
  - `rpc_server_v1.py` (compat surfaces only)
  - compat tests

## Baseline Evidence (Captured Today)
- Full saturation, CCD pinned, Go L2:
  - `bench_runs/apocalypse_fullstack_ccd_pinned_go_l2_20260319_141306/APOCALYPSE_FULLSTACK_CCD_PINNED_GO_L2_REPORT.md`
- Compat baseline tests:
  - `bench_runs/compat_baseline_20260319_142109/compat_test_baseline.log`
  - `bench_runs/compat_baseline_20260319_142109/extra_compat_tests.log`
- Wasmtime enablement verification (plain `python3`, no venv activation required):
  - `bench_runs/compat_baseline_20260319_142732_wasmtime_fix/wasm_post_fix.log`

### Baseline Test Snapshot
- WASM suites (initial run): `12/12` pass, with `6` runtime-dependent skips (`wasmtime backend unavailable`).
- WASM suites (post-fix run): `12/12` pass, `0` skips.
- BPF suites: `6/6` pass.
- Extra compat suites (IBC/Cosmos/SPL): `5/5` pass.

## CosmWasm Matrix (Current -> 90%)

| Surface | Current Evidence | Current State | 90% Gate |
|---|---|---|---|
| Entrypoint + fallback dispatch (`instantiate/execute/query/reply/sudo/migrate`) | `test_wasm_dispatch_cw_v1.py`, `test_wasm_cw_admin_paths_v1.py` | Implemented | Must remain passing |
| Mutable rollback + submsg/reply guardrails | `test_wasm_submsg_reply_guardrails_v1.py`, `test_wasm_cw_submsg_bank_v1.py` | Implemented | Must remain passing |
| Admin/query RPC parity (`querySmart/queryRaw/info/updateAdmin/clearAdmin`) | `test_wasm_query_modes_v1.py`, `test_wasm_cw_admin_lifecycle_v1.py`, `test_wasm_policy_surface_v1.py` | Implemented | Must remain passing |
| Host imports + real fixture (cw20, env/storage/bank ABI) | `test_wasm_compat_v1.py`, `test_wasm_host_env_v1.py`, `test_wasm_cw20_base_integration_v1.py`, `test_wasm_cw_bank_payload_v1.py`, `test_wasm_cw_submsg_instantiate_v1.py` | Present but currently skipped without wasmtime | Zero skips in WASM suite on target environment |
| IBC/governance verifier-lite surface | `test_ibc_semantics_v1.py`, `test_ibc_rpc_surface_v1.py` | Implemented at verifier-lite level | Must remain passing |
| `query_chain` behavior | `wasm_runtime.py` currently returns `"query_chain unsupported"` when adapter missing | Partial | Add adapter-backed coverage test and fail-closed behavior test |
| Gas/fuel determinism | Fuel wiring exists in `wasm_runtime.py` | Partial test coverage | Add explicit out-of-gas regression test |
| Policy milestone marker | `rpc_wasm_policy` currently reports `target_today_pct: 80` | Not at 90 yet | Raise to `90` only after gates above are green |

### CosmWasm 90% Tasks
1. Unblock runtime coverage by ensuring `wasmtime` is available in the dev/test environment used for gates.
2. Add deterministic tests for:
   - `query_chain` adapter success/failure paths.
   - out-of-gas/fuel behavior under bounded gas.
3. Keep all existing WASM/IBC suites green.
4. Update `rpc_wasm_policy.cosmwasm_compat.target_today_pct` from `80` to `90` after completion.

## BPF Matrix (Current -> 90%)

| Surface | Current Evidence | Current State | 90% Gate |
|---|---|---|---|
| xBPF core ops + ABI args + control flow | `test_bpf_feature_matrix_v1.py`, `test_bpf_xbpf_control_flow_v1.py` | Implemented (`15` feature keys, `16` examples) | Must remain passing |
| Stateful app behavior (counter/transfer/owner/state machine/guard abort) | `test_bpf_app_examples_v1.py` | Implemented | Must remain passing |
| RPC deploy/call/storage/logging | `test_bpf_contract_runtime_v1.py`, `test_bpf_contract_rpc_v1.py`, `test_bpf_contract_logs_v1.py` | Implemented | Must remain passing |
| SPL token compatibility path | `test_spl_token_atomic_v1.py`, `test_spl_token_strict_meta_v1.py` | Implemented via compat module | Keep passing and wire through BPF path tests |
| Raw eBPF via `ubpf` | Optional path in `bpf_runtime.py` | Partial (no dedicated gate here) | Add explicit ubpf-path smoke/failure semantics tests |
| Solana parity breadth | High-level status says non-Solana-complete | Partial | Define and lock explicit supported subset for 90% |
| BPF policy observability | No dedicated `rpc_bpf_policy` surface today | Missing | Add read-only policy/status RPC for backend + limits |

### BPF 90% Tasks
1. Add tests for raw eBPF/`ubpf` execution path and expected error behavior when unavailable.
2. Add RPC-level gas/step-limit regression test for BPF call paths.
3. Add BPF policy/status RPC (backend, limits, supported feature set) for operability.
4. Keep all current BPF and SPL suites green.

## Token Conformance Track (Required for "Done")

Policy lock artifacts:
- `TOKEN_SUPPORT_PROFILE_V1.md`
- `config/token_support_profile_v1.json`

### CW20 (CosmWasm lane)

| Surface | Current Evidence | 90% Gate |
|---|---|---|
| Real fixture lifecycle (deploy/instantiate/query/transfer) | `test_wasm_cw20_base_integration_v1.py` | Must remain passing under plain `python3` |
| Query parity (`token_info`, `balance`, `allowance`) | `test_wasm_cw20_base_integration_v1.py`, `test_wasm_cw20_conformance_v1.py` | Keep green; expand optional minter/marketing queries later |
| Execute parity (`transfer`, `send`, `increase/decrease_allowance`, `transfer_from`) | `test_wasm_cw20_conformance_v1.py` | Keep execute matrix green with rollback checks |
| Event/log correctness | `test_wasm_submsg_reply_guardrails_v1.py`, `test_wasm_cw_submsg_bank_v1.py`, `test_wasm_cw20_conformance_v1.py` | Keep CW20 action/event assertions green |
| Funds + submessage behavior | `test_wasm_cw_bank_payload_v1.py`, `test_wasm_cw_submsg_instantiate_v1.py`, `test_wasm_cw20_conformance_v1.py` | Keep passing, including negative payload validation |

### BEP20 / ERC20 (EVM lane)

Note: BEP20 is ERC20-compatible at interface level. Target is strict ERC20 behavior plus BSC/BEP20 practical compatibility.

| Surface | Current Evidence | 90% Gate |
|---|---|---|
| Core ERC20 methods (`totalSupply`, `balanceOf`, `transfer`, `approve`, `allowance`, `transferFrom`) | `test_go_evm_fastpath_v1.py` | Keep full parity passing (Go + Python path) |
| Metadata (`name`, `symbol`, `decimals`) | `test_go_evm_fastpath_v1.py` | Keep passing with constructor-seeded metadata cases |
| Event topics/data correctness (`Transfer`, `Approval`) | `test_go_evm_fastpath_v1.py`, `test_go_evm_erc20_bep20_edges_v1.py` | Keep edge-case event tests green (zero-value transfer, self-transfer, max allowance transitions) |
| Full-width (`uint256`) routing safety | `test_go_evm_u256_routing_v1.py` | Keep wide-value fallback path green (no u64 truncation) |
| Non-standard token behavior tolerance (tokens returning no bool) | Mentioned risk in `HashLockAI.md` | Add compatibility tests/wrappers for no-return and false-return variants |
| Bridge/porting parity | `test_erc20_to_vyrn_v1.py` | Keep passing, including BEP20 labeling/network metadata checks |

### Token-Specific Tasks
1. Done: Added CW20 execute/query matrix + rollback/event checks in `test_wasm_cw20_conformance_v1.py`.
2. Done (except non-standard return variants): Added ERC20/BEP20 edge-case suite in `test_go_evm_erc20_bep20_edges_v1.py`.
3. Done for current fastpaths: Added token event conformance checks in both lanes.
4. Done: Added full-width routing regression in `test_go_evm_u256_routing_v1.py`.
5. Pending: Re-run saturation after token conformance work to measure churn reduction.

## 90% Completion Definition

### CosmWasm 90% is complete when:
- `test_wasm_*.py` is green with `0` skips on the target environment.
- `test_ibc_semantics_v1.py` and `test_ibc_rpc_surface_v1.py` remain green.
- New query-chain + gas-limit regression tests are green.
- `rpc_wasm_policy` target marker is `90`.
- CW20 conformance matrix tests are green.

### BPF 90% is complete when:
- `test_bpf_*.py` remains fully green.
- SPL compat tests remain green.
- New ubpf-path + RPC gas-limit + policy-surface tests are green.
- Supported BPF subset is explicitly documented and enforced.

### EVM Token Compatibility Gate (BEP20/ERC20)
- ERC20/BEP20 conformance + edge-case tests are green.
- Non-standard return handling tests are green.
- Bridge/port parity tests remain green.

## Automation Agent Execution Template
1. Run baseline suites and record artifacts.
2. Implement only compat-lane tasks (respect native guardrail).
3. Re-run full compat suite.
4. Run saturation gate:
   - CCD pinned
   - Go L2 enabled
   - WAL/fsync/snapshot guardrails on
5. Compare against:
   - `bench_runs/apocalypse_fullstack_20260319_135955`
   - `bench_runs/apocalypse_fullstack_ccd_pinned_20260319_140325`
   - `bench_runs/apocalypse_fullstack_ccd_pinned_go_l2_20260319_141306`
