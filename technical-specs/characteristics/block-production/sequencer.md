# Citrea Sequencer

<!-- <figure><img src="../../../.gitbook/assets/block_prod.png" alt=""><figcaption><p>Block Production in Citrea</p></figcaption></figure> -->

The Citrea sequencer is a specialized full node with the crucial task of ordering user transactions and building rollup blocks. It acts as the primary gateway for users to interact with the rollup by including their transactions to the blocks.

Essentially, the Citrea sequencer continuously monitors the transaction mempool. In every two seconds, the block production is triggered and the sequencer selects some transactions from the mempool. These transactions are then executed against the current state of the rollup, and in case it's successful, the sequencer includes them into the block. The block is then signed by the sequencer and broadcasted to the network.

At the time of broadcast, these transactions are also sent to the prover. Until prover submits the batch of transactions into the Bitcoin, these transactions are not finalized. In this case, the sequencer gives a _soft confirmation_ to the full nodes. A soft confirmation essentialy provides an insight for the full nodes regarding the next transactions & blocks to be written to DA. It also includes some additional data, such as

- DA Block Height & Hash
- Batch Hash
- Previous State Root
- Post State Root
- L1 Fee Rate
- Block Timestamp

and so on. 

Key idea of soft confirmations is to get a soft finality from the sequencer regarding the transcations and their orders, and progressing with further transactions until batches are finalized on Bitcoin. It's also important to recall that zero-knowledge proofs are used to ensure the validity of the transactions and the block, and combined with force transaction mechanism and on-chain data availability, the sequencer can't steal users' funds or freeze them voluntarily.

<!-- TODO: Add transaction selection logic a bit here -->
<!-- TODO: Talk more on soft-confirmation fields -->