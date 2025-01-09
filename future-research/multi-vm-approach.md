# Multi VM Approach

Citrea is designed to be compatible and interoperable with multiple virtual machines. It runs inside a generic STARK zkVM, meaning any virtual machine can be implemented and will produce execution proof. Currently on the path to mainnet, Citrea implements EVM and it will be launched that way. 

Implementing different VMs like SVM or integrating WASM-based smart contracts to Citrea is also possible thanks to its future-proof design. The objective in this approach is to build an environment where contracts can be written in multiple languages like Solidity (EVM) or Rust, C++ (WASM) and run & interact on the same blockchain, natively. 

Two main methods that fits into this approach are:
- Integrating Arbitrum Stylus-like* solutions to Citrea's EVM module: Essentially bringing a WASM execution model into the system.
- Building a Wasm-based sovereign module from scratch: A dedicated WASM execution environment needs to be built and integrated with custom precompiles.

-----

The idea is quite experimental and it will be further researched after the mainnet launch.

* Arbitrum Stylus is under the BUSL License, and it is currently allowed for specific use cases in the Arbitrum Ecosystem. As cypherpunks writing code, we deeply value their work and respect their license. This will also continue with honesty & transparency when we start to implement the ideas in this document.