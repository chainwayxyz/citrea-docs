# Introduction

Bitcoin is designed to faciliate trustless payments in a decentralized network of participants. Over the years, Bitcoin blockspace became more and more valuable because of its high success in achieving decentralization and security together. Citrea uses Bitcoin blockspace to secure its execution, introducing a novel way to use it efficiently via zero-knowledge technology.&#x20;

Citrea uses a small subset of Bitcoin blockspace and divides the same blockspace among magnitude of transactions that can be done on the Layer 1 itself. By rolling up batch of transaction together and computing the result using the most advanced verifiable computing technique, zero-knowledge proofs, Citrea provides Bitcoin equivalent security to build vast array of applications including but not limited to more complex financial primitives.

Citrea inherits full Bitcoin L1 security. Citrea inherits all four properties needed for blockchain security from Bitcoin;

* **Validity:** Each Citrea batch is processed inside the zero-knowledge VM, resulting in succinct zero-knowledge proof that validates all the inputs, computation, and outputs. The proof is inscribed into Bitcoin, making it available for every single participant in the network, including light nodes (SPVs) and Bitcoin script itself, through BitVM.&#x20;
* **Data Availability:** Each Citrea proof outputs the state difference of the batch. The proofs with outputs are inscribed in Bitcoin. By scanning over proofs in Bitcoin blocks, anyone can build the latest state tree of the rollup, ensuring the relevant data to transact is always available on Bitcoin.&#x20;
* **Censorship Resistance:** Citrea utilizes inscription-like forced transaction envelopes on Bitcoin main chain. Anyone can submit a Citrea transaction to Bitcoin within a specific encoding. Citrea circuit ensures that every single forced transaction is included in the next batch via inclusion and soundness proofs over the corresponding Bitcoin block they are included.
* **Re-org Resistance:** Citrea includes most recent finalized Bitcoin block hash into the rollup block header. After the corresponding proof for the rollup block sent to Bitcoin and finalized, it cannot be re-orged without re-orging Bitcoin blockchain, which is almost impossible after 6 blocks of confirmation.
