# CCD Pinning Launch

Use this launcher to isolate native (DAG) onto one core in one CCD and pin all other services to the other CCD:

```bash
cd <REPO_ROOT>
./run_stack_ccd_pinned.sh
```

## Default behavior on this machine (Ryzen 9 7900X)

Detected L3/CCD groups:
- CCD A: `0-5,12-17`
- CCD B: `6-11,18-23`

Default pin plan:
- `dag_rpc` (native): one physical core on CCD A -> `0,12`
- `main_rpc`, `l2_rpc`, `dag_guard`, `dag_metrics`, relayer/gateway (if enabled): all cores on CCD B -> `6-11,18-23`

## Overrides

- Choose which CCD hosts native:
```bash
NATIVE_CCD_INDEX=1 ./run_stack_ccd_pinned.sh
```

- Force native to a custom cpuset:
```bash
NATIVE_CPUSET=6,18 ./run_stack_ccd_pinned.sh
```

- Force non-native services to custom cpuset:
```bash
OTHER_CPUSET=0-5,12-17 ./run_stack_ccd_pinned.sh
```

- Typical local-web launch with pinning:
```bash
STACK_PROFILE=web START_GATEWAY=1 START_L2=1 ./run_stack_ccd_pinned.sh
```

## Stop stack

```bash
DATA_DIR=chain_data ./stop_stack.sh
```
