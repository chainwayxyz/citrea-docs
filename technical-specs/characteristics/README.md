---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Characteristics

Citrea possesses the following characteristics, in addition to TL;DR page, very shortly:

#### Execution and Computation:

* **ZK-rollup**: Citrea is a ZK-rollup. State transitions are verified using zkSNARKs.
* **Type 2 zkEVM**: Citrea is a fully EVM-compatible, with only minor changes to block structure and tree/data storages that does not touch the application layer. Thus, it is a Type 2 zkEVM.
* **Citrea runs a zkVM**: Citrea uses Risc Zero guest code for proof generation at the moment. It's also modular in the architecture, meaning that comes with easy modify & change with respect to the further developments in the area of zkVMs.
* **cBTC**: Citrea uses Bitcoin as its native token. To avoid confusion and improve on/off-ramp UX; Citrea native $BTC is referred to as $cBTC in Citrea.

#### Block Production:

* **Sequencer**: Citrea has a sequencer that is responsible for ordering transactions, constructing rollup blocks and giving soft-confirmations to full blocks to achieve soft finality.
* **Soft Confirmations**: Citrea uses soft-confirmations to give a soft finality status to transactions and blocks before they are being finalized on the Bitcoin DA.
* **Sequencer Commitments**: In addition to soft confirmations, Citrea sequencer also inscribes sequencer commitments to prevent any reordering attacks.
* **Forced Transactions**: Users can directly inscribe their transactions into the Bitcoin to overcome any censorship resistance and enabling the force inclusion of specific transactions. This property is currently being implemented for the mainnet phase.

#### Data Availability:

* **Bitcoin as Data Availability**: Citrea uses Bitcoin as the only source of truth for the system. There are no intermediary chains or committees used to store Citrea state. All the data is written to Bitcoin in a state difference format along with zk-proofs. Full nodes can read the data from directly from Bitcoin to reconstruct and verify the state, without needing to trust any other source. 
* **State Differences**: To minimize the on-chain data storage, Citrea includes compressed state differences in the batch proofs that are published. These state differences reflect the state changes between the start and end blocks of the rollup batch, multiple blocks. It is a much more efficient type of data storage that allows a rollup to maintain its security properties. Also, it's the only technically possible way to store rollup data on Bitcoin as transaction data exceeds the 4 MB limit of Bitcoin blocks in many occasions.

#### Prover & Zero Knowledge Proofs:

* **Prover**: Citrea has a prover that is responsible for generating zero-knowledge proofs for the state transitions in batches. These batch proofs are also then published to Bitcoin directly. 
* **Light Client Proof**: Prover also enables the generation of Light Client Proofs for the Clementine bridge, and thereby enabling the trust-minimized two-way peg with a challenge - response mechanism.

#### Trust-Minimized Bitcoin <> Citrea Bridge:

* **Clementine**: Clementine is the trust-minimized bridge to Bitcoin that is being developed by Citrea team. It enables the two-way peg between Citrea and Bitcoin. Citrea proofs are optimistically verified in Bitcoin, enabling the first universal trust-minimized bridge. On testnet, at the moment, Clementine is working without fraud-proofs. Upon completion of open-source BitVM implementation, fraud proofs will also be enabled on Clementine and it will be ready for the mainnet.

-----

In the following pages of this documentation you can find more detailed explanations of the characteristics mentioned above.