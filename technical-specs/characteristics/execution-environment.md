# Execution Environment

The Citrea VM is a fully Ethereum Virtual Machine (EVM) equivalent VM running on Bitcoin and using $cBTC as its native token. EVMs as a concept are the most battle-tested and mature VM in the cryptocurrency ecosystem. It is a deterministic, stack-based virtual machine, renowned for its ability to efficiently execute smart contracts in a secure and isolated environment.

Citrea implements its own EVM, referred to as a "zkEVM". A zkEVM is a special EVM implementation that makes the full VM implementation provable. Citrea zkEVM is classified as Type 2, which means full equivalency and a scalable and trustless proof system due to being based on zk-STARKs. Developers can utilize Solidity, Vyper, and other EVM-compatible languages to develop their smart contracts without any changes. 

Citrea zkEVM is built using [RISC Zero](https://www.risczero.com/).

### Differences from Ethereum

- Block times: Citrea as a block time of 2 seconds, compared to Ethereum's 12 seconds. 

- Custom Block Gas Limit: Citrea has an 8 million gas limit per block, compared to Ethereum's 30 million. While individual blocks provide a lower block gas limit, given Citrea has a 2 second block time, in a total of 12 seconds Citrea provides more gas throughput than Ethereum (48 million vs 30 million). 

<!-- TODO: Improve Fee Calculation Info, perhaps provide more explanation & check correctness -->
- Custom Fee Calculation: In rollups, the fee calculation typically covers two major points: Rollup (i.e. "L2") execution costs and the DA cost (+ proof generation costs for ZK rollups). Citrea has a custom fee calculation mechanism that covers both of these points. However, this mechanism comes with its own challenges as Bitcoin's fee mechanism is entirely different from Ethereum's.  In Bitcoin, fees can rise or fall very dramatically in just 1 - 2 blocks. Along with that, if the state diff size is large and sats/vByte is high too, the fees can be even higher. To prevent such volatility and to keep the rollup fees reasonable, Citrea charges a statistically calculated dynamic `L1_FEE_OVERHEAD` along with networks' own fees based on mempool demands. `l1_fee_rate` field is also returned in the RPC requests to inform users as well. 

-----

For further compatibility, Citrea is initially intentionally designed to make multiple VMs interoperable. You can find more information about this concept [here](../../future-research/multi-vm-approach.md).