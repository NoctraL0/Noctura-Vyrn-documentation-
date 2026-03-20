# Inferno Hotspots (All-Lane Saturation, 60s)

Run directory:
- <REPO_ROOT>/bench_runs/limit_hunt_5p45e20_20260319_015446

Final lane results (all statuses 0):
- Native: 3,452,000 committed in 60.064s (57,471.94 tx/s)
- Compat mix: 3,058 calls ok in 60.046s (50.93 calls/s)
- BPF: 53,888 calls ok in 60.006s (898.05 calls/s)
- L2 mixed: 49,201 req total, 12,404 app_ok, 36,797 app_reject, 783.08 req/s

## DAG RPC inferno
Profile file:
- profiles/dag_inferno.svg

Top stacks (by sampled share):
- _do (dag_rpc_server.py:4213)
- mine_with_txs (multi_dag_runtime.py:1703, 1731, 1754, 1806)
- execute_child_block (state_exec_vm_v1.py:1282)
- _record_committed (multi_dag_runtime.py:964)
- write/packb in dag write path (dag_rpc_server.py:1155, 1165 + msgpack.packb)

Interpretation:
- Native throttle is dominated by mining + state execution + serialization/write path under full mixed contention.

## Main RPC inferno
Profile file:
- profiles/main_rpc_inferno.svg

Top stacks:
- _contract_call_common -> _contract_chain_ctx -> _load_head (rpc_server_v1.py)
- genericpath.exists checks in contract chain context path
- _execute_with_wasmtime (wasm_runtime.py)
- HTTP socket read/write + response framing (http/server.py, socketserver.py)

Interpretation:
- Compat lane bottleneck is mostly contract-call control path + context loading overhead, then runtime execution and HTTP response overhead.

## L2 RPC inferno
Profile file:
- profiles/l2_rpc_inferno.svg

Top stacks:
- dataclasses._asdict_inner / genexpr conversion overhead
- do_POST (l2_rpc_server.py)
- inbox_add/outbox_add path (l2_core_v1.py)
- l2_batcher: _check_channel, add_inbox/add_outbox, build_batch, _item_root

Interpretation:
- L2 throttle is channel capacity + request shaping path, with notable Python dataclass conversion overhead in request/response processing.

