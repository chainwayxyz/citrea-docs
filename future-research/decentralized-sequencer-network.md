# Decentralized Sequencer Network

<!-- TODO: Link sequencer.md in this text -->
<!-- TODO: Include force_transaction.md in this text -->
<!-- TODO: Possibly link prover.md -->

Almost all rollups today have a single sequencer in the Web3 ecosystem, so does Citrea. While it is sufficient for security (safety) and being non-custodial, liveness failures may still occur. To prevent this, a promising idea is to implement a decentralized sequencer network. We aim to launch Citrea mainnet with a force transaction mechanism, which greatly provides censorship resistance, and decentralized sequencers also benefit this topic.

The main considerations are:
- **Architecture**: Deciding the sequencer set, making it permissionless or not, modifying the architecture, determining the best consensus (CometBFT, Hotstuff, MonadBFT), and making further adjustments are all crucial properties, each with tradeoffs.
- **Latency**: Achieving consensus among multiple sequencers is not easy, it bring some latency to the system and it should not impact responsiveness of the rollup hugely.
- **Economic Incentives**: There are interesting ideas stemming from an economic perspective and a game theory perspective on the decentralized networks. It's important to ensure the safety in theory, just like in practice.

Currently, potential approaches that can fit with Citrea and Bitcoin are:
- **PoS-like layer for sequencing blocks**: For users, the source of truth will always be zero-knowledge proofs in Bitcoin. The decentralized sequencing layer will reduce short-term trust in sequencers to protect ordering, as ordering will be finalized on the sequencing layer in a single slot.
- **Shared Sequencing**: We're following the state-of-art solutions emerging in other ecosystems. Once they're proven to be secure and battle-tested throughly, a good approach could be adapting them into Citrea.

In case you were wondering, based sequencing is not a good idea for Citrea, as it uses Bitcoin as a DA layer. Unfortunately, Bitcoin script is not expressive enough for this purpose.

-----

Upon mainnet launch, we will start diving deep into this topic and will work on it with full focus.