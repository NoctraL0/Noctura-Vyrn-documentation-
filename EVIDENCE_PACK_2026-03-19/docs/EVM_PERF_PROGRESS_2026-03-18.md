# EVM Perf Progress (2026-03-18)

## Goal
Push Go EVM compat execution faster while keeping native chain behavior untouched.

## What changed

- Added native quick call path (reduced Python marshalling overhead):
  - `go_evm_fastpath.py`: `run_state_native_quick`
- Added strict slot0 micro-fastpath eligibility and runtime routing:
  - `contract_runtime_stub.py`: `_go_slot0_eligible_bytecode`, `_go_slot0_eligible`
  - Uses `evm_fastpath = go_slot0` when eligible
- Added tuple-based slot0 hotpath (avoids dict churn):
  - `go_evm_fastpath.py`: `run_slot0_quick`
- Optimized Go shared library execution path:
  - `go_evm_fastpath/evmfast.go`
  - removed unnecessary calldata/storage copies in runner path
  - lazy jumpdest map build
  - used zero-copy `unsafe.Slice` for C input bytes
  - optimized `evmfp_run_slot0` direct path
- Rebuilt shared library:
  - `go_evm_fastpath/libevmfast.so`

## Validation

- `python -m py_compile` passed (`contract_runtime_stub.py`, `go_evm_fastpath.py`)
- `test_go_evm_fastpath_v1.py` passed

## Perf results

### cProfile hotpath script
Command:

```bash
PYTHONPATH=<REPO_ROOT> .venv/bin/python profile_evm_hotpath_v1.py
```

Before (same script, earlier run):
- `noop_direct`: `14.068 us`
- `incr_direct`: `13.708 us`
- `incr_mutable`: `94.869 us`

After latest changes:
- `noop_direct`: `6.425 us`
- `incr_direct`: `5.779 us`
- `incr_mutable`: `94.996 us`

### Throughput benchmark
Command:

```bash
PYTHONPATH=<REPO_ROOT> .venv/bin/python bench_go_evm_fastpath_v1.py
```

Latest key lines:
- `python_noop_direct`: `373,986 ops/s`
- `python_incr_direct`: `385,562 ops/s`
- `python_owner_guard`: `175,456 ops/s`
- `python_state_machine`: `183,427 ops/s`
- `python_callvalue_store`: `184,743 ops/s`

## Next step to approach ns-class latency

To get close to native nanosecond-class behavior, EVM execution needs to run fully in native process/hot loop without per-call Python boundary (or with batched execution per crossing). Current per-call Python orchestration is still microsecond-scale.
