# Token Support Profile v1 (Baseline Lock)

## Goal
Define a strict, testable compatibility profile for token types while preserving native L1 performance and safety guardrails.

## Scope
- In scope:
  - ERC20 / BEP20 interface-compatible tokens on EVM compat lane.
  - CW20 tokens on CosmWasm compat lane.
- Out of scope (v1):
  - Rebasing tokens.
  - Reflection / fee-on-transfer accounting customizations.
  - Blacklist / pause semantics beyond standard ABI behavior.
  - Proxy upgrade trust assumptions (handled by deployment policy, not runtime emulation).

## ERC20 / BEP20 v1
- Required method parity:
  - `name`, `symbol`, `decimals`, `totalSupply`
  - `balanceOf`, `transfer`
  - `approve`, `allowance`, `transferFrom`
- Required event parity:
  - `Transfer(address,address,uint256)`
  - `Approval(address,address,uint256)`
- Required behavior parity:
  - Zero-value transfer support.
  - Self-transfer support.
  - Repeated `approve` overwrite semantics.
  - `transferFrom` allowance decrement semantics.
  - Revert on insufficient balance/allowance.
- Non-standard return handling:
  - Marked as compatibility extension (v1.1 hardening task), not guaranteed by v1 baseline.

## CW20 v1
- Required execute parity:
  - `transfer`
  - `increase_allowance`
  - `decrease_allowance`
  - `transfer_from`
- Required query parity:
  - `token_info`
  - `balance`
  - `allowance`
- Required behavior parity:
  - Allowance lifecycle correctness.
  - `transfer_from` allowance consumption.
  - Failure rollback guarantees.
  - Action/event log visibility for key token actions.

## Numeric Safety Policy
- ERC20/BEP20 external amount semantics are `uint256`.
- Any u64-only optimization path must fail-open into a full-width execution path, never truncate.
- Silent clipping/truncation is forbidden.

## Hashing Policy
- Token-critical derivations and selectors must use Keccak-256.
- Sha3-256 fallback is non-compliant for production token-critical paths.
- Runtime must surface a clear backend-missing error when compliant Keccak is unavailable.

## Baseline Evidence
- CW20 conformance suite:
  - `test_wasm_cw20_conformance_v1.py`
- ERC20/BEP20 edge suite:
  - `test_go_evm_erc20_bep20_edges_v1.py`
- Porting parity + BEP20 labeling:
  - `test_erc20_to_vyrn_v1.py`
