# Execution Environment

The Citrea VM is a fully Ethereum Virtual Machine (EVM) equivalent VM running on Bitcoin and using $cBTC as its native token. EVMs as a concept are the most battle-tested and mature VM in the cryptocurrency ecosystem. It is a deterministic, stack-based virtual machine, renowned for its ability to efficiently execute smart contracts in a secure and isolated environment.

Citrea implements its own EVM, referred to as a "zkEVM". A zkEVM is a special EVM implementation that makes the full VM implementation provable. Citrea zkEVM is classified as Type 2, which means full equivalency and a scalable and trustless proof system due to being based on zk-STARKs. Citrea zkEVM is built using RISC Zero.

For further compatibility, Citrea is implemented intentionally to make multiple VMs interoperable.
