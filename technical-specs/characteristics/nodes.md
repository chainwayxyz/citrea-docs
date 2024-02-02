# Nodes

## Full Nodes

Full nodes in Citrea are the nodes that sync with the sequencer(s) as well as verify the zk proofs. Full nodes are designed for users who need instant confirmations from the sequencer or need the full history of Citrea.

A sequencer that produced a block broadcasts the block over a network of full nodes. Full nodes apply the sequencer block to their local state. RPC endpoints can now serve the block data to explorers, wallets, and other applications without waiting for additional zk proving. After a batch of blocks is proven and inscribed in Bitcoin, full nodes extract and verify the proofs. According to the result of the proof they confirm, finalize, or revert the sequencer broadcasted blocks.

## Light Nodes

&#x20;A light node is a node that is designed to fully validate the full nodes' responses using minimal bandwidth and storage requirements. In Citrea, light nodes run next to a Bitcoin light node (SPV) or full node and only need the several latest Bitcoin block headers to trustlessly access the latest light client proof. They can also directly connect to the peer-to-peer network and retrieve the light client proof through the network.

Using the state root extracted from the light client proof, Citrea light nodes can validate full node and RPC endpoint responses. With the advancements in light node technology on Bitcoin like ZeroSync header chain proofs, the proofs that verify a chain of Bitcoin block headers thus allow instant sync with Bitcoin headers allowing Citrea light nodes to sync with the rollup near-instantaneously.&#x20;

This improvement significantly reduces the trust on full nodes and boosts decentralization of Citrea with more verifying nodes. Such a light node can also live in another blockchain, which enables trust-minimized bridges with every other smart contract-capable blockchain ecosystem.
