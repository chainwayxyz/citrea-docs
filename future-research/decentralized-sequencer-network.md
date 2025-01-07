# Decentralized Sequencer Network

<!-- TODO: Link sequencer.md in this text -->
<!-- TODO: Include force_transaction.md in this text -->
<!-- TODO: Possibly link prover.md -->

Currently, most rollup solutions, including Citrea, rely on a single sequencer. Even though this approach provides sufficient security guarantees in terms of safety and non-custodial operation, it presents a single point of failure concerning liveness. While censorship resistance can be achieved by force transactions, for liveness, a decentralized sequencer network is one of the ideal solutions.

A decentralized sequencer network essentially consists of multiple sequencers, where each sequencer is responsible for block production and transaction ordering based on the algorithm of network. 

Several important considerations regarding the decentralized sequencer networks are:

- **Architecture**: Deciding the sequencer set, making it permissionless or not, modifying the architecture, determining the best consensus (CometBFT, Hotstuff, MonadBFT), and making further adjustments are all crucial properties, each with tradeoffs.
- **Latency**: Achieving consensus among multiple sequencers is a challenging task, it brings some latency to the system and it should not impact responsiveness of the rollup hugely.
- **Economic Incentives**: There are interesting ideas stemming from an economic perspective and a game theory perspective on the decentralized networks. It's important to ensure the safety in theory, just like in practice.

Currently, potential approaches that can fit with Citrea and Bitcoin are:

- **PoS-like layer for sequencing blocks**: For users, the source of truth will always be zero-knowledge proofs in Bitcoin. The decentralized sequencing layer will reduce short-term trust in sequencers to protect ordering, as ordering will be finalized on the sequencing layer in a single slot.
- **Shared Sequencing**: We're following the state-of-art solutions emerging in other ecosystems. Once they're proven to be secure and battle-tested throughly, a good approach could be adapting them into Citrea.

One thing is not mentioned above - based sequencing. As Citrea uses Bitcoin for the DA layer, it is not possible to implement based sequencing with limited capabilities of Bitcoin script.

-----

The idea is on the roadmap and active development & implementation will kick off after the mainnet launch.
