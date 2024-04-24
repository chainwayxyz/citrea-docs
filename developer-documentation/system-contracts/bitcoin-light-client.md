// TODO: Add tables for params and returns
# BitcoinLightClient

Serves as a smart contract based light client for Bitcoin. For each new Bitcoin block, the block hash and the witness root is written into this contract. This is done with a system transaction executed in the beginning of the first Citrea block that corresponds to the new Bitcoin block.

## State Structure

```solidity
uint256 public blockNumber;
```
Next block height to store block hash and witness root information for.

```solidity
mapping(uint256 => bytes32) public blockHashes;
```
A `block height: block hash` mapping. It stores the block hash of the block for a given block height in Bitcoin.

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

```solidity
function initializeBlockNumber(uint256 _blockNumber) external onlySystem
```
Gets triggered in the first ever Citrea block, and it sets the block height of the Bitcoin block that corresponds to the first Citrea block. This function is called only once during the lifetime of the contract and it is called by the system caller.

```solidity
function setBlockInfo(bytes32 _blockHash, bytes32 _witnessRoot) external onlySystem
```
Called by the system caller and it sets the block hash and witness root of the next Bitcoin block denoted by the `blockNumber`. It also increments the `blockNumber` by one.

```solidity
function getBlockHash(uint256 _blockNumber) external view returns (bytes32)
```
Returns the block hash of the Bitcoin block corresponding to the given block height.

---

{% hint style="warning" %}
The following functions `getWitnessRootByHash` and `getWitnessRootByNumber` returning the zero value does **NOT** mean that there is no such a block recorded unlike the blockhash getters as it is possible for a valid witness root to be the zero value in the case of a Bitcoin block with one transaction. This happens as the `wTXId` of a coinbase transaction is the zero value and the merkle root is the leaf itself in the case of one leaf.
{% endhint %}

```solidity
function getWitnessRootByHash(bytes32 _blockHash) external view returns (bytes32)
```
Returns the witness root of the Bitcoin block corresponding to the given block hash.

```solidity
function getWitnessRootByNumber(uint256 _blockNumber) external view returns (bytes32)
```
Returns the witness root of the Bitcoin block corresponding to the given block height.

---

```solidity
function verifyInclusion(bytes32 _blockHash, bytes32 _wtxId, bytes calldata _proof, uint256 _index) external view returns (bool)
```

```solidity
function verifyInclusion(uint256 _blockNumber, bytes32 _wtxId, bytes calldata _proof, uint256 _index) external view returns (bool)
```