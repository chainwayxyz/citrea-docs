# Bitcoin Appchains (L3)

Citrea lets you deploy **Bitcoin Appchains (L3s)**: application-specific OP Stack chains that **settle to Citrea**, while using a modular **Data Availability (DA)** layer like **Celestia** or **Avail**.

From an architecture point of view:

* **Bitcoin** is the base layer.
* **Citrea (L2)** is a rollup on Bitcoin (zkEVM).
* **Your Appchain (L3)** is an OP Stack chain that treats **Citrea as its settlement layer**, and uses **Celestia/Avail for DA**.

***

### Why an L3 ₿appchain?

An L3 is useful when you want **dedicated blockspace** and **predictable execution** for a single application (or app ecosystem), without competing for fees with everything else.

Some easy benefits from an infra perspective are as follows:

* **Dedicated blockspace**: your app’s throughput/fees are isolated from other apps.
* **Custom chain parameters**: block time, gas limits, sequencing policy, etc.
* **Lower DA costs**: batch data goes to a DA network (Avail/Celestia) instead of being fully posted on the settlement layer.
* **EVM + Ethereum tooling**: standard JSON-RPC, op-geth/op-node, Foundry, etc.

{% hint style="info" %}
Note: Citrea does not support EIP-4844 blobs at the moment. Therefore using an external DA layer keeps data costs and throughput more flexible.
{% endhint %}

***

### How an L3 works on Citrea

At a high level, an OP Stack L3 has the usual OP Stack components — except it settles to **Citrea**, and uses **Alt-DA mode** for data availability.

```
Users
  |
  | (JSON-RPC)
  v
L3 execution (op-geth)  <----- engine API ----->  L3 rollup node (op-node)
  |                                               |
  | produces blocks                               | derives chain by reading:
  v                                               |  - Citrea commitments
op-batcher                                        |  - DA data (Celestia/Avail)
  |
  | uploads batch data
  v
DA Server  --->  DA Layer (Celestia or Avail)
  |
  | submits DA commitment tx to Citrea
  v
Citrea (settlement + OP Stack contracts for the L3)
  ^
  | proposes outputs
  v
op-proposer
```

#### What each component does

* `op-geth`: Executes L3 transactions and serves the L3 JSON-RPC.
* `op-node`: Derives the canonical L3 chain by combining:
  * commitments on **Citrea**, and
  * batch data retrieved from the **DA layer**.
* `op-batcher`: Compresses L3 blocks into batches and publishes them.
* `DA Server (sidecar)`: The adapter between OP Stack and the DA network.
* `op-proposer`: Submits L3 output roots to Citrea (settlement/bridging logic).

***

### Data Availability via OP Stack Alt-DA mode

These ₿appchains use **Alt-DA mode** in the OP Stack:

1. The batcher **uploads** compressed batch data to the DA layer (via the DA Server).
2. The batcher **posts a commitment** on Citrea that points to that data.
3. Any node can follow Citrea, then use the DA Server to fetch the underlying batch data and fully derive the L3 chain.

{% hint style="info" %}
Note: Alt-DA mode is a beta OP Stack feature. The “generic commitment / DA-server” flow adds additional trust assumptions compared to posting all batch data directly on the settlement layer.
{% endhint %}

Both Celestia and Avail are DA networks designed to keep large volumes of transaction data available at lower cost than typical settlement layers.

#### Avail DA

* You run an **Avail DA Server** sidecar and publish data under an **AppID**.
* Avail’s model includes **light clients performing DAS**, with availability verification built around commitments/proofs (e.g., KZG-based designs).

#### Celestia DA

* You run a **Celestia Light Node** and a **Celestia DA Server** sidecar.
* Celestia is built around **Data Availability Sampling (DAS)** and **Namespaced Merkle Trees (NMTs)**.

***

### Transaction lifecycle (end-to-end)

1. A user sends a transaction to your **L3 RPC**.
2. The **sequencer** orders transactions and executes them in **op-geth**.
3. **op-batcher** collects L3 blocks and compresses them into batches.
4. Batch data is posted to **Celestia/Avail** via the **DA Server**.
5. A **DA commitment** is submitted to **Citrea**.
6. **op-node** derives the L3 state by reading Citrea + pulling batch data from the DA layer.
7. **op-proposer** periodically posts output roots to Citrea.

***

### Security model and tradeoffs

An L3 ₿appchain inherits different guarantees from different layers:

* **Settlement**: anchored to **Citrea** (and Citrea anchors to Bitcoin).
* **Execution correctness**: enforced by OP Stack rules and the L3 derivation pipeline.
* **Data availability**: depends on the selected DA layer **and** the Alt-DA publication/retrieval path.

If you are deploying an L3 beyond experimentation, you should add operational monitoring for:

* batch publication to DA,
* DA retrieval health,
* consistency between Citrea commitments and DA data.

***

For the remainder of this section we provide example deployments of ₿appchains.&#x20;

If you want to learn more or build one from a business perspective, feel free to reach us at [https://origins.citrea.xyz](https://origins.citrea.xyz/).
