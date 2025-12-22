# Architecture and Transaction Lifecycle

## System Architecture Overview

***

Citrea’s architecture consists of several core components that work together. Mainly, these include:

* a private Mempool for transaction staging
* a Sequencer node for block production
* a Batch Prover node and a Light Client Prover node for zero-knowledge proof generation
* permissionless Full Nodes for verification
* a BitVM-based trust-minimized bridge, Clementine

Each component has a distinct role, yet they complement each other to ensure that transactions are executed correctly and finality is achieved by the finalization of published proofs in Bitcoin.&#x20;

Below, let's introduce each major component and its function in the system first:

### Mempool

Citrea has a private **mempool** (memory pool) as a temporary staging area for pending transactions before they are included in a block. The mempool enforces some basic validity and resource checks first. If the transaction passes these checks, it is added to the mempool.&#x20;

<details>

<summary>Basic Mempool Checks</summary>

* Does the address have enough balance?
* Does the transaction have a valid nonce that is greater than or equal to the current nonce?
* Is there enough space in the mempool?
* Does it fit into account limits?

</details>

Citrea mempool has custom [SubPools](https://reth.rs/docs/reth_transaction_pool/enum.SubPool.html#variants) that use `reth-transaction-pool` architecture. For **Queued**, **BaseFee**, and **Pending** pools, Citrea has the 10x configuration limits compared to default values of Ethereum: **100,000** transactions per subpool with **200 MB** limit. &#x20;

Another custom mempool configuration is the number of transaction slots per account, `max_account_slots`. Citrea has a maximum of **64 slots per account** in the mempool to prevent spamming.

{% hint style="info" %}
Citrea does not have a Blob pool as it does not support blob transactions at the moment (no EIP-4844)&#x20;
{% endhint %}

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2FYApH3KQmTcYO2LdJ2IBT%2Fimage.png?alt=media&#x26;token=056662c9-6dc0-45d4-8e01-333a80f1a0c4" alt=""><figcaption></figcaption></figure>

### Sequencer

The Citrea sequencer is a specialized full node responsible for ordering user transactions and constructing rollup blocks. It continuously monitors the mempool and, every two seconds, selects a batch of valid transactions (prioritized by fees) to form the next block. Along with these user transactions, the sequencer also injects [system transactions](https://docs.citrea.xyz/developer-documentation/system-contracts#system-transactions) at the beginning of the block, covering important operations such as updating the latest Bitcoin block headers or handling bridge events. The sequencer executes all these transactions against the current state of the rollup, updates balances and contract storage, and computes a new state root. Gas usage and state changes are also stored in transaction receipts.&#x20;

Upon assembling and executing a block, the sequencer generates, signs, and broadcasts a **soft confirmation** that attests to the block's content and the state root. This provides a fast & provisional confirmation of transactions/blocks for full nodes (i.e., soft-finality).

Lastly, the sequencer is also responsible for generating sequencer commitments, which provide finality for transactions and blocks once settled on Bitcoin.

{% hint style="success" %}
Regarding trust assumptions, the sequencer has the authority over choosing and ordering transactions and may cause liveness failures (delays), but it **cannot violate** correctness, steal funds, or freeze them arbitrarily.&#x20;

Validity proofs and on-chain data availability on Bitcoin guarantee the integrity of state transitions and the validity of each block.
{% endhint %}

### Batch Prover

The Batch Prover is a dedicated node that generates ZK Proofs that attest to the correctness of Citrea blocks in **batches**. As mentioned above, sequencer posts commitments to Bitcoin. Once these commitments are finalized on Bitcoin, the prover fetches the corresponding batch of Citrea blocks and replays their execution inside a zkVM, and produces a succinct proof for the entire state transition. This proof is equivalent to the following statement: “***if you start from the last proven state and execute all Citrea blocks in order, you will arrive at the state root claimed by the sequencer***” – using this, anyone (e.g. full nodes) can verify without replaying the transactions themselves.

Citrea’s batch prover uses a Groth16 circuit (running in the [RISCZero](https://risczero.com/) zkVM environment, with [Boundless](https://beboundless.xyz/)). Once the proof for a given batch of blocks is generated, the batch prover packages the proof (and any needed public inputs such as the old and new state root, and the range of block indices covered) into a transaction that is posted on the Bitcoin network.

{% hint style="success" %}
Posting the proof to Bitcoin serves a similar purpose as posting sequencer commitments: it timestamps and makes public the evidence that the Citrea blocks are valid. Full nodes and other verifiers will pick up the proof from Bitcoin and check its validity. If valid, this proof guarantees the finality of the batch of L2 blocks in question, since we now have a cryptographic guarantee of their correctness that is recorded on Bitcoin.
{% endhint %}

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2FHVYinR2zsDgR8xab3oB3%2Fimage.png?alt=media&#x26;token=df0f0496-0967-47d6-8f34-63d2297d803d" alt=""><figcaption></figcaption></figure>

### Light Client Prover

The Light Client Prover (LCP) is a specialized node that produces a recursive proof of the entire Citrea rollup state.

The mission of the LCP is to enable light clients (and other external verifiers like smart contracts on other chains) to easily verify the latest state of the rollup with minimal data, by checking a single proof that encapsulates all necessary history. To accomplish this, the LCP runs a light client circuit continuously. The circuit processes each new Bitcoin block along with the previous light client proof, producing a new proof that verifies the correct Citrea state root for that block.

Essentially, each light client proof builds on the prior one, forming *an unbroken chain of proofs* (a recursive proof sequence) from the start of the rollup up to the current state. By verifying just the latest proof, a light client can be confident that all the Citrea blocks and all the batch proofs in the rollup’s history (up to that point) were valid and consistent with what was posted on Bitcoin.

{% hint style="success" %}
The primary use case for the LCP’s output in Citrea’s design is to feed into [Clementine](https://citrea.xyz/clementine_whitepaper): the Bitcoin bridge requires a valid LCP to resolve any disputes about the rollup state in BitVM. This way, it inherits the security from the ZK proof if an operator tries to cheat. The LCP combines the security of both Bitcoin and the batch proofs into one succinct artifact.
{% endhint %}

### Full Nodes

A full node is a permissionless participant that anyone can run to fully verify the rollup’s state and serve data to others. A full node keeps a complete copy of Citrea’s state (all accounts, contracts, etc.) and the history of transactions. Its responsibilities include:

* **Syncing Citrea Blocks:** It connects to the Sequencer and receives every new block (and soft-confirmation). The full node replays the transactions in each block, applying the **state transition function** to update its copy of the state (and recompute the new state root).
* **Monitoring Bitcoin:** The full node follows Bitcoin’s blocks to detect Citrea data posted on Bitcoin. For every finalized Bitcoin block, it fetches and verifies **sequencer commitments** and **batch proofs** from Bitcoin. Essentially, full nodes use Bitcoin as the source of truth to cross-verify the sequencer’s output.
* **Serving Data & RPC:** Full nodes expose a standard **JSON-RPC API** (compatible with Ethereum’s EVM interface) to allow wallets, dApps, explorers, and users to interact with the Citrea network. Through a full node, one can query contract states, submit transactions, retrieve receipts and proofs, etc., without relying on any centralized service.

In essence, full nodes **ensure trustlessness**: users don’t have to trust the sequencer or any single party, because they can run a full node to independently validate every block and proof. Unlike miners in Bitcoin, Citrea’s full nodes do **not** compete to produce blocks – their role is purely to verify and relay information.&#x20;

### Clementine

**Clementine** is Citrea’s BitVM-based trust-minimized two-way peg for **BTC ↔ cBTC** - a new breakthrough that enables optimistic verification of ZK Proofs directly on Bitcoin without any changes / soft-forks.&#x20;

For full protocol details and diagrams, see: <https://docs.citrea.xyz/technical-specs/characteristics/clementine-trust-minimized-bitcoin-bridge>

## Transaction Lifecycle on Citrea

***

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2FVNtMZiDIyR4DjUkiRRbp%2Fimage.png?alt=media&#x26;token=b6ed57e5-5640-49bc-ab6d-7f18232820fc" alt=""><figcaption><p>The transaction lifecycle with numbers indicating the chronological order.</p></figcaption></figure>

Having introduced the components, let's now walk through the lifecycle of a user-submitted transaction from the moment a transaction is sent, to its inclusion in a Citrea block, and ultimately to its cryptographic finalization on Bitcoin.&#x20;

### **1) Submission to Mempool**

A user initiates a transaction (for example, transferring cBTC or calling a smart contract on Citrea) and broadcasts it to the Citrea network, typically via a full node’s RPC interface. The full node relays the transaction to the [Sequencer](#sequencer), where it enters [**mempool**](#mempool) **if it is valid**.

### **2) Sequencing and Block Production**

As discussed, the sequencer constantly watches the mempool for new transactions. At regular intervals (every \~2 seconds in the current design), the Sequencer selects a set of the highest-priority pending transactions to include in the next block. It then executes these transactions one by one against the current state. During execution, smart contract code is run in Citrea’s EVM environment, state variables are updated, and gas is consumed as in other EVMs.&#x20;

The Sequencer packages the transactions into a **rollup block**. Along with user transactions, the Sequencer also includes system transactions in this block – for example, a transaction to update the BitcoinLightClient with a new Bitcoin block hash, or to process a bridge deposit that became final.&#x20;

### **3) Soft Confirmation Broadcast**

After producing a block, the Sequencer immediately generates a **soft confirmation** for it. A soft confirmation is essentially a signed statement containing the new block’s hash, the previous block’s hash, the new state root, and a reference to the latest Bitcoin block (among other metadata). By issuing a soft confirmation, the sequencer itself guarantees that the signed confirmation is the next in the canonical Citrea chain.

The Sequencer then broadcasts the soft-confirmation (with the block data) to all **Full Nodes** in the network. Full nodes receive the new block and, using the soft confirmation, they update their local view of the chain to include it. They execute the block’s transactions themselves to update their state, verifying that they get the same post-state root. At this point, the transaction is considered *soft-confirmed* on Citrea – users and applications see it as included in a block with an assigned order and can start using it immediately, but this inclusion is still conditional on later being committed on Bitcoin.&#x20;

### **4) Sequencer Commitments on Bitcoin**

To secure the ordering of transactions and blocks, the Sequencer periodically publishes **Sequencer Commitments** to the Bitcoin blockchain. This typically happens after accumulating a batch of Citrea blocks or after a time interval. These commitments include&#x20;

* a Merkle root of recent soft-confirmation hashes (covering a sequence of Citrea blocks)&#x20;
* and the start and end block indices of that batch.&#x20;

By inscribing this data in a Bitcoin transaction, the Sequencer creates a permanent, tamper-resistant record of the Citrea blocks up to that point. Once this Bitcoin transaction gets enough confirmations, those Citrea blocks are considered *finalized in order*: neither the Sequencer nor anyone else can reorder or discard them without contradicting data that is now settled on Bitcoin.

Full nodes watch for sequencer commitments on Bitcoin and, when found, mark the corresponding Citrea blocks as finalized. At this stage, the user’s transaction has an immutable position in the history, anchored by Bitcoin’s proof-of-work. However, one must still ensure that the contents of those blocks are valid – this is where the prover comes in next.

### **5) Batch Proof Generation & Submission**&#x20;

After the Sequencer’s commitment is finalized on Bitcoin, the **Batch Prover** takes over to prove the correctness of that batch of blocks. The prover node fetches the commitment reference(s) from Bitcoin. It then executes a zkSNARK circuit (Batch Proof Circuit) that simulates applying all those transactions in order, from the known previous state root to the new state root claimed by the Sequencer. If the batch of transactions is valid (no fraudulent state transitions), the circuit produces a succinct **batch proof** attesting to that fact.&#x20;

The proving step may take some time (minutes, hours, depending on batch size and proving hardware), but it’s done asynchronously. Once the proof is ready, the Batch Prover embeds the proof along with the batch’s index and state roots into a Bitcoin transaction and publishes it. This action puts the validity proof on Bitcoin for anyone to retrieve. It also timestamps the proof so that it corresponds to a specific Bitcoin block (and by extension, a specific Citrea batch proof submission).

{% hint style="success" %}
Batch Proofs include the state differences of Citrea blocks. That means, anyone running a Bitcoin node can trustlessly reach and sync with Citrea's canonical chain.&#x20;

The proof size depends on the number and type of transactions on Citrea. To prevent non-standard batch proof transactions on Bitcoin and to enable faster finalization of blocks, Citrea uses a chunking mechanism to keep each proof size under 400 kilobytes per transaction.&#x20;

There can be multiple batch proofs posted for any Bitcoin block.
{% endhint %}

### **6) Proof Verification and Finality**

Finally, Citrea full nodes (as well as any external observers) fetch the newly posted batch proofs directly from Bitcoin and verify them against the claimed state transition. Upon sufficient confirmations, full nodes:

* verify the succinct proof for a batch of blocks,
* check whether they match the commitment’s root and `[start, end]`
* update the last finalized/proven block and transaction information in its database.

The node advances its proven tip to the batch’s end height; all transactions in that window are now **proven** on Citrea (reversal would require a deep Bitcoin reorg).

In parallel, the **Light Client Prover** maintains a **recursive** proof of the entire rollup: it ingests the previous LCP proof and the latest Bitcoin block (plus recent batch proofs) to output a single, updated proof that *the Citrea state is correct up to this Bitcoin block*. External verifiers (including the **Clementine** bridge via BitVM) can check **one** proof to learn the latest valid state, without processing whole chain history.

***

Throughout the lifecycle of a transaction, Bitcoin is used at two critical points: first to **commit** to block order (preventing censorship or reordering attacks), and second to **attest** to execution validity (preventing invalid state transitions).&#x20;
