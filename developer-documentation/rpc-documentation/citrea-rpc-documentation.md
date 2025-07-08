# Citrea RPC Documentation

Complete reference for Citrea's special JSON-RPC API endpoints for interacting with the ledger data and debugging, outside of the standard Ethereum JSON-RPC methods.

## Base URLs

- **Local node:** `http://0.0.0.0:8080`
- **Public Testnet:** `https://rpc.testnet.citrea.xyz`

Replace these with your node's RPC URL and port as needed.

---

## Table of Contents

### 1. [Sync & Status Endpoints](#sync--status-endpoints)
- [`citrea_syncStatus`](#citrea_syncstatus)

### 2. [L2 Block Endpoints](#l2-block-endpoints)
- [`ledger_getL2BlockByNumber`](#ledger_getl2blockbynumber)
- [`ledger_getL2BlockByHash`](#ledger_getl2blockbyhash)
- [`ledger_getL2BlockRange`](#ledger_getl2blockrange)
- [`ledger_getHeadL2Block`](#ledger_getheadl2block)
- [`ledger_getHeadL2BlockHeight`](#ledger_getheadl2blockheight)

### 3. [Genesis & State Endpoints](#genesis--state-endpoints)
- [`ledger_getL2GenesisStateRoot`](#ledger_getl2genesisstateroot)

### 4. [Sequencer Commitment Endpoints](#sequencer-commitment-endpoints)
- [`ledger_getSequencerCommitmentsOnSlotByNumber`](#ledger_getsequencercommitmentsonslotbynumber)
- [`ledger_getSequencerCommitmentsOnSlotByHash`](#ledger_getsequencercommitmentsonslotbyhash)
- [`ledger_getSequencerCommitmentByIndex`](#ledger_getsequencercommitmentbyindex)

### 5. [Proof Endpoints](#proof-endpoints)
- [`ledger_getVerifiedBatchProofsBySlotHeight`](#ledger_getverifiedbatchproofsbyslotHeight)
- [`ledger_getLastVerifiedBatchProof`](#ledger_getlastverifiedbatchproof)

### 6. [L1 Integration Endpoints](#l1-integration-endpoints)
- [`ledger_getLastScannedL1Height`](#ledger_getlastscannedl1height)

### 7. [Status Query Endpoints](#status-query-endpoints)
- [`citrea_getLastCommittedL2Height`](#citrea_getlastcommittedl2height)
- [`citrea_getLastProvenL2Height`](#citrea_getlastprovenl2height)
- [`citrea_getL2StatusHeightsByL1Height`](#citrea_getl2statusheightsbyl1height)

### 8. [Deposit Endpoints](#deposit-endpoints)
- [`citrea_sendRawDepositTransaction`](#citrea_sendrawdeposittransaction)

### 9. [Debug & Trace Endpoints](#debug--trace-endpoints)
- [`debug_traceBlockByHash`](#debug_traceblockbyhash)
- [`debug_traceBlockByNumber`](#debug_traceblockbynumber)
- [`debug_traceTransaction`](#debug_tracetransaction)
- [`eth_estimateDiffSize`](#eth_estimatediffsize)

---

## Sync & Status Endpoints

### `citrea_syncStatus`

**Description**  
Returns the current synchronization status of the local Citrea node for both L1 (Bitcoin) and L2 (Citrea).

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"citrea_syncStatus","params":[],"id":1}' \
http://0.0.0.0:8080
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "citrea_syncStatus",
  "params": [],
  "id": 1
}
```

#### Parameters
_None_

#### Example Response

**If fully synced:**
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "l1Status": { "Synced": 19224 },
    "l2Status": { "Synced": 2326433 }
  }
}
```

**If still syncing:**
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "l1Status": { "Synced": 19224 },
    "l2Status": {
      "Syncing": {
        "headBlockNumber": 264052,
        "syncedBlockNumber": 27050
      }
    }
  }
}
```

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `l1Status` | `object` | L1 (Bitcoin) synchronization status |
| `l2Status` | `object` | L2 (Citrea) synchronization status |
| `Synced` | `number` | Block height when fully synced |
| `Syncing.headBlockNumber` | `number` | Latest known block number |
| `Syncing.syncedBlockNumber` | `number` | Current synced block number |

---

## L2 Block Endpoints

### `ledger_getL2BlockByNumber`

**Description**  
Returns a complete L2 block by its block number.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getL2BlockByNumber","params":[1000333],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getL2BlockByNumber",
  "params": [1000333],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `number` | `integer` | **Yes** | L2 block height to fetch |

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "header": {
      "height": "0xf438d",
      "hash": "999528b4d08dc8d0b05cbace0f5e661f36d824cedde1ffa75dc0c197b355c111",
      "prev_hash": "3dd4a535bc994f8f8d7d1f814be99f73dc522aa314905ca9985e479b350f17e7",
      "state_root": "c55d26ed7c1733d6be6283f68ab87db37271bf476008eba5231f5d321e0efe2c",
      "signature": "69f712ea23731ce8432867526084e260d1aeef9290f4c26889361e3bb5e168724bfe328986a56752778a2a93c360b9c291614ce4e15d4b1c2fac7bf31c7068e9",
      "l1_fee_rate": "0x9502f900",
      "timestamp": "0x67089f9c",
      "tx_merkle_root": "13148bcd464a1ef2824f13372513eda01d2f83d498790bf558060dc0bf4cbcbb"
    },
    "txs": [
      "00400000004f3fe920c0195eab45c67026fde19b23853c34e709c18e625e43702ca325f13f7a924be8beed58eb6afdaa596392fa7b40ccff1ead3a33e4db7617034366c2600201edff3b3ee593dbef54e2fbdd421070db55e2de2aebe75f398bd85ac97ed364ba0000000101000000b100000002f8ae8213fb6e018401312d0183021953945da3155a299558bcaa8da073ad74ed0827fb64a380b844a9059cbb000000000000000000000000ae45cbe2d1e90358cbd216bc16f2c9267a4ea80a00000000000000000000000000000000000000000000003b16c9e8eeb7c80000c080a04d09d171896b18b3651da059780fd92748b9813f1a6a11f26550cd759862fb99a03cb268a677ffdb6eb2e563e5ffda0fb32df547828ecf67affb03c087772daadc00000000000000009994040000000000"
    ]
  }
}
```

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `header.height` | `string` | Block height in hex format |
| `header.hash` | `string` | 32-byte block hash |
| `header.prev_hash` | `string` | Previous block hash |
| `header.state_root` | `string` | State root after this block |
| `header.signature` | `string` | Sequencer signature |
| `header.l1_fee_rate` | `string` | L1 fee rate in hex |
| `header.timestamp` | `string` | Block timestamp in hex |
| `header.tx_merkle_root` | `string` | Merkle root of transactions |
| `txs` | `array` | Array of transaction data (hex-encoded) |

---

### `ledger_getL2BlockByHash`

**Description**  
Returns a complete L2 block by its block hash.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getL2BlockByHash","params":["999528b4d08dc8d0b05cbace0f5e661f36d824cedde1ffa75dc0c197b355c111"],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getL2BlockByHash",
  "params": ["999528b4d08dc8d0b05cbace0f5e661f36d824cedde1ffa75dc0c197b355c111"],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `hash` | `string` | **Yes** | 32-byte L2 block hash (hex) |

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    /* Same structure as ledger_getL2BlockByNumber */
  }
}
```

---

### `ledger_getL2BlockRange`

**Description**  
Returns all L2 blocks in the inclusive range `[start, end]`.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getL2BlockRange","params":[1000333,1000335],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getL2BlockRange",
  "params": [1000333, 1000335],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `start` | `integer` | **Yes** | First block number in range |
| `end` | `integer` | **Yes** | Last block number in range |

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    { /* Block 1000333 */ },
    { /* Block 1000334 */ },
    { /* Block 1000335 */ }
  ]
}
```

---

### `ledger_getHeadL2Block`

**Description**  
Returns the latest (head) L2 block.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getHeadL2Block","params":[],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getHeadL2Block",
  "params": [],
  "id": 1
}
```

#### Parameters
_None_

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    /* Same structure as ledger_getL2BlockByNumber */
  }
}
```

---

### `ledger_getHeadL2BlockHeight`

**Description**  
Returns the block number of the latest L2 block.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getHeadL2BlockHeight","params":[],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getHeadL2BlockHeight",
  "params": [],
  "id": 1
}
```

#### Parameters
_None_

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0xbed0d6"
}
```

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `result` | `string` | Latest L2 block height in hex format |

---

## Genesis & State Endpoints

### `ledger_getL2GenesisStateRoot`

**Description**  
Returns the 32-byte genesis state root for L2.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getL2GenesisStateRoot","params":[],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getL2GenesisStateRoot",
  "params": [],
  "id": 1
}
```

#### Parameters
_None_

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x785c6d461111b5e9fdc515e0955d55baddfd8eb2dd2769342cc58060d6f5e0c5"
}
```

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `result` | `string` | 32-byte genesis state root (hex) |

---

## Sequencer Commitment Endpoints

### `ledger_getSequencerCommitmentsOnSlotByNumber`

**Description**  
Returns sequencer commitment(s) found in the specified DA slot height.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getSequencerCommitmentsOnSlotByNumber","params":[88001],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getSequencerCommitmentsOnSlotByNumber",
  "params": [88001],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `height` | `integer` | **Yes** | L1 DA slot height |

#### Example Response

**If commitments exist:**
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    {
      "merkleRoot": "0xd6dc3eb1a59be7bc1ece68318f82a60919f0f98d8c648337d588ce243a17f473",
      "index": "0xba5",
      "l2EndBlockNumber": "0xb7a20b"
    },
    {
      "merkleRoot": "0xf85a8f3f872fe8079fe778a2f6a8ebd92cf3d1a7c6324af025c8bffbb46d1811",
      "index": "0xba6",
      "l2EndBlockNumber": "0xb7a5f3"
    }
  ]
}
```

**If no commitments:**
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": null
}
```

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `merkleRoot` | `string` | Merkle root of sequencer commitment |
| `index` | `string` | Global commitment index (hex) |
| `l2EndBlockNumber` | `string` | Last L2 block in this commitment (hex) |

---

### `ledger_getSequencerCommitmentsOnSlotByHash`

**Description**  
Returns sequencer commitment(s) found in the DA slot identified by its L1 block hash.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getSequencerCommitmentsOnSlotByHash","params":["788636f732f490c5072582c7dcde85e34d737c8a4d98e792a59c9d0100000000"],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getSequencerCommitmentsOnSlotByHash",
  "params": ["788636f732f490c5072582c7dcde85e34d737c8a4d98e792a59c9d0100000000"],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `hash` | `string` | **Yes** | 32-byte DA slot hash (hex) |

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    {
      "merkleRoot": "0xd6dc3eb1a59be7bc1ece68318f82a60919f0f98d8c648337d588ce243a17f473",
      "index": "0xba5",
      "l2EndBlockNumber": "0xb7a20b"
    }
  ]
}
```

---

### `ledger_getSequencerCommitmentByIndex`

**Description**  
Returns a sequencer commitment by its global index.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getSequencerCommitmentByIndex","params":["0xba5"],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getSequencerCommitmentByIndex",
  "params": ["0xba5"],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `index` | `string` | **Yes** | Commitment index (hex) |

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "merkleRoot": "0xd6dc3eb1a59be7bc1ece68318f82a60919f0f98d8c648337d588ce243a17f473",
    "index": "0xba5",
    "l2EndBlockNumber": "0xb7a20b"
  }
}
```

---

## Proof Endpoints

### `ledger_getVerifiedBatchProofsBySlotHeight`

**Description**  
Returns verified ZK batch proof(s) for the specified DA slot height.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getVerifiedBatchProofsBySlotHeight","params":[85890],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getVerifiedBatchProofsBySlotHeight",
  "params": [85890],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `height` | `integer` | **Yes** | DA slot height (L1) |

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    {
      "proof": "0x0300000000000000000000000000000081670c116a84c0351f18c3...", // ~2MB proof data
      "proofOutput": {
        "stateRoots": [
          "0x5e2a5b8da99b65c5eba4b36f2368321dad95c08c641d698b09ca6756caac589e",
          "0x182a9543bf64a90e78f7d810dedebdf4da3322752db5ee5533fae564e31033f7"
          // ... additional state roots
        ],
        "finalL2BlockHash": "0x8fe769cdb2d34f60b5a625caa8697ae1d1f213b558c9dc9e681059f18732e608",
        "stateDiff": {
          "0x412f612f0201edff3b3ee593dbef54e2fbdd421070db55e2de2aebe75f398bd85ac97ed364": "0xf4fd64e249e658fdb748f83190e47f0b1cd5e87fe30d005c257d7a94cca458e7167e690000000000"
          // ... state differences (truncated for readability)
        },
        "lastL2Height": "0xae0e97",
        "sequencerCommitmentIndexRange": ["0x92c", "0x92e"],
        "sequencerCommitmentHashes": [
          "0x8a9cd0b9fda255370073abff4176416665d14552d5f0d671998ca89523041b36"
          // ... additional hashes
        ],
        "lastL1HashOnBitcoinLightClientContract": "0xa1717b35aa14be01ce037d1a346c4c1714afcfa5e8a66a35af5ead0000000000",
        "previousCommitmentIndex": "0x92b",
        "previousCommitmentHash": "0xa37a45539d40448d22f6da7978ca58f5845b72a5043129941817099471198749"
      }
    }
  ]
}
```

> **Note:** The actual proof data is approximately 2MB. This example shows truncated data for documentation purposes.

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `proof` | `string` | ZK proof data (hex, ~2MB) |
| `proofOutput.stateRoots` | `array` | Array of state roots |
| `proofOutput.finalL2BlockHash` | `string` | Final L2 block hash in batch |
| `proofOutput.stateDiff` | `object` | State changes (key-value pairs) |
| `proofOutput.lastL2Height` | `string` | Last L2 height in proof (hex) |
| `proofOutput.sequencerCommitmentIndexRange` | `array` | Range of commitment indices |
| `proofOutput.sequencerCommitmentHashes` | `array` | Commitment hashes in range |

---

### `ledger_getLastVerifiedBatchProof`

**Description**  
Returns the most recent verified batch proof.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getLastVerifiedBatchProof","params":[],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getLastVerifiedBatchProof",
  "params": [],
  "id": 1
}
```

#### Parameters
_None_

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    /* Same structure as ledger_getVerifiedBatchProofsBySlotHeight */
  }
}
```

---

## L1 Integration Endpoints

### `ledger_getLastScannedL1Height`

**Description**  
Returns the highest L1 block number the node has scanned so far.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getLastScannedL1Height","params":[],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getLastScannedL1Height",
  "params": [],
  "id": 1
}
```

#### Parameters
_None_

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x15e53"
}
```

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `result` | `string` | Last scanned L1 block height (hex) |

---

## Status Query Endpoints

### `citrea_getLastCommittedL2Height`

**Description**  
Returns the latest L2 block height that has been committed to Bitcoin via sequencer commitment.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"citrea_getLastCommittedL2Height","params":[],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "citrea_getLastCommittedL2Height",
  "params": [],
  "id": 1
}
```

#### Parameters
_None_

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "height": 12489007,
    "commitment_index": 3444
  }
}
```

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `height` | `number` | Latest committed L2 block height |
| `commitment_index` | `number` | Sequencer commitment index |

---

### `citrea_getLastProvenL2Height`

**Description**  
Returns the latest L2 block height that has been proven on Bitcoin with a validity proof.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"citrea_getLastProvenL2Height","params":[],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "citrea_getLastProvenL2Height",
  "params": [],
  "id": 1
}
```

#### Parameters
_None_

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "height": 11457999,
    "commitment_index": 2401
  }
}
```

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `height` | `number` | Latest proven L2 block height |
| `commitment_index` | `number` | Sequencer commitment index |

---

### `citrea_getL2StatusHeightsByL1Height`

**Description**  
Returns committed and proven L2 heights as of a given Bitcoin block.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"citrea_getL2StatusHeightsByL1Height","params":[800000],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "citrea_getL2StatusHeightsByL1Height",
  "params": [800000],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `l1_height` | `integer` | **Yes** | Bitcoin block height to query |

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "committed": {
      "height": 12489007,
      "commitment_index": 3444
    },
    "proven": {
      "height": 11457999,
      "commitment_index": 2401
    }
  }
}
```

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `committed` | `object` | Latest committed L2 status |
| `proven` | `object` | Latest proven L2 status |

