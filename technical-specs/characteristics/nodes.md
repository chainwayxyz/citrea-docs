# Nodes

Citrea operates with four main distinct types of nodes.

<figure><img src="../../../.gitbook/assets/block-cycle.png" alt=""><figcaption><p>Nodes in Block Production</p></figcaption></figure>

### 1. [Sequencer](#sequencer-node)

> **Implementation:** <https://github.com/chainwayxyz/citrea/tree/nightly/crates/sequencer>

The sequencer is responsible for the block production and the ordering of transactions. You can read more about it in the [Sequencer](./block-production/sequencer.md) section. 

### 2. [Full Node](#full-node)

> **Implementation:** <https://github.com/chainwayxyz/citrea/tree/nightly/crates/fullnode>

A full node in Citrea has several responsibilities:

- Maintains a complete copy of the Citrea state.
- It syncs with the [sequencer](./block-production/sequencer.md) to receive the latest blocks with transactions, and applies the state transition function over the transactions, updates the local state \& recomputes the state roots.
- Follows Bitcoin, fetches and validates the [sequencer commitments](./block-production/sequencer-commitments.md) inscribed on Bitcoin.
- Validates the [batch proofs](./block-production/batch-proof.md) produced by the Batch Prover, which are also inscribed on Bitcoin.
- Provides a [JSON-RPC interface](/developer-documentation/rpc-documentation/README.md) for users, wallets, explorers, or any application to interact with the network - allows them to query the state, send transactions, and more.

In short, full nodes enable independent verification of all chain data with sequencer commitments and batch proofs. They do not carry any transaction producing/ordering responsibilities. They act as independent data access points for users and applications, allowing them to interact with the Citrea network without relying on any third-party.

### 3. [Batch Prover](#batch-prover)

> **Implementation:** <https://github.com/chainwayxyz/citrea/tree/nightly/crates/batch-prover>

The Batch Prover is a special node that generates ZK Proofs of correct execution for batches of Citrea blocks. 

As mentioned in the [block production](./block-production/README.md), the sequencer posts sequencer commitments on Bitcoin. Once they are finalized, the batch prover fetches these commitments and also retrieves the corresponding batch of Citrea blocks from its local state. Using these two inputs, the batch prover produces a succinct proof that the transition from the previous state to the new state was computed correctly. Therefore, it attests the following: “**_if you start from the last proven state and execute all these transactions in order, you will arrive at the state root claimed by the sequencer_**” – using this, anyone (e.g. full nodes) can verify without replaying the transactions themselves. 

Citrea’s batch prover uses a Groth16 circuit (running in the [Risc0](https://risczero.com) zkVM environment, with [Boundless](https://beboundless.xyz)) to achieve this. Once the proof for a given batch of blocks is generated, the batch prover packages the proof (and any needed public inputs, like the old and new state root, and the range of block indices covered) into a transaction that is posted on the Bitcoin network. 

Posting the proof to Bitcoin serves a similar purpose as posting sequencer commitments: it timestamps and makes public the evidence that the Citrea blocks were valid. Full nodes and other verifiers will pick up the proof from Bitcoin and check its validity. If valid, this proof guarantees the finality of the batch of L2 blocks in question, since we now have a cryptographic guarantee of their correctness that is recorded on Bitcoin.

### 4. [Light Client Prover](#light-client-prover)

> **Implementation:** <https://github.com/chainwayxyz/citrea/tree/nightly/crates/light-client-prover>

The Light Client Prover (LCP) is a specialized node that produces a recursive proof of the entire Citrea rollup state.

The mission of LCP is to enable light clients (and other external verifiers like smart contracts on other chains) to easily verify the latest state of the rollup with minimal data, by checking a single proof that encapsulates all necessary history. To accomplish this, the LCP runs the light client circuit continuously. This circuit takes as input each new Bitcoin block and the previous light client proof, and outputs a new proof that asserts “**_the Citrea state is correct up to this Bitcoin block_**”. 

Essentially, each light client proof builds on the prior one, forming an unbroken chain of proofs (a recursive proof sequence) from the start of the rollup up to the current state. By verifying just the latest proof, a light client can be confident that all the Citrea blocks and all the batch proofs in the rollup’s history (up to that point) were valid and consistent with what was posted on Bitcoin. 

The primary use case for the LCP’s output in Citrea’s design is to feed into [Clementine](https://citrea.xyz/clementine_whitepaper): the Bitcoin bridge requires a valid LCP to resolve any disputes about the rollup state in BitVM. This way, it inherits the security from the ZK proof if an operator tries to cheat. The LCP combines the security of both Bitcoin and the batch proofs into one succinct artifact.
