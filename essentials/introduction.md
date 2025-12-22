# TL;DR

Citrea is the first rollup that enhances the capabilities of Bitcoin blockspace with zero-knowledge technology, making it possible to **build everything on Bitcoin**.&#x20;

{% hint style="success" %}
**Citrea requires no changes to the network's consensus rules. Citrea and Clementine have been live on Bitcoin Testnet4 since September 2024.**
{% endhint %}

### <mark style="background-color:yellow;">Citrea is a</mark> <mark style="background-color:yellow;"></mark><mark style="background-color:yellow;">**rollup on Bitcoin**</mark>

**Citrea is a rollup**. It regularly submits **batch proofs** to Bitcoin - which also include the **state differences** for each batch, meaning that any changes from the start to the end of a batch are also available for anyone reading Bitcoin.&#x20;

A Citrea full node can read the state differences and reconstruct the state directly from Bitcoin - unlike sidechains, in which only small cryptographic commitments are posted and it is impossible to reconstruct the state using that data.

Citrea uses **Bitcoin as a Data Availability layer**, and it's the only source of truth for the system. As long as Bitcoin is secure, the state of Citrea will be accessible and reconstructable.

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2FbhAqhd1iRL2kcYTwrOYJ%2Fimage.png?alt=media&#x26;token=483a4181-657a-4bec-888d-c5be8bbed419" alt=""><figcaption></figcaption></figure>

### <mark style="background-color:yellow;">Citrea is a</mark> [<mark style="background-color:yellow;">Type 2 zkEVM</mark>](https://docs.citrea.xyz/essentials/execution-environment)

Citrea is **EVM-compatible**. As a developer, you can write & deploy your smart contracts in Solidity using the standard EVM tooling.

Citrea processes a large number of zkEVM transactions off-chain. In **batches,** using these transactions, it generates succinct **STARK** proofs (later wrapped into succinct **SNARK** proofs). These proofs are then published to Bitcoin directly. This allows for easy client side verification of the batch validity in an inscription-like envelope.

### <mark style="background-color:yellow;">Citrea's currency is Bitcoin</mark>

Citrea uses **Bitcoin** as its native currency. To avoid confusion and improve on/off-ramp UX, Citrea’s native Bitcoin is referred to as Citrea Bitcoin, **$cBTC** in short.

{% hint style="warning" %}
Citrea uses $BTC (cBTC) as its native token. To avoid confusion and improve on/off-ramp UX, Citrea’s native $BTC is referred to as $cBTC. **There is no Citrea token**. Please beware of scams!
{% endhint %}

### <mark style="background-color:yellow;">Citrea is designed to scale Bitcoin</mark>

As a separate execution layer, Citrea has different properties compared to Bitcoin and Ethereum. As an **independent execution layer**, it offers higher throughput, richer smart‑contract functionality, and lower fees than Bitcoin’s base layer, while still anchoring finality to Bitcoin.

| Dimension         | Citrea                  | Ethereum | Bitcoin        |
| ----------------- | ----------------------- | -------- | -------------- |
| Execution         | zkEVM                   | EVM      | Bitcoin Script |
| Settlement        | Bitcoin                 | Ethereum | Bitcoin        |
| Data availability | Bitcoin via State Diffs | Ethereum | Bitcoin        |
| Fees & Currency   | BTC (cBTC)              | ETH      | BTC            |

### <mark style="background-color:yellow;">Citrea has a native trust-minimized two-way peg: Clementine</mark>

Clementine is the BitVM-based trust-minimized two-way peg mechanism of Citrea. It uses **Light Client Proofs** that include recursively proven Citrea batch proofs, which enables **optimistic ZK Proof verification** **on Bitcoin** using BitVM.

Clementine is **trust-minimized** as it has a **1-of-N trust assumption**, which means that as long as one single entity in the bridge verifier set is honest, no party can steal the pegged BTC from the bridge.&#x20;

Clementine is deployed on Citrea testnet. You can read more about Clementine [here](https://docs.citrea.xyz/essentials/clementine-trust-minimized-bitcoin-bridge).

### <mark style="background-color:yellow;">Citrea is audited & fully open-source</mark>

Citrea & Clementine are built in public.

Both Citrea and Clementine codebases are open-source:&#x20;

* <https://github.com/chainwayxyz/citrea>
* <https://github.com/chainwayxyz/clementine>

Both Citrea and Clementine codebases are [audited](https://docs.citrea.xyz/security/audits-inquiries) as of October 2025.
