# BitcoinLightClient

Serves as a smart contract based light client for Bitcoin. For each new Bitcoin block, the block hash and the witness root is written into this contract. This is done with a system transaction executed in the beginning of the first Citrea block that corresponds to the new Bitcoin block.

## State Structure

```solidity
uint256 public blockNumber;
```
Next block height to store block hash and witness root information for.

---

```solidity
mapping(uint256 => bytes32) public blockHashes;
```
A `block height: block hash` mapping. It stores the block hash of the block for a given block height in Bitcoin.

---

```solidity
mapping(bytes32 => bytes32) public witnessRoots;
```
A `block hash: witness root` mapping. It stores the witness root of the block corresponding to a given block hash in Bitcoin.

## Access Control Structure

```solidity
modifier onlySystem() 
```
This contract has only one access control modifier, and it is used to check the caller of a function against the hardcoded system caller address which is `0xdeaDDeADDEaDdeaDdEAddEADDEAdDeadDEADDEaD`.

## Functions

<details>
<summary>initializeBlockNumber()</summary>

```solidity
function initializeBlockNumber(uint256 _blockNumber) external onlySystem
```
Gets triggered in the first ever Citrea block, and it sets the block height of the Bitcoin block that corresponds to the first Citrea block. This function is called only once during the lifetime of the contract and it is called by the system caller.

| Parameters    | Description |
|-----------|-------------|
| `uint256 _blockNumber`   | Bitcoin block height corresponding to the first Citrea block  |

</details>

---

<details>
<summary>setBlockInfo()</summary>

```solidity
function setBlockInfo(bytes32 _blockHash, bytes32 _witnessRoot) external onlySystem
```
Called by the system caller and it sets the block hash and witness root of the next Bitcoin block denoted by the `blockNumber`. It also increments the `blockNumber` by one.

| Parameters    | Description |
|-----------|-------------|
| `bytes32 _blockHash`   | Block hash of the current Bitcoin block |
| `bytes32 _witnessRoot`   | Witness root (merkle root of `wTXID`s) of the current Bitcoin block |

</details>

---

<details>
<summary>getBlockHash()</summary>

```solidity
function getBlockHash(uint256 _blockNumber) external view returns (bytes32)
```
Returns the block hash of the Bitcoin block corresponding to the given block height.

| Parameters    | Description |
|-----------|-------------|
| `uint256 _blockNumber`   | Block height of the queried block |

| Returns    | Description |
|-----------|-------------|
| `bytes32 blockHash` | Block hash of the queried block |

</details>

---

{% hint style="warning" %}
The following functions `getWitnessRootByHash` and `getWitnessRootByNumber` returning the zero value does **NOT** mean that there is no such a block recorded unlike the blockhash getters as it is possible for a valid witness root to be the zero value in the case of a Bitcoin block with one transaction. This happens as the `wTXId` of a coinbase transaction is the zero value and the merkle root is the leaf itself in the case of one leaf.
{% endhint %}


<details>
<summary>getWitnessRootByHash()</summary>

```solidity
function getWitnessRootByHash(bytes32 _blockHash) external view returns (bytes32)
```
Returns the witness root of the Bitcoin block corresponding to the given block hash.

| Parameters    | Description |
|-----------|-------------|
| `bytes32 _blockHash`   | Block hash of the queried block |

| Returns    | Description |
|-----------|-------------|
| `bytes32 witnessRoot` | Witness root (merkle root of `wTXID`s) of the queried block |

</details>

---

<details>
<summary>getWitnessRootByNumber()</summary>

```solidity
function getWitnessRootByNumber(uint256 _blockNumber) external view returns (bytes32)
```
Returns the witness root of the Bitcoin block corresponding to the given block height.

| Parameters    | Description |
|-----------|-------------|
| `uint256 _blockNumber`   | Block height of the queried block |

| Returns    | Description |
|-----------|-------------|
| `bytes32 witnessRoot` | Witness root (merkle root of `wTXID`s) of the queried block |

</details>

---

{% hint style="warning" %}
The following `verifyInclusion` functions will pass when zero value is passed with `_wtxId` as the zero value is a valid `wTXId` for a coinbase transaction and it exists in all Bitcoin blocks. Thus the integrators must make sure to not provide the zero `wTXId` as input accidentally as it may happen in cases like sending information from a deleted Solidity user record which will have the zero `bytes32` value as that is the default value for `bytes32` in Solidity.
{% endhint %}

<details>
<summary>verifyInclusion()</summary>

```solidity
function verifyInclusion(bytes32 _blockHash, bytes32 _wtxId, bytes calldata _proof, uint256 _index) external view returns (bool)
```
Verifies the inclusion of a Bitcoin transaction in a particular Bitcoin block specified by its block hash. Returns whether the transaction is included in the given block. It functions by constructing the Merkle root from the `wTXID` of the queried transaction and provided Merkle path as `_proof` and then comparing it with the witness root stored in this contract of the block specified by the passed block hash.

| Parameters    | Description |
|-----------|-------------|
| `bytes32 _blockHash`   | Block hash of the block the queried transaction should be in |
| `bytes32 _wtxId`   | `wTXID` of the queried transaction |
| `bytes _proof`   | Merkle inclusion proof of the transaction, this is a Merkle path used to construct to Merkle root |
| `uint256 _index`   | Leaf index of the transaction in the Merkle tree |

| Returns    | Description |
|-----------|-------------|
| `bool isIncluded` | Whether the transaction is in the block specified by the block hash |

---

```solidity
function verifyInclusion(uint256 _blockNumber, bytes32 _wtxId, bytes calldata _proof, uint256 _index) external view returns (bool)
```
Verifies the inclusion of a Bitcoin transaction in a particular Bitcoin block specified by its block height. Returns whether the transaction is included in the given block. It functions by constructing the Merkle root from the `wTXID` of the queried transaction and provided Merkle path as `_proof` and then comparing it with the witness root stored in this contract of the block specified by the passed block height.

| Parameters    | Description |
|-----------|-------------|
| `uint256 _blockNumber`   | Block height of the block the queried transaction should be in |
| `bytes32 _wtxId`   | `wTXID` of the queried transaction |
| `bytes _proof`   | Merkle inclusion proof of the transaction, this is a Merkle path used to construct to Merkle root |
| `uint256 _index`   | Leaf index of the transaction in the Merkle tree |

| Returns    | Description |
|-----------|-------------|
| `bool isIncluded` | Whether the transaction is in the block specified by the block height |

</details>