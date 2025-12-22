# Transaction Finality

As a rollup on Bitcoin, Citrea has three layers of finality:

* `Confirmed`
* `Finalized`
* `Proven`

The degree of finality begins when a block is *soft-confirmed* by the sequencer, then *finalizes* when the batch window that contains it is finalized on Bitcoin, and reaches its highest level when that window’s execution is *proven* and reconstructable from Bitcoin L1 itself.&#x20;

When BTC <-> cBTC moves across the bridge, Bitcoin settlement and a short challenge window complete the full process.

### Soft Confirmations

A transaction on Citrea first appears inside a **sequencer-signed soft confirmation (L2 Blocks)** with a block. At this point, the block has been executed in the zkEVM, a new state root has been produced, and full nodes can update their local view immediately. Soft-confirmations enable responsive interfaces and optimistic flows. On the other hand, though, they are not anchored to Bitcoin yet and remain mutable.&#x20;

The sequencer's signature provides a soft-finality for full nodes. However, as mentioned, the sequencer could still reorder transactions in this range - therefore soft confirmations are great for the UX as a direct confirmation from the sequencer itself but the transactions are not finalized at this stage.

<details>

<summary><strong>Soft Confirmation Fields</strong></summary>

Each soft confirmation encapsulates essential information about a block:

* **Header:**
  * **Height:** The block height/header
  * **Previous Hash**: The hash of the preceding soft confirmation, forming a chain of blocks that prevents replay attacks and ensures correct ordering.
  * **State Root**: The root hash of the rollup state after applying the transactions in this soft confirmation.
  * **Transactions Merkle Root:** A 32-byte Merkle root of all transactions included in the block
  * **L1 Fee Rate**: The fee rate on Bitcoin L1, relevant for calculating transaction costs.
  * **Timestamp**: The timestamp of the block, recording when it was produced by the sequencer.
* **Header Hash:** Rollup block header hash
* **Signature**: The sequencer's signature on the soft confirmation data.
* **Transactions**: List of transactions included in the block.

The structure above is signed and distributed to the network. Soft confirmations form the basis for *sequencer commitments*, which are published to the DA layer for finalization.

</details>

### Finalized

A batch window of Citrea becomes finalized when the sequencer ***inscribes*** a commitment to Bitcoin and that inscription reaches the required confirmation depth. These commitments are called **Sequencer Commitments** and contain the following info:

* a Merkle root over the soft-confirmation hashes
* the batch window’s start and end heights on Citrea&#x20;

Once a sequencer commitment is finalized on Bitcoin, the ordering of blocks in that window is pinned and the exact set of soft-confirmed blocks that will be proven next is canonical. Therefore, the sequencer commitments provide a great degree of finalization as the order of transactions and blocks are now certain. The finalization here does not prove the execution itself. Yet, it defines the canonical sequence to be proved next.

Sequencer commitments are also used in the **Batch Proof Circuits** to make sure proofs are generated for the canonical batch used in sequencer commitments, too. **Light Client Proofs** that are used in BitVM based [Clementine](https://docs.citrea.xyz/essentials/clementine-trust-minimized-bitcoin-bridge) bridge benefit from the sequencer commitments indirectly, too.&#x20;

<details>

<summary>Querying Sequencer Commitments</summary>

Sequencer commitments can be queried by using the **Citrea Batch Explorer**, along with the [RPC endpoints](https://docs.citrea.xyz/developer-documentation/rpc-documentation/citrea-rpc-documentation). You can find the explorer [here](https://citrea.xyz/batch-explorer).

</details>

### Proven

The highest level of finality is reached when a batch’s **execution is proven** and posted on Bitcoin. A batch enters **proven finality** once a succinct ZK proof attesting to the correct execution of that batch (called a *batch proof*), along with the minimal state data needed (state diffs), is inscribed on Bitcoin and gains sufficient confirmations. At this point, the entire state transition of that batch is *cryptographically validated* end-to-end, and the necessary data to reconstruct the new state is available on the Bitcoin L1. In other words, any node can *independently rebuild Citrea’s state from Bitcoin alone* by applying each batch’s state diff in order, starting from genesis.

Proven finality provides an **irrefutable guarantee** of correctness. Full nodes will retrieve the batch proof transaction from Bitcoin, verify the zkSNARK, and if the proof is valid, the batch of L2 blocks is conclusively final. Even if the sequencer (or any actor) was malicious, they *cannot* fake a valid proof for an incorrect state transition - thus proven finality is the ultimate safeguard for Citrea’s security.&#x20;

The rollup blocks are as secure as the Bitcoin proof-of-work that cemented its data and the cryptographic soundness of the zk-proof. Except for an extreme Bitcoin reorganization beyond the finality depth (e.g. >6 blocks), a proven batch cannot be reversed or disputed – the L2 block is effectively as final as an L1 Bitcoin transaction.

<details>

<summary>Batch Proof Inscription Contents</summary>

Each batch proof inscription contains the necessary components for verifiers to validate and apply the batch:

* **Succinct ZK Proof:** A zkSNARK (generated by Citrea’s batch prover) proving that if one starts from the previous proven state and executes all transactions in the batch, the resulting state matches the sequencer’s claims. This proof attests to the validity of the batch’s state transition.
* **State Difference:** A compressed representation of the state changes introduced by the batch (often called the *state diff*). This is the minimal data needed to update the prior state to the new state. Instead of posting full transaction data, Citrea posts the state diff between pre- and post-state, which is constrained by the zk-circuit (any alteration would break the proof).
* **Initial & Final State Roots:** The state root before the batch and the state root after the batch, proving the state was correctly updated. These are used as public inputs to the circuit and help nodes quickly verify they apply the correct batch in sequence.
* **Batch Block Range:** The index range of Citrea blocks covered by this batch (e.g. “L2 blocks 1000–1050”). This links the proof to a specific sequence of soft-confirmed blocks.
* **Referenced Bitcoin Block:** The hash (identifier) of the Bitcoin block containing the sequencer commitment for this batch. This ties the proof to the exact L1 commitment it is proving, ensuring the proof’s batch corresponds to an anchored commitment in a particular Bitcoin block.

When a batch proof is published, full nodes use these components to validate the proof and apply the state diff to their local state. Once verified, the batch’s blocks are considered fully final — the proof serves as a permanent, Bitcoin-backed *certificate of correctness* for those transactions.

</details>
