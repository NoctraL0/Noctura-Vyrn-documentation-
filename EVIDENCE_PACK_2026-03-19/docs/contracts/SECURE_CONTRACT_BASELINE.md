# Secure Contract Baseline

Use this baseline for all Solidity contracts in this repo to avoid repeating
audit findings like missing access control, weak lifecycle checks, and input
gas grief vectors.

## Required Rules

1. Every `external`/`public` mutating function must have an explicit trust model.
   - Use a modifier like `onlyOperator`, `onlyAuthorized`, or `onlyRole`.
   - If intentionally public, document why in a comment above the function.

2. Use explicit state machines for one-way lifecycles.
   - Prefer `mapping(bytes32 => uint8)` state over multiple bool mappings.
   - Validate transitions (`unknown -> recorded -> finalized`) before writes.

3. Enforce idempotency and replay protections.
   - Reject duplicate IDs before emitting side-effect events.
   - Reject finalization on unknown IDs.

4. Bound dynamic inputs.
   - Add max length checks for `string`/`bytes` calldata.
   - Favor fixed-size identifiers (`bytes32`, `address`, enums) where possible.

5. Keep authority rotation explicit and auditable.
   - Include `transferOperator` or role admin flow.
   - Emit events on privilege changes.

6. Keep failure reasons deterministic.
   - Use custom errors for hot paths.
   - Avoid silent fallback logic in write paths.

7. Add security tests for each mutating API.
   - Unauthorized caller reverts.
   - Duplicate operation reverts.
   - Invalid lifecycle transition reverts.
   - Oversized input reverts.

8. Treat stubs as production-risk unless fenced.
   - If a contract is demo-only, mark it clearly and restrict deployment scope.

## Deployment Gate

Before deploy:

1. Run guardrails: `python3 scripts/sol_guardrails_check.py`
2. Compile with your production settings (for this repo, `solc 0.8.19` and
   `viaIR=true` for string-heavy contracts).
3. Run security tests for the changed contract.
4. Save artifact evidence (tx hash, contract address, compile settings, and
   runtime mode).

