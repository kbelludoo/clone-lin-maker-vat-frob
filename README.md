# clone-lin-maker-vat-frob

Experimental **LIN** clone of MakerDAO / Sky `Vat.frob` — the Dai and USDS
CDP lock/draw/wipe/free safety kernel (signed `ink`/`art` updates, `tab = art*rate`,
`tab <= ink*spot` unless less-risky, debt ceiling, dust).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [makerdao/dss](https://github.com/makerdao/dss) `fa4f6630afb0624d04a003e920b0d71a00331d98`
  - `src/vat.sol` sha256 `9016fd94f0f1b014abf184edab86a3f5843042b17e3734407e0f0fabb920f49f` blob `ff90bbed5b36e54318bd5461cbf32e4167df2b28`
  - License AGPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_maker_vat_frob.lin`](src/lin_maker_vat_frob.lin) (sha256 `33c4a7e904b8b5a727b2a624203483cf6dfad3330e729a2b0a3481735b64eece`, identical to lin-open)
- **C11 oracle copy:** [`test/oracles/maker_vat_frob_c11.c`](test/oracles/maker_vat_frob_c11.c)
- **Proofs (lin-open only):** `examples/maker_vat_frob/` and `make maker-vat-frob-proof`
- **Class:** EXPERIMENTAL uint64-scale, `RAY' = 1e9`. Not mainnet `RAY=1e27`. Not wad×ray rad. Not a production vat replacement.

Named goldens: OPEN lock `1e9` + draw `5e8` at 200% spot → **frob_ok=1**; UNSAFE draw `3e9` → **0**; LESS-RISKY wipe underwater → **1**; 150% `max_art` **1500000000** vs naive 1:1 **1000000000** (buffer **5e8**); naive i64 `7e18*3e18` wraps to **-5389669316318724096**; 128-bit muldiv fail-closes to **0**.

## Reproduce proofs (in lin-open)

```bash
make maker-vat-frob-proof
# or: python3 test/prove_maker_vat_frob_external.py
```

## Not claimed

Uint256 / RAY=1e27 / wad×ray rad mainnet parity.
LIN replacing Maker, Sky, vat, or the EVM. TVL or dollar savings. zk soundness.
`wish` / `wards` / `gem` / `dai` mappings. `vat.fold` (see clone-lin-maker-vat-fold).