---

## Deposit Endpoints

### `citrea_sendRawDepositTransaction`

**Description**  
Submits a raw deposit transaction to the Citrea sequencer. The node first simulates the deposit; if valid, it is queued for inclusion.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"citrea_sendRawDepositTransaction","params":["0x01020304..."],"id":1}' \
http://0.0.0.0:8080
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "citrea_sendRawDepositTransaction",
  "params": ["0x01020304..."],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `deposit` | `string` | **Yes** | Raw serialized deposit tx data (hex) |

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": null
}
```

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `result` | `null` | Success returns null |

---

## Debug & Trace Endpoints

### `debug_traceBlockByHash`

**Description**  
Returns full execution traces for all transactions in a block, addressed by block hash.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"debug_traceBlockByHash","params":["0xcdaab76f4f92d9d9dbd9a4ad2e7d5d2e467a73564fab904f33cfb537e3c46487",{}],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "debug_traceBlockByHash",
  "params": ["0xcdaab76f4f92d9d9dbd9a4ad2e7d5d2e467a73564fab904f33cfb537e3c46487", {}],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `blockHash` | `string` | **Yes** | 32-byte block hash (hex) |
| `options` | `object` | No | Tracer options (default `{}`) |

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    {
      "result": {
        "failed": false,
        "gas": 46004,
        "returnValue": "0000000000000000000000000000000000000000000000000000000000000001",
        "structLogs": [
          {
            "pc": 0,
            "op": "PUSH1",
            "gas": 78416,
            "gasCost": 3,
            "depth": 1,
            "stack": []
          }
          // ... additional trace steps
        ]
      },
      "txHash": "0xc1eed81fcc7ce4203bdc9f24157e03814c3dbdc678849cdd5b8499c6c2c2d952"
    }
  ]
}
```

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `result.failed` | `boolean` | Whether transaction failed |
| `result.gas` | `number` | Gas used |
| `result.returnValue` | `string` | Transaction return value |
| `result.structLogs` | `array` | Detailed execution trace |
| `txHash` | `string` | Transaction hash |

