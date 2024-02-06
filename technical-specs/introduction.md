# Introduction

Bitcoin is designed to facilitate trustless payments in a decentralized network of participants. Over the years, Bitcoin blockspace has become more and more valuable because of its high success in achieving decentralization and security. Citrea uses Bitcoin blockspace to secure its execution, introducing a novel way to use it efficiently via zero-knowledge proof technology.

Citrea uses a small subset of Bitcoin blockspace and divides the same blockspace among magnitude of transactions than can be done on the layer 1 itself. By rolling up a batch of transactions together and computing the result using the most advanced verifiable computing technique - zero-knowledge proofs - Citrea provides Bitcoin-equivalent security to a vast array of applications; including but not limited to more complex financial primitives.

Citrea inherits full Bitcoin L1 security, including all four properties needed for its security:

* **Validity:** Each Citrea batch is processed inside the zero-knowledge VM, resulting in a succinct zero-knowledge proof that validates all the inputs, computation, and outputs. The proof is inscribed into Bitcoin, making it available for every single participant in the network, including light nodes (SPVs) and Bitcoin script itself, through BitVM.
* **Data Availability:** Each Citrea proof outputs the state difference of the batch. The proofs with outputs are inscribed in Bitcoin. By scanning over proofs in Bitcoin blocks, anyone can build the latest state tree of the rollup, ensuring the relevant data to transact is always available on Bitcoin.
* **Censorship Resistance:** Citrea utilizes inscription-like [forced transaction](security-properties/censorship-resistance-and-force-transactions/) envelopes on the Bitcoin main chain. Anyone can submit a Citrea transaction to Bitcoin within a specified encoding. Citrea's circuit ensures that every single forced transaction is included in the next batch via inclusion and soundness proofs over the corresponding Bitcoin block they are included within.
* **Re-org Resistance:** Citrea includes the most recent finalized Bitcoin block hash into the rollup block header. After the corresponding proof for the rollup block is sent to Bitcoin and finalized, it cannot be re-orged without re-orging the Bitcoin blockchain itself, something that is almost impossible after 6 blocks of confirmation.
