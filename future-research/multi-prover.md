# Multi Prover

Citrea uses ZK proofs prove the validity of the state transitions. Full nodes verify the batch proofs produced by the prover. These batch proofs are also used to generate Light Client Proofs for the BitVM-based bridge Clementine.

An idea to make the proof generation process more resilient is to implement a multi-prover system. This approach increases the security as it reduces the chance of a single point of failure algorithm-wise in case there's a vulnerability in a proof system (e.g. soundness).

Currently Citrea has a template multi-prover system implemented using [RISC Zero](https://risczero.com) and [SP1](https://docs.succinct.xyz/docs/introduction). You can check the initial guest code implementation from [here](https://github.com/chainwayxyz/citrea/tree/nightly/guests).

As of Citrea Testnet, SP1 prover is not active, and we are only utilizing RISC Zero. After the mainnet launch we are planning to activate SP1 prover with additional prover implementations for multi-prover system, with a possible SGX-extension.

***
