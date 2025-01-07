## Sequencer Commitments

Sequencer commitments are envelopes on Bitcoin that are inscribed by the sequencer before proof generation. Such envelope contains:

- **State Root**: The root hash of the rollup's state tree after applying a set of transactions in blocks. This represents the new state of the rollup.
- **L2 Blocks**:  It includes a range of L2 blocks (identified by their start and end numbers) that were processed to reach the new state.
- **Soft Confirmation Hashes**: Merkle root built on top of the hashes of soft confirmations, representing a set of L2 blocks.

These commitments are inscribed directly on Bitcoin, creating an immutable record.

### Benefits

Once a commitment is inscribed on Bitcoin, the sequencer cannot reorder transactions within that batch without invalidating the commitment. This ensures that the sequencer cannot manipulate the order of transactions in the batch, maintaining the integrity of the batch. 

While it does not guarantee data availability or safety, sequencer commitments prevent re-orgs on L2 blocks once they are commitment inscriptions are finalized. It allows full nodes to verify the sequencer's claims about the state transitions.

Another major usecase of sequencer commitments is that it is used in the Batch Proof circuits for Blockspace Proving. You can read more about it [here](https://www.blog.citrea.xyz/citreas-batch-proofs/).
