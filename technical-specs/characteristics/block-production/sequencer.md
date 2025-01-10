# Citrea Sequencer

The Citrea sequencer is a specialized full node with the crucial task of ordering user transactions and building rollup blocks. It acts as the primary gateway for users to interact with the rollup by including their transactions in the blocks.

#### Block Production

Essentially, the Citrea sequencer continuously monitors the transaction mempool. Every two seconds, the block production is triggered and the sequencer selects some transactions from the mempool. These transactions are then executed against the current state of the rollup, and if successful, the sequencer includes them into the block. The block is then signed by the sequencer and broadcasted to the network.

<figure><img src="../../../.gitbook/assets/block_prod.png" alt=""><figcaption><p>Block Production in Citrea</p></figcaption></figure>

#### Soft Confirmations

At the time of broadcast, these transactions are also sent to the prover. Until the [sequencer commitments](./sequencer-commitments.md) & [batch proofs with state diffs](https://www.blog.citrea.xyz/citreas-batch-proofs/) are submitted to Bitcoin, these transactions are not finalized. For this reason, the sequencer gives a _soft confirmation_ to the full nodes. A soft confirmation essentialy provides a soft-finality for the full nodes regarding the next transactions & blocks to be written to DA.

A soft confirmation signed by the sequencer includes the following data:

- Current Citrea block height
- Citrea Citrea block hash & previous Citrea block hash
- Current finalized Bitcoin block height & block hash
- Transaction data & signatures

and more. 

![A more detailed block cycle](../../../.gitbook/assets/block_cycle.png)

Regarding the trust assumptions that come with sequencer's role, it's important to recall that zero-knowledge proofs are used to ensure the validity of the transactions and the block. Combined with force transaction mechanism and on-chain data availability, the sequencer may cause liveness failures, but it cannot act maliciously and damage users' funds or freeze them voluntarily. 

<!-- TODO: Add transaction selection logic a bit here -->
