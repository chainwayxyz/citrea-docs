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

### Citrea is a [Type 2 zkEVM](technical-specs/characteristics/execution-environment.md)

Citrea processes a large number of zkEVM transactions in batches and generates a succinct zkSNARK proof from them. These proofs are then published to Bitcoin directly. This allows for easy verification of the batch validity in an inscription-like envelope.

### Citrea is a **rollup on Bitcoin**

Citrea **batch proofs** also includes the **state differences** for each batch, meaning that any changes from block A to block B in a batch are also available for anyone who's reading Bitcoin. This allows a Citrea full node to **directly read the state from Bitcoin permissionlessly** and reconstruct the state - which is the key of Citrea being a rollup on Bitcoin, rather than a sidechain.

### Citrea has a native trust-minimized two-way peg: Clementine

Clementine is the BitVM-based trust-minimized two-way peg mechanism implemented by Citrea Team. BTC pegging mechanism uses the idea of BitVM at it's core, and it utilizes Light Client Proofs that includes recursively proven batch proofs. 

Clementine has a 1-of-N trust assumption, which means that as long as one single entity in the bridge multisig is honest, no party can steal the pegged BTC from the bridge. 

You can read more about Clementine [here](https://www.blog.citrea.xyz/unveiling-clementine/).

### Citrea is fully open-source

You can find the source code of [Citrea](https://github.com/chainwayxyz/citrea) and [Clementine](https://github.com/chainwayxyz/clementine) here. It's been built in public from day one. 

You can also check many other projects and other relevant open-sourced components from our organization page on Github, [Chainway Labs](https://github.com/chainwayxyz).

-----

Citrea uses $BTC as its native token. To avoid confusion and improve on/off-ramp UX; Citrea native $BTC is referred to as $cBTC in Citrea.

In the following pages of this documentation, you can find much more details on how Citrea works and what does each component do in its architecture.
