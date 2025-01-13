# Nodes

Nodes are machines that run in Citrea network.

There are three types of nodes in Citrea currently:

- [Sequencer](/technical-specs/characteristics/block-production/sequencer.md): A special node that produces blocks in the Citrea network.
<!-- TODO: Fix this link once prover details are more organized -->
- [Prover](/technical-specs/characteristics/proof-generation.md): A special node that generates zero-knowledge proofs of execution for the sequencer's blocks.
- Full Node

## Full Nodes

Full nodes in Citrea are the nodes that sync with the sequencer. They also verify the ZK proofs by reading the DA. Full nodes are designed for users who need instant soft-confirmations from the sequencer or need the full history of Citrea.

#### How it works

Once the sequencer produces a block and broadcasts it over a network of full nodes, full nodes apply the sequencer block to their local state. Their RPC endpoints can then serve the block data to explorers, wallets, and other applications without waiting for additional zk proving unless they're waiting for a finalization. 

After a batch of blocks is proven and inscribed in Bitcoin, full nodes extract and verify the proofs. According to the result of the proof they confirm, finalize, or revert the sequencer broadcasted blocks.

Full nodes can store the entire state & history of Citrea. Depending on the run configuration, they can also provide transaction receipts with more details.

We're working towards multiple types of full node implementations, such as archival & pruned full nodes, to let users choose the node type that best suits their needs.

#### Who can run a full node?

Anyone can run a full node permissionlessly for their own security. However, please note that there are no keys or incentives to run a full node. For more details, you can visit the node running guide [here](/users/node/run-a-node.md).
