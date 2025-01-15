# Citrea Sequencer

The Citrea sequencer is a specialized full node with the crucial task of ordering user transactions and building rollup blocks. 

#### Process of Block Production

Sequencer carries the following operations to build a block, in detail:

- It continuously monitors the [mempool](./mempool.md), a dedicated area for pending transactions. 
- Every two seconds, the sequencer assembles a new block from the transactions in the mempool. The subset of transactions selected from the mempool are stated under the mempool section. The ordering of these transactions are based on their priority fees.
- Along with user transactions, sequencer is also responsible for including system transactions, such as updating the `L1BlockHash` in the [BitcoinLightClient](/developer-documentation/system-contracts/bitcoin-light-client.md) or handling [Deposit](/developer-documentation/system-contracts/bridge.md) transacations.
- The sequencer executes the assembled transactions against the current state of the rollup, updates the balances, storage values, and state. Gas usages and state changes are recorded in transaction receipts.
- Once the trasactions are executed, the sequencer also generates a signed [soft confirmation](./soft-confirmation.md). This provides a soft-finality to the transactions.
- The signed soft confirmation with block data is broadcasted to the network for full nodes to update their local chain state.

Soft confirmations does not provide finality - you can refer to [Sequence Commitments](./sequencer-commitments.md) for more information.

#### Trust Assumptions

Regarding the trust assumptions that come with sequencer's role, it's important to recall that ZK Proofs are used to ensure the validity of the transactions and the block. Combined with force transaction mechanism and on-chain data availability, the sequencer may cause liveness failures, but it cannot act maliciously and damage users' funds or freeze them voluntarily. 
