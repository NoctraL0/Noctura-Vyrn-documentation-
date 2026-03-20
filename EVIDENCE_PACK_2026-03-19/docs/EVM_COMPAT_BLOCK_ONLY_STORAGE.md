# EVM Compat Block-Only Storage Mode

This mode keeps EVM compatibility execution, but avoids persisting EVM tx/receipt bodies to disk.

## Default Behavior in Stack Launcher

`run_stack.sh` now defaults to:

- `WRITE_TX_BODIES=1`
- `WRITE_EVM_TX_BODIES=0`
- `WRITE_EVM_RECEIPTS=0`

That means:

- block headers/roots still persist,
- EVM tx and receipt payloads are kept in memory only,
- website/live UX can still see recent activity from in-memory state,
- long-term EVM tx history lookups from disk are intentionally disabled.

## Explicit Launch Example

```bash
cd <REPO_ROOT>
WRITE_TX_BODIES=1 \
WRITE_EVM_TX_BODIES=0 \
WRITE_EVM_RECEIPTS=0 \
./run_stack_web_wallet.sh
```

## Optional Override (legacy behavior)

If you want full EVM tx/receipt persistence again:

```bash
WRITE_EVM_TX_BODIES=1 WRITE_EVM_RECEIPTS=1 ./run_stack_web_wallet.sh
```

## Runtime Check

Use policy endpoint:

```bash
curl -s -X POST http://127.0.0.1:8545 \
  -H 'content-type: application/json' \
  --data '{"jsonrpc":"2.0","id":1,"method":"policy","params":[]}'
```

Confirm runtime fields:

- `write_tx_bodies`
- `write_evm_tx_bodies`
- `write_evm_receipts`
