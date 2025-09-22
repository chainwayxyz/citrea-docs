# System Contracts

#### Citrea Specific System Contracts

| Address                                      | Name & Link                                                                                                                   |
| -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `0x3100000000000000000000000000000000000001` | [Bitcoin Light Client Proxy](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000001)          |
| `0x3100000000000000000000000000000000000002` | [Bridge Proxy](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000002)                        |
| `0x3100000000000000000000000000000000000003` | [Base Fee Vault Proxy](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000003)                |
| `0x3100000000000000000000000000000000000004` | [L1 Fee Vault Proxy](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000004)                  |
| `0x3100000000000000000000000000000000000005` | [Priority Fee Vault Proxy](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000005)            |
| `0x3100000000000000000000000000000000000006` | [WCBTC](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000006)                               |
| `0x3200000000000000000000000000000000000001` | [Bitcoin Light Client Implementation](https://explorer.testnet.citrea.xyz/address/0x3200000000000000000000000000000000000001) |
| `0x3200000000000000000000000000000000000002` | [Bridge Implementation](https://explorer.testnet.citrea.xyz/address/0x3200000000000000000000000000000000000002)               |
| `0x3200000000000000000000000000000000000003` | [Base Fee Vault Implementation](https://explorer.testnet.citrea.xyz/address/0x3200000000000000000000000000000000000003)       |
| `0x3200000000000000000000000000000000000004` | [L1 Fee Vault Implementation](https://explorer.testnet.citrea.xyz/address/0x3200000000000000000000000000000000000004)         |
| `0x3200000000000000000000000000000000000005` | [Priority Fee Vault Implementation](https://explorer.testnet.citrea.xyz/address/0x3200000000000000000000000000000000000005)   |
| `0x31ffffffffffffffffffffffffffffffffffffff` | [Proxy Admin](https://explorer.testnet.citrea.xyz/address/0x31ffffffffffffffffffffffffffffffffffffff)                         |



System contracts are a set of pre-deployed smart contracts responsible for the core functionalities of the Citrea. These contracts are embedded in the genesis state. They contain functions that can be only processed through a system transaction, in addition to regular functions that can be called by any address.

## System Transactions

System transactions are transactions that are done by the system caller, which is an EVM address without a corresponding known private key, and is hardcoded in the system contracts and at the node level. System caller address is `0xdeaDDeADDEaDdeaDdEAddEADDEAdDeadDEADDEaD` and it is chosen as it is a non-random address. To illustrate how hard it is to find the corresponding private key for the system caller address, it is worth mentioning that the probability of finding a private key for a given address is 1 in 2^160. If all the Bitcoin miners stopped what they are doing and started searching for the private key of this address, it would take 80 quintillion years (8 \* 10^19 years) to find it. Our universe is only 13.8 billion years old, so that is like the age of the universe squared, that's how hard this is.

In contrast to regular transactions, these are prepared and executed in the node level, and they are included at the beginning of Citrea blocks.

Gas consumption of these transactions do not affect any fees as opposed to regular transactions, but it is accounted into the block gas limit calculations.

It must be noted that some of the transactions that can be processed as system transactions, such as `deposit` which handles the bridging of BTC from Bitcoin to Citrea, can be also processed as a regular transaction.

These transactions are inherently trustless because they are programmed directly into the node, ensuring their inclusion in blocks.
