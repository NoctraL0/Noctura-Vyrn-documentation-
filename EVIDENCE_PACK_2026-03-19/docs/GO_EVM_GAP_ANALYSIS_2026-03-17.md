# Go EVM Gap Analysis (vs Python EVM)

Date: 2026-03-17

## Current state

- Go EVM parity scripts pass for covered cases.
- Go EVM fastpath tests pass for covered cases.
- Runtime dispatch is now Go-first with safe Python fallback (`VYRN_EVM_ENGINE=auto` default).

## Status Update (After Go Fastpath + Runtime Cutover Patch)

### 1) Full gas accounting semantics

Status: Implemented in Go runner.

Python enforces detailed gas logic (including memory expansion, warm/cold access costs, and call gas behavior).

Examples:
- `evm_layer.py:480`
- `evm_layer.py:553`
- `evm_layer.py:1047`

Go runner now mirrors Python-style gas behavior:
- base opcode charging
- dynamic charging (memory growth, copy words, SHA3 words, EXP bytes, LOG topic/data)
- SLOAD/SSTORE warm/cold + SSTORE extra charges
- call gas forwarding (63/64 rule), stipend, precompile metering/refunds, child gas refund
- OOG error signaling (`EVM_OUT_OF_GAS`)
- response gas telemetry (`gas_left`, `gas_used`)

### 2) Access-list + warm-set behavior

Status: Implemented in Go runner.

Python supports `access_list` warming and propagates warm sets through subcalls.

Examples:
- `evm_layer.py:601`
- `evm_layer.py:1172`
- `evm_layer.py:1338`

Go runner now supports:
- request `access_list` ingestion
- caller/callee/precompile pre-warming
- warm address + warm slot tracking
- warm set propagation through subcalls

### 3) Precompile correctness parity

Status: Implemented in Go runner (with strict-mode support).

Python has richer precompile behavior plus strict mode support.

Examples:
- `evm_layer.py:290`
- `evm_layer.py:350`

Go precompile path now includes:
- `0x01` ecrecover handling
- `0x02` SHA-256 (corrected from Keccak)
- `0x03` RIPEMD-160
- `0x04` identity
- `0x05` modexp
- `0x06/0x07/0x08` bn256 add/scalar-mul/pairing
- `0x09` BLAKE2f (EIP-152 style validation/execution)
- `EVM_PRECOMPILE_STRICT` behavior for stricter failure handling

### 4) CREATE address derivation robustness

Status: Implemented in Go runner.

Python attempts canonical CREATE address derivation using RLP path (with fallback).

Examples:
- `evm_layer.py:1153`

Go now derives CREATE addresses via canonical RLP path:
- `keccak256(rlp([sender_20_bytes, nonce]))[-20:]`
- defensive legacy fallback retained only for unexpected internal failures
- parity coverage added for sender/nonce edge cases

### 5) Runtime wiring still Python

Status: Implemented with safe rollout defaults.

Runtime now supports:
- `VYRN_EVM_ENGINE=auto` (default): prefer Go, fallback to Python on unavailable/unsupported paths
- `VYRN_EVM_ENGINE=go`: Go-only, fail closed if backend is unavailable
- `VYRN_EVM_ENGINE=python`: force Python path

Examples:
- `contract_runtime_stub.py` (Go/Python/auto routing + engine/fallback metadata)
- `nexus_runtime_v1.py` (Go/Python/auto routing in create + call paths)

## Practical interpretation

- Opcode/function parity for tested paths is already strong.
- Items 1-5 are now implemented.
- Go fastpath is integrated into runtime execution with safe default fallback behavior.

## Validation scripts

- Existing:
  - `python3 compare_go_python_evm_v1.py`
  - `python3 compare_go_python_evm_world_v1.py`
  - `python3 test_go_evm_fastpath_v1.py`
- Added for this patch:
  - `python3 test_go_evm_gas_access_precompile_v1.py`
  - `python3 test_go_evm_create_derivation_v1.py`
  - `python3 test_evm_go_runtime_routing_v1.py`
  - `python3 test_nexus_evm_engine_routing_v1.py`
