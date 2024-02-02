# Re-org Resistance

Citrea blocks are proposed by a block producer and acknowledged by nodes, which both need to have an access to Bitcoin network directly. Nodes see and verify Citrea blocks through the Bitcoin network.

Every block header produced on Citrea has a field for the last finalized Bitcoin blockhash, which is configured and baked in into the state transition function. As every participant in the network has the same finalized block after some confirmation, they can validate or invalidate the Citrea block by looking at Bitcoin blockhash in its header. An adversary who wants to re-org Citrea after some confirmation must re-org the Bitcoin network as well to make nodes validate the new fork. After a Citrea block header is pushed in a batch, it is impossible to re-org the block without re-orging Bitcoin, which is almost impossible after 6 blocks.
