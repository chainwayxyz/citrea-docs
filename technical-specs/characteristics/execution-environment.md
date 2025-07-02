# Execution Environment

The Citrea VM is a fully Ethereum Virtual Machine (EVM) equivalent VM running on Bitcoin and using $cBTC as its native token. EVMs as a concept are the most battle-tested and mature VM in the cryptocurrency ecosystem. It is a deterministic, stack-based virtual machine, renowned for its ability to efficiently execute smart contracts in a secure and isolated environment.

Citrea implements its own EVM, referred to as a "zkEVM". A zkEVM is a special EVM implementation that makes the full VM implementation provable. Citrea zkEVM is classified as Type 2, which means full equivalency and a scalable and trustless proof system due to being based on zk-STARKs. Citrea zkEVM is built using RISC Zero.

#### Different characteristics of Citrea

Being a Type 2 zkEVM, Citrea has some different characteristics compared to Ethereum and other EVM blockchains:

- **Native currency is Bitcoin**: Citrea uses $cBTC for gas fees and transfers in the network, which is the trust-minimized 1-1 backed Bitcoin asset. It is **not** an ERC-20 - it is the native asset with 18 decimals (not 8!).
- **Block time & gas limit**: Citrea has a block time of **2 seconds** with a block gas limit of **10 million**.
- **L1 Fee**: As Citrea posts state diffs to Bitcoin, there is a fundamental DA cost - this reflected as **L1 Fee** to the users. L1 Fee is deducted from the address balance at the end of a transaction, and it is calculated with the formula of `l1FeeRate * diffSize`.
    - **L1 Fee Rate**: Wei per Byte cost of the state diff. 
    - **Diff Size**: The size of the state diff in bytes. 
- **EVM Version & Tweaks**: Citrea supports **Pectra** version of EVM with some tweaks:
    - **EIP-4844**: EIP-4844 is not supported on Citrea, along with KZG precompile. If the address `0x0A` is called it will return as an EOA address.
    - **EIP-2935**: EIP-2935 is not supported on Citrea, which means that `BLOCKHASH` still returns the latest 256 blockhashes (instead of 8192).
- **Additional Precompiles**: Compared to Pectra version, Citrea offers two additional precompiles: **secp256r1** and **Schnorr**. You can read more about them [here](https://docs.citrea.xyz/developer-documentation/schnorr-secp256r1).
- **Merkle Tree**: Citrea uses Jellyfish Merkle Tree for its state instead of Patricia Merkle Tree.
- **System Smart Contracts**: There are special system smart contracts of Citrea that is optimized for system level operations: `BitcoinLightClient`, `Bridge`, and `FeeVault`. You can read more about them [here](https://docs.citrea.xyz/developer-documentation/system-contracts).