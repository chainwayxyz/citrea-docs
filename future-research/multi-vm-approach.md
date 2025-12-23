# Multi VM Approach

Citrea is designed to be compatible and interoperable with multiple virtual machines. It runs inside a generic STARK zkVM, meaning any virtual machine can be implemented and will produce execution proofs. Citrea currently implements Ethereum Virtual Machine (EVM).

Implementing different VMs like SVM or integrating WASM-based smart contracts to Citrea is also possible thanks to its future-proof design. The objective in this approach is to build an environment where contracts can be written in multiple languages like Solidity (EVM) or Rust, C++ (WASM) and run & interact on the same blockchain, natively.

Two main methods that fit into this approach are:

* Integrating Arbitrum Stylus-like\* solutions to Citrea's EVM module: Essentially bringing a WASM execution model into the system.
* Building a Wasm-based sovereign module from scratch: A dedicated WASM execution environment needs to be built and integrated with custom precompiles.

***

The concept is quite experimental and it will be further researched after the mainnet launch.

* Arbitrum Stylus is under the BUSL License, and it is currently allowed for specific use cases in the Arbitrum Ecosystem. The term is used here to describe a similar approach, not the exact implementation.
