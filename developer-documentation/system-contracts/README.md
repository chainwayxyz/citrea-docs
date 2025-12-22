# System Contracts

System contracts are a set of pre-deployed smart contracts on Citrea that are responsible for the core functionalities. These contracts are embedded in the genesis state. They contain functions that can be only processed through a system transaction, in addition to regular functions that can be called by any address.

{% hint style="success" %}
System contracts are also audited under Citrea and Clementine - you can find the reports at [audits-inquiries](https://docs.citrea.xyz/security/audits-inquiries "mention") section.
{% endhint %}

| Address                                      | Name & Explorer Link                                                                                                          |
| -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `0x3100000000000000000000000000000000000001` | [Bitcoin Light Client Proxy](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000001)          |
| `0x3100000000000000000000000000000000000002` | [Bridge Proxy](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000002)                        |
| `0x3100000000000000000000000000000000000003` | [Base Fee Vault Proxy](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000003)                |
| `0x3100000000000000000000000000000000000004` | [L1 Fee Vault Proxy](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000004)                  |
| `0x3100000000000000000000000000000000000005` | [Priority Fee Vault Proxy](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000005)            |
| `0x3100000000000000000000000000000000000006` | [WCBTC](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000006)                               |
| `0x3100000000000000000000000000000000000007` | [Failed Deposit Vault Proxy](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000007)          |
| `0x3200000000000000000000000000000000000001` | [Bitcoin Light Client Implementation](https://explorer.testnet.citrea.xyz/address/0x3200000000000000000000000000000000000001) |
| `0x3200000000000000000000000000000000000002` | [Bridge Implementation](https://explorer.testnet.citrea.xyz/address/0x3200000000000000000000000000000000000002)               |
| `0x3200000000000000000000000000000000000003` | [Base Fee Vault Implementation](https://explorer.testnet.citrea.xyz/address/0x3200000000000000000000000000000000000003)       |
| `0x3200000000000000000000000000000000000004` | [L1 Fee Vault Implementation](https://explorer.testnet.citrea.xyz/address/0x3200000000000000000000000000000000000004)         |
| `0x3200000000000000000000000000000000000005` | [Priority Fee Vault Implementation](https://explorer.testnet.citrea.xyz/address/0x3200000000000000000000000000000000000005)   |
| `0x3200000000000000000000000000000000000007` | [Failed Deposit Vault Implementation](https://explorer.testnet.citrea.xyz/address/0x3200000000000000000000000000000000000007) |
| `0x31ffffffffffffffffffffffffffffffffffffff` | [Proxy Admin](https://explorer.testnet.citrea.xyz/address/0x31ffffffffffffffffffffffffffffffffffffff)                         |

In this section we're going over three main system smart contracts of Citrea:&#x20;

* [bitcoin-light-client](https://docs.citrea.xyz/developer-documentation/system-contracts/bitcoin-light-client "mention")
* [bridge](https://docs.citrea.xyz/developer-documentation/system-contracts/bridge "mention")
* [fee-vaults](https://docs.citrea.xyz/developer-documentation/system-contracts/fee-vaults "mention")

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2FXOvqLtMbo8dA08qyO49X%2Fimage.png?alt=media&#x26;token=33de669b-1b70-4599-84af-36589ca28921" alt=""><figcaption></figcaption></figure>

### System Transactions

System transactions originate from a hardcoded address with no private key:

```solidity
address constant SYSTEM_CALLER = 0xdeaDDeADDEaDdeaDdEAddEADDEAdDeadDEADDEaD;
```

Finding the corresponding private key is a 1-in-2^160 problem, which is practically impossible within the lifetime of the universe.&#x20;

The sequencer prepares the calls and injects them at the start of every block. They spend no balance from the sender, but their gas consumption still counts toward the block gas limit (which is *10 million*).

Typical system transactions include the one-time bootstraps

```solidity
lightClient.initializeBlockNumber(uint256 height);
bridge.initialize(bytes prefix, bytes suffix, uint256 depositAmount);
```

as well as the per-Bitcoin-block maintenance call

```solidity
lightClient.setBlockInfo(bytes32 blockHash, bytes32 witnessRoot, uint256 coinbaseDepth);
```

and the bridge’s deposit handler:

```solidity
bridge.deposit(Transaction moveTx, MerkleProof proof, bytes32 shaScriptPubkeys);
```

Because their inclusion is enforced, these transactions are deterministic and cannot be censored by external actors even though no private key signs them.
