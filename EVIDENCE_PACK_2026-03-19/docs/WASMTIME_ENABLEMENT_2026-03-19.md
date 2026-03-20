# Wasmtime Enablement - 2026-03-19

## Problem
- Direct runs with `/usr/bin/python3` did not have `wasmtime`, which caused:
  - `wasm_runtime.backend_available() == False`
  - WASM suites to skip with `wasmtime backend unavailable`
  - runtime fallback paths instead of real WASM execution

## Fix Applied
- Updated `wasm_runtime.py` to auto-resolve `wasmtime` from project `.venv` site-packages when the active interpreter does not have it installed.
- Updated `requirements.txt` to include:
  - `wasmtime>=36.0.0`

## Verification
- Plain `python3` check:
  - `backend_available True`
- Post-fix WASM suite run:
  - `12/12` passing
  - `0` skips
- Artifact:
  - `bench_runs/compat_baseline_20260319_142732_wasmtime_fix/wasm_post_fix.log`
