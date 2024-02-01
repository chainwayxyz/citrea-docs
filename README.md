---
cover: .gitbook/assets/proper_bg.png
coverY: 200.6996282527881
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# TL;DR

> Citrea uses BTC as native token. There is no Citrea token or such. Please beware of scams!

Citrea is the first rollup that enhances the capabilities of Bitcoin blockspace with zero-knowledge technology, making it possible to build everything on Bitcoin. This requires no changes to the network's consensus rules.&#x20;

**Citrea is a Type 2 zkEVM.** Citrea process large number of zkEVM transaction batches and submits a succinct zero-knowledge proof that allows easy verification of the batch validity in an inscription-like envelopes to Bitcoin. The zero-knowledge proofs generated by the prover outputs the state difference that batch resulted.&#x20;

**Citrea proofs are inscribed in Bitcoin and verified in Bitcoin via BitVM.** Anyone running a Bitcoin node can both verify Citrea and access to its full state. This method ensures that the data needed to transact on Citrea or verify Citrea becomes permissionlessly available for every single node in the Bitcoin network. The proof inscribed in Bitcoin is optimistically verified in Bitcoin as well, thanks to BitVM.

**Citrea supports Light Nodes**. Taking advantage of a recursive zero-knowledge proof system, STARKs spesifically, Citrea allows easy verification of the blockchain with light nodes. By recursively verifying batch proofs in light client proofs, the full Citrea blockchain shrinks into a small zero-knowledge proof and lives inside Bitcoin. Anyone running a Bitcoin Light Node (SPV) can trustlessly verify Citrea and access to its state root.&#x20;

**Citrea has the first universal trust-minimized two way peg.** Citrea light client proofs are natively verified on Bitcoin via BitVM, enabling the first Layer 2 verification in Bitcoin and trust-minimized two way peg mechanism. Pegged BTCs held in BitVM contract (taproot) on Bitcoin network. Withdrawals are only authorized with valid ZK proofs, meaning no party can steal the pegged BTC from the bridge.

Citrea uses $BTC as native token. To avoid confusion and improve on/off-ramp UX; Citrea native $BTC is named as $cBTC in Citrea.