---

### `debug_traceBlockByNumber`

**Description**  
Returns full execution traces for all transactions in a block specified by number or tag.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"debug_traceBlockByNumber","params":["0xBF2249",{}],"id":1}' \
http://0.0.0.0:8080
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "debug_traceBlockByNumber",
  "params": ["0xBF2249", {}],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `blockNumber` | `string` | **Yes** | Block height (hex) or `"latest"` |
| `options` | `object` | No | Tracer options |

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    /* Same structure as debug_traceBlockByHash */
  ]
}
```

---

### `debug_traceTransaction`

**Description**  
Returns execution trace for a single transaction by hash.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"debug_traceTransaction","params":["0xc1eed81fcc7ce4203bdc9f24157e03814c3dbdc678849cdd5b8499c6c2c2d952",{}],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "debug_traceTransaction",
  "params": ["0xc1eed81fcc7ce4203bdc9f24157e03814c3dbdc678849cdd5b8499c6c2c2d952", {}],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `txHash` | `string` | **Yes** | Transaction hash (hex) |
| `options` | `object` | No | Tracer options |

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "failed": false,
    "gas": 46004,
    "returnValue": "0000000000000000000000000000000000000000000000000000000000000001",
    "structLogs": [
      /* Same structure as debug_traceBlockByHash trace data */
    ]
  }
}
```

