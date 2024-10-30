# Multi Prover Implementation

<!-- TODO: Link Clementine -->

ZK-proofs are the backbone of Citrea. They are used to prove the correctness of the state transitions and the validity of the transactions by the full nodes via batch proofs. Along with that, they are used to generate Light Client Proofs, which is used in our BitVM-based bridge design Clementine.

A common problem with ZK-proofs is that, even though they're super efficient and easy to verify, they are very hard to generate. Citrea uses Risc0 for proving, and at the moment our prover is efficient enough to make the system continue. However, we can speed up the proving process a lot as the state-of-art continues to evolve rapidly. Day by day new algorithms and techniques are being developed, and several companies have made enormous progress both in terms of software and hardware. We believe this acceleration will continue even faster in the future, and we can easily adapt them into Citrea since our architecture is designed with this in mind.

While these improvements are going on, another feasible yet much more efficient idea is having a multi-prover system. The goal is to make the proof generation process faster and resistant to the proof system & circuit bugs by distributing it across multiple independent provers. There are a couple of ways to achieve this theoretically:

<!-- TODO: Revisit here. -->
<!-- Discuss: SGX, multiple zkEVM, multiple zkVM, horizontal scaling, vertical scaling, prover coordination, impacts, dependencies -->