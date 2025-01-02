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

Citrea processes a large number of zkEVM transactions in batches and generates a succinct STARK proof (later wrapped into a succinct SNARK proof) from them. These proofs are then published to Bitcoin directly. This allows for easy client side verification of the batch validity in an inscription-like envelope.

### Citrea is a **rollup on Bitcoin**

Citrea **batch proofs** also includes the **state differences** for each batch, meaning that any changes from batch start to batch end in a batch are also available for anyone who's reading Bitcoin. This allows a Citrea full node to read the state differences and reconstruct the state from Bitcoin - which is the key of Citrea being a rollup on Bitcoin, rather than a sidechain.

Citrea uses **Bitcoin as a Data Availability layer**, and it's the only source of truth for the system. As long as Bitcoin is secure, nodes always continue to function.

### Citrea has a native trust-minimized two-way peg: Clementine

Clementine is the BitVM-based trust-minimized two-way peg mechanism of Citrea. BTC pegging mechanism uses the idea of BitVM at its core, and it utilizes Light Client Proofs that includes recursively proven Citrea batch proofs. These proofs are used in the Clementine bridge and gets optimistically verified in Bitcoin directly, thanks to BitVM. 

Clementine has a 1-of-N trust assumption, which means that as long as one single entity in the bridge verifier set is honest, no party can steal the pegged BTC from the bridge. Clementine is partially deployed on Citrea testnet - excluding the BitVM implementation.

You can read more about Clementine [here](https://www.blog.citrea.xyz/unveiling-clementine/).

### Citrea is fully open-source

Citrea is built in public.

Citrea codebase is open-source: [https://github.com/chainwayxyz/citrea](https://github.com/chainwayxyz/citrea).

Clementine codebase is source available: [https://github.com/chainwayxyz/clementine](https://github.com/chainwayxyz/clementine).

-----

Citrea uses $BTC as its native token. To avoid confusion and improve on/off-ramp UX, Citrea’s native $BTC is referred to as $cBTC.

In the following pages of this documentation, you can find much more details on how Citrea works and what does each component do in its architecture.
