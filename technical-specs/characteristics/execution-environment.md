# Execution Environment

Citrea VM is a fully Ethereum Virtual Machine (EVM) equivalent VM running on Bitcoin and using $cBTC as its native token. EVM is the most battle-tested and matured VM. It is a deterministic stack-based virtual machine, renowned for its ability to efficiently execute smart contracts in a secure and isolated environment.

Citrea implements its own EVM, spesifically zkEVM. zkEVM is a special EVM implementation that makes the full VM implementation provable. Citrea zkEVM is classified as Type 2, which means full equivalency, and based on ZK-STARKs, a scalable and trustless proof system. Citrea zkEVM built using RISC Zero.

For further compatability, Citrea is implemented in a way to support multiple VMs interoperable.
