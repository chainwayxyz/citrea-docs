# Multi Prover Implementation

<!-- TODO: Link Clementine -->

ZK-proofs are the backbone of Citrea. They are used to prove the correctness of the state transitions and the validity of the transactions by the full nodes via batch proofs. Along with that, they are used to generate Light Client Proofs, which is used in our BitVM-based bridge design Clementine.

A common problem with ZK-proofs is that, even though they're super efficient and easy to verify, they are very hard to generate. Citrea uses Risc0 for proving, and at the moment the prover is efficient enough to make the system continue. However, speeding the proving process up more feasible than ever, as the state-of-art research & implementations continue to evolve rapidly. 

Every day, new algorithms and techniques are being developed, and several companies have made enormous progress both in terms of software and hardware. This acceleration trend will continue even faster in the future, and as Citrea architecture is designed with this in mind, it will be able to adapt to the future quickly.

While these improvements are going on, another feasible yet much more efficient idea is having a multi-prover system. The goal is to make the proof generation process faster and resistant to the proof system & circuit bugs by distributing it across multiple independent provers. There are a couple of ways to achieve this (theoretically):

- Generating multiple types of proofs: 
    - Using this approach increases the security as it reduces the chance of a single point of failure algorithm-wise in case there's a vulnerability in a proof system.
    - Can be accompanied by SGX prover implementations.

- Running multiple provers on a distributed network where each prover is responsible for a different state interval to reduce the proof generation latency
    - This is also a reasonable approach but implementation phase is not easy.

- Implementing a parallel proving mechanism
    - A much more complex approach.

First two approaches are the common ones, and arguably they're easier to implement compared to [decentralized sequencer networks](decentralized-sequencer-network.md). 

---

Multi-prover system is on the roadmap and active development & implementation will kick off after the mainnet launch.

<!-- TODO: Revisit here. -->
<!-- Discuss: SGX, multiple zkEVM, multiple zkVM, horizontal scaling, vertical scaling, prover coordination, impacts, dependencies -->