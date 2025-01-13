# Citrea Sequencer

The Citrea sequencer is a specialized full node with the crucial task of ordering user transactions and building rollup blocks. It acts as the primary gateway for users to interact with the rollup by including their transactions in the blocks.

#### Key Functions

- **Transaction Ordering**: The sequencer continuously monitors the mempool for pending transactions.

- **Block Building**: Every two seconds, the sequencer assembles a new Citrea block from the transactions in the mempool. It executes these transactions against the current state of the rollup, applying the necessary state transitions using STF (State Transition Function).

- **Soft Confirmation Generation**: Upon building a new block the sequencer generates a **soft confirmation**, which consists of the following:

```rust
pub struct SoftConfirmationReceipt<DS: DaSpec> {
    pub l2_height: u64,
    pub hash: [u8; 32],
    pub prev_hash: [u8; 32],
    pub da_slot_height: u64,
    pub da_slot_hash: <DS as DaSpec>::SlotHash,
    pub da_slot_txs_commitment: <DS as DaSpec>::SlotHash,
    pub l1_fee_rate: u128,
    pub tx_hashes: Vec<[u8; 32]>,
    pub deposit_data: Vec<Vec<u8>>,
    pub timestamp: u64,
    pub soft_confirmation_signature: Vec<u8>,
    pub pub_key: Vec<u8>,
}
```

This information is then signed & broadcasted to the network, providing a soft-finality for the full nodes regarding the next transactions & blocks to be written to Bitcoin DA.

- **Sequencer Commitment Generation**: The sequencer generates a cryptographic commitment to the Citrea blocks that are inscribed on Bitcoin. This commitment is then submitted to the Bitcoin network, providing a finality for the transactions and blocks. You can check the [sequencer commitments](./sequencer-commitments.md) section for more information.

- **Interaction with Prover**: Along with the process above, the sequencer also submits a batch of blocks to prover. The prover node generates a proof and inscribes it to Bitcoin, which is used by full nodes verification & proven finalization.

![A more detailed block cycle](../../../.gitbook/assets/block_cycle.png)

#### Trust Assumptions

Regarding the trust assumptions that come with sequencer's role, it's important to recall that zero-knowledge proofs are used to ensure the validity of the transactions and the block. Combined with force transaction mechanism and on-chain data availability, the sequencer may cause liveness failures, but it cannot act maliciously and damage users' funds or freeze them voluntarily. 

<!-- TODO: Add transaction selection logic a bit here -->
