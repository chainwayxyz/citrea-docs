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

{% hint style="warning" %}
Citrea uses BTC as its native token. **There is no Citrea token**. Please beware of scams!
{% endhint %}

Citrea is the first rollup that enhances the capabilities of Bitcoin blockspace with zero-knowledge technology, making it possible to **build everything on Bitcoin**. This requires **no changes to the network's consensus rules**.

## Citrea is a [Type 2 zkEVM](technical-specs/characteristics/execution-environment.md)

Citrea processes a large number of zkEVM transaction batches and submits a succinct zero-knowledge proof that allows **easy verification** of the batch validity in an inscription-like envelope to Bitcoin. The zero-knowledge proofs generated by the prover output the state difference resulting from that batch, with these proofs then being published to Bitcoin regularly.

## Citrea proofs are inscribed in Bitcoin and optimistically verified via BitVM

Anyone running a Bitcoin node can both verify Citrea and access its full state. This method ensures that the data needed to transact on Citrea or verify Citrea becomes permissionlessly available for every single node in the Bitcoin network. The proof inscribed in Bitcoin by prover(s) is optimistically verified in Bitcoin as well, thanks to BitVM.

## Citrea supports Light Nodes

Taking advantage of a recursive zero-knowledge proof system - STARKs specifically - Citrea allows easy verification of the blockchain by light nodes. By recursively verifying batch proofs within light client proofs, the full Citrea dataset shrinks into a small zero-knowledge proof and lives inside Bitcoin. Anyone running a Bitcoin Light Node (SPV) can trustlessly verify Citrea and access its state root.

## Citrea has the first universal trust-minimized two-way peg

Citrea light client proofs are natively verified on Bitcoin via BitVM, enabling the first layer 2 verification in Bitcoin and trust-minimized two-way peg mechanism. Pegged BTC is held in the BitVM contract (leveraging Taproot) on the Bitcoin network. Withdrawals are only authorized with valid ZK proofs, meaning no party can steal the pegged BTC from the bridge.

Citrea uses $BTC as its native token. To avoid confusion and improve on/off-ramp UX; Citrea native $BTC is referred to as $cBTC in Citrea.
