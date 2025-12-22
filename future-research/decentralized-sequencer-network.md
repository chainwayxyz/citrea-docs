# Decentralized Sequencer Network

A decentralized sequencer network is composed of multiple sequencers, each tasked with block production and transaction ordering according to the network’s algorithm. Citrea is a ZK Rollup that currently relies on a single sequencer, which provides strong security and non-custodial operation. However, it also introduces a single point of failure for liveness. Although censorship resistance can be achieved through force transactions, a decentralized sequencer network is ideal for addressing the liveness challenge by distributing responsibilities among multiple participants.

A few key challenges of decentralized sequencer networks are as follows:

* **Architecture**: Deciding the sequencer set (permissionless or not), modifying the architecture, determining the optimal consensus mechanism (e.g., CometBFT, Hotstuff, MonadBFT).
* **Latency**: Achieving consensus among multiple sequencers poses significant challenges due to the latency introduced by communication between sequencers. Ideally, a decentralized sequencer network should not introduce observable latency compared to the current implementation.
* **Economic Incentives**: The economic model should be designed to ensure that sequencers are rewarded for their work and penalized for misbehavior, for the network's security and liveness.

***

The concept is on the roadmap and active development & implementation will kick off after the mainnet launch.
