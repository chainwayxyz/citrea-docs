# Proof Generation



<figure><img src="../../.gitbook/assets/prover.png" alt=""><figcaption><p>Proof Generation in Citrea</p></figcaption></figure>

Citrea's proof generation mechanism is specialized for Bitcoin and BitVM. Citrea is running on top of recursion capable STARK based zkVM, RISC Zero, and has two different types of proofs:&#x20;

* **Batch Proof:** Batch proofs are produced for every few Bitcoin blocks. Citrea batch proof circuit is configured to scan Bitcoin blocks for batch root inscriptions via inclusion and soundness proofs, and if exists, it inputs the L2 batch that results in the batch root and proves the validity of the L2 batch. The proof outputs state difference resulted by the batch, initial and latest state roots, and the blockhash of Bitcoin block scanned. The proof with outputs is inscribed in Bitcoin.&#x20;
* **Light Client Proof:** Light client proofs recursively validate batch proofs and provides a single proof for full rollup history, allowing trustless and instant light clients. Light client proof circuit inputs previously generated light client proof, array of batch proofs with their inclusion and soundess proofs, and array of Bitcoin block headers corresponding to the batch proofs inscribed in. Circuit recursively verifies every single batch proof and the light client proof, asserts latest state root of the proof N-1 is equal to initial state root of the proof N. This logic ensures that so state transition is skipped, thus end result is the same state root with the actual rollup state root. \
  Light client proofs can be generated at any time by recursively verifying the previous light client proof with the batch proofs. Light client proofs are broadcasted in peer-to-peer network and also inscribed in Bitcoin.\
  Light nodes can listen the peer-to-peer network or only track the several latest Bitcoin headers, and the latest proof they find verifies the full rollup history and provides a trustless access to the state root.

Citrea circuit mainly proves two different logic; execution and blockspace. During the batch proof generation, both execution and blockspace are proved. During the light client proof generation, blockspace and verification of batch proofs are proved.

* **Execution proving:** Citrea runs the state transition function of the rollup, which is slightly broader logic that includes the EVM, inside the zk circuit. The circuit inputs the pre-state of the rollup, new batch of blocks, and outputs the state difference between the pre and post state after applying the batch.
* **Blockspace proving:** Blockspace proving is a brand new concept used in Citrea. Blockspace proving logic is a custom zk circuit that scans a Bitcoin block, extracts the state roots or Citrea batch proofs and forced transactions from it. In batch proofs it asserts the state roots' accuracy and in light client proofs it verifies the batch proofs if exists. Bitcoin block scanning inside the circuit is being done by the inclusion and soundness proof given, and checked against the corresponding Bitcoin block header.

Citrea merges execution proving and blockspace proving inside a single circuit for batch proofs. Individual batch proofs are only helpful for full nodes. In order to run a light node on batch proof only system, the light node must check every Bitcoin block one by one for proofs, which is not a light client anymore because of its bandwidth and storage needs. \
In Citrea, thanks to its light client proofs which apply recursion over batch proofs and blockspace proofs, users have trustless light clients. Anyone with the several latest Bitcoin block or connection to the peer-to-peer network can extract the Citrea light client proof and be fully sure that it represent the only valid fork of the chain and validate every state transition since Citrea's genesis block.