---

### `eth_estimateDiffSize`

**Description**  
Returns estimated diff size and gas consumption for a potential transaction.

#### HTTP Request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"eth_estimateDiffSize","params":[{"from":"0x0452663bfc46B86aC93d8220D9847cF5220D7642","to":"0x5fbdb2315678afecb367f032d93f642f64180aa3","value":"10000"}],"id":1}' \
https://rpc.testnet.citrea.xyz
```

#### JSON-RPC Body
```json
{
  "jsonrpc": "2.0",
  "method": "eth_estimateDiffSize",
  "params": [{
    "from": "0x0452663bfc46B86aC93d8220D9847cF5220D7642",
    "to": "0x5fbdb2315678afecb367f032d93f642f64180aa3",
    "value": "10000"
  }],
  "id": 1
}
```

#### Parameters
| Name | Type | Required | Description |
|------|------|----------|-------------|
| `from` | `string` | **Yes** | Sender address |
| `to` | `string` | **Yes** | Recipient address |
| `value` | `string` | **Yes** | Amount to send (wei) |
| `gas` | `string` | No | Gas limit |
| `gasPrice` | `string` | No | Gas price |
| `data` | `string` | No | Transaction data |

#### Example Response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "gas": "0x5208",
    "l1DiffSize": "0x11"
  }
}
```

#### Response Fields
| Field | Type | Description |
|-------|------|-------------|
| `gas` | `string` | Estimated gas consumption (hex) |
| `l1DiffSize` | `string` | Estimated L1 diff size (hex) |

---

## Error Responses

All endpoints may return standard JSON-RPC error responses:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32602,
    "message": "Invalid params"
  }
}
```

### Common Error Codes
| Code | Message | Description |
|------|---------|-------------|
| `-32600` | Invalid Request | The JSON sent is not a valid Request object |
| `-32601` | Method not found | The method does not exist / is not available |
| `-32602` | Invalid params | Invalid method parameter(s) |
| `-32603` | Internal error | Internal JSON-RPC error |

---

## Notes

- All hex values should include the `0x` prefix
- Block heights can be specified as integers or hex strings
- Response data may be truncated in documentation for readability
- Large responses (like proofs) may be several MB in size
- Always check for error responses before processing results 