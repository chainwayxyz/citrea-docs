# Bitcoin Light Client

Citrea’s on-chain light client for Bitcoin. Each finalized Bitcoin block is pushed into this contract via a system transaction so that other contracts (most importantly the bridge) can prove transactions of Bitcoin. For each new Bitcoin block, the block hash and the witness root are written.

```solidity
address constant BITCOIN_LIGHT_CLIENT = 0x3100000000000000000000000000000000000001;
```

That proxy address `0x3100…0001` hosts the client on testnet: [explorer link](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000001).

### Important to Know

```solidity
address constant SYSTEM_CALLER = 0xdeaDDeADDEaDdeaDdEAddEADDEAdDeadDEADDEaD;
```

The system caller is a hardcoded address with no private key. It is used to execute system transactions that mutate the state of the contract.

```solidity
event BlockInfoAdded(uint256 height, bytes32 blockHash, bytes32 witnessRoot, uint256 coinbaseDepth);
```

Only the system caller can mutate state; the modifier rejects everyone else and there is no separate admin role. All hashes are treated as ***little endian*** during verification - block hashes, wtxids, and each proof. Therefore, callers must respect that encoding when constructing proofs. Every update emits `BlockInfoAdded` with the height, block hash, witness root, and coinbase depth for indexing and debugging.

### State

```solidity
uint256 public blockNumber;
mapping(uint256 => bytes32) public blockHashes;
mapping(bytes32 => bytes32) public witnessRoots;
mapping(bytes32 => uint256) public coinbaseDepths;
```

The `blockNumber` slot marks the next Bitcoin height the contract expects to append. Each successful update stores the block hash at its height, and also keys `witnessRoots` and `coinbaseDepths` by that block hash. The depth tracks where the coinbase transaction sits inside the tree so proof lengths can be enforced.

Be careful when reading `witnessRoots`: a zero root can either mean “*not recorded*” or “*single-transaction block whose coinbase wtxid is zero*,” so downstream contracts must distinguish those cases instead of assuming zero means missing data.

### Access Control

```solidity
modifier onlySystem()
```

Calls that mutate state must come from the system caller. With no owner exposed, there is nothing to reconfigure or override once deployed, which keeps the light client deterministic and narrow in scope.

Citrea purposely lags behind Bitcoin by a configurable finality depth enforced in the node. The sequencer only calls `setBlockInfo` for a Bitcoin block after it has sat that many confirmations deep, so `blockNumber` is always behind Bitcoin tip. This buffer avoids propagating shallow Bitcoin reorgs into Citrea (which could otherwise halt the chain).

On Citrea Testnet, that depth is set to `100` confirmations to withstand the frequent super-deep reorgs on Bitcoin Testnet4, so you should expect roughly 100-block latency between a Bitcoin event and its availability to on-chain contracts like the bridge.

### Functions

```solidity
function initializeBlockNumber(uint256 _blockNumber) external onlySystem
```

This one-time bootstrap anchors the light client to the Bitcoin height used when Citrea genesis is derived. Any second attempt reverts, so the sequencer must ensure it runs exactly once in the first block.

```solidity
function setBlockInfo(bytes32 _blockHash, bytes32 _witnessRoot, uint256 _coinbaseDepth) external onlySystem
```

Appends the next Bitcoin block in order. The call writes the provided block hash to `blockHashes` at the current `blockNumber`, stores the witness root and coinbase depth keyed by that hash, emits `BlockInfoAdded`, and finally increments `blockNumber`. Heights cannot be skipped or overwritten - if the sequencer attempts to jump ahead or rewind, the transaction will fail, keeping the light client consistent with Bitcoin’s linear history.

```solidity
function getBlockHash(uint256 height) external view returns (bytes32)
function getWitnessRootByHash(bytes32 blockHash) external view returns (bytes32)
function getWitnessRootByNumber(uint256 height) external view returns (bytes32)
```

The getters expose stored data for other contracts. A missing block hash returns zero.

Witness root getters mirror the storage semantics: they return zero when data has not been written, but they also return zero legitimately when the witness root itself is zero (single-transaction blocks where the coinbase wtxid is zero). Callers must not treat a zero witness root as proof of absence without additional context.

```solidity
function verifyInclusion(
    bytes32 blockHash,
    bytes32 wtxId,
    bytes calldata proof,
    uint256 index
) external view returns (bool)
```

```solidity
function verifyInclusion(
    uint256 height,
    bytes32 wtxId,
    bytes calldata proof,
    uint256 index
) external view returns (bool)
```

Both overloads rebuild the witness Merkle root from the provided wtxid, proof bytes, and index and compare it against storage. The proof length must equal the stored coinbase depth multiplied by 32 bytes, which hardens the verifier against variable-depth Merkle tricks such as the [Bitslog](https://bitslog.com/2018/06/09/leaf-node-weakness-in-bitcoin-merkle-tree-design/) attack.

```solidity
function verifyInclusionByTxId(
    uint256 height,
    bytes32 txId,
    bytes calldata blockHeader,
    bytes calldata proof,
    uint256 index
) external view returns (bool)
```

This variant works for legacy txids. It first hashes the provided header to confirm it matches the stored block hash, uses the coinbase depth to enforce proof length, extracts the transaction Merkle root from the header, and then delegates to `ValidateSPV.prove` for the inclusion check. Callers should keep the height in sync with the header to avoid mismatched proofs.

{% hint style="warning" %}
Passing a zero wtxid will verify because coinbase wtxids are zero. Please ensure that callers do not inadvertently prove inclusion of the coinbase transaction when they intended to prove user-supplied data, especially when any non-coinbase input is required for downstream logic.
{% endhint %}
