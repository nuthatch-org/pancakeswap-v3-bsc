# pancakeswap-v3-bsc

A [nuthatch](https://github.com/nightswatchhq/nuthatch) nest: **PancakeSwap V3 on BNB Smart Chain**.

Concentrated-liquidity pools discovered from the factory, and their swaps, mints and burns.

One binary, one config file, no graph-node, no gateway, no query fees.

## What it indexes

**Chain:** `bsc`. **1 contract**, **12 tables**.

| alias | address |
|---|---|
| `factory` | `0x0bfbcf9fa4f9c56b0f40a671ad40e0805a091865` |

## Verified

Indexed blocks **117,399,468 to 117,449,466** and sealed **23,525 events**. Every table below is generated from the vendored ABIs, and the run above is what this nest actually decoded, not an estimate.

## Read this before trusting it

- **Not a config change from `uniswap-v3`.** PancakeSwap's pool `Swap` carries two extra `uint128` protocol-fee parameters, so a different topic0. Reusing the Uniswap pool ABI seals **zero swaps while reporting a healthy run**.
- The pool ABI is vendored from a PancakeSwap V3 pool on **Ethereum** - the same factory address deploys the same bytecode there - because BSC has neither Sourcify coverage for it nor a Blockscout instance.
- BSC's keyless public endpoint refuses archive requests *and* address-less `getLogs`. The latter is what the topic0 filter flip issues, so past 500 children this nest needs your own endpoint (nuthatch#761).

## Run it

```sh
nuthatch init --from https://github.com/nightswatchhq/pancakeswap-v3-bsc
cd pancakeswap-v3-bsc
nuthatch dev --dir . --backfill 50000 --seal-direct
nuthatch sql --dir . "SELECT count(*) FROM \"factory__fee_amount_enabled\""
```

The endpoint in `nuthatch.toml` is keyless and public, so this file is publishable: a `nuthatch.toml` is pinned into the nest's content address and must never carry a credential. It is enough to follow the tip. A **backfill** wants archive depth it may not have: pass your own with `--rpc`, and check it first with `nuthatch doctor --rpc <url>`.

## Tables

```
factory__fee_amount_enabled
factory__fee_amount_extra_info_updated
factory__owner_changed
factory__pool_created
factory__set_lm_pool_deployer
factory__white_list_added
pool__burn
pool__collect
pool__flash
pool__initialize
pool__mint
pool__swap
```
