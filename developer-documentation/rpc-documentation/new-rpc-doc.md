# Citrea Ledger JSON‑RPC API Endpoints (Public API)

> **Base URL examples**  
> *Local node:* `http://0.0.0.0:8080`  
> *Public Testnet:* `https://rpc.testnet.citrea.xyz`

---

## `citrea_syncStatus`

**Description**  
Returns the current synchronization status of the local Citrea node for both L1 (Bitcoin) and L2 (roll‑up).

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"citrea_syncStatus","params":[],"id":1}' \
http://0.0.0.0:8080
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "citrea_syncStatus",
  "params": [],
  "id": 1
}
```

### Parameters  
_None_

### Example Response

If synced fully:
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "l1Status": {
      "synced": "0x15e47"
    },
    "l2Status": {
      "synced": "0xbecfbe"
    }
  }
}
```
If still syncing:
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "l1Status": {
      "synced": "0x15e47"
    },
    "l2Status": {
      "syncing": {
        "headBlockNumber": "0xbecfbf",
        "syncedBlockNumber": "0xbecfbe"
      }
    }
  }
}
```

---

## `ledger_getL2BlockByNumber`

**Description**  
Returns an L2 block (soft‑confirmed) by its block number.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getL2BlockByNumber","params":[1000333],"id":1}' \
https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getL2BlockByNumber",
  "params": [1000333],
  "id": 1
}
```

### Parameters  

| Name   | Type    | Required | Description                     |
|--------|---------|----------|---------------------------------|
| number | Integer | Yes      | L2 block height to fetch.       |

### Example response
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

---

## `ledger_getL2BlockByHash`

**Description**  
Returns an L2 block (soft‑confirmed) by its block hash.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getL2BlockByHash","params":["999528b4d08dc8d0b05cbace0f5e661f36d824cedde1ffa75dc0c197b355c111"],"id":1}' \
https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getL2BlockByHash",
  "params": ["999528b4d08dc8d0b05cbace0f5e661f36d824cedde1ffa75dc0c197b355c111"],
  "id": 1
}
```

### Parameters  

| Name | Type       | Required | Description                 |
|------|------------|----------|-----------------------------|
| hash | Hex string | Yes      | 32‑byte L2 block hash.      |

### Example response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": { /* same structure as ledger_getL2BlockByNumber */ }
}
```

---

## `ledger_getL2BlockRange`

**Description**  
Fetches all L2 blocks in the inclusive range `[start, end]`.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getL2BlockRange","params":[1000333,1000335],"id":1}' \
https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getL2BlockRange",
  "params": [1000333, 1000335],
  "id": 1
}
```

### Parameters  

| Name | Type    | Required | Description                           |
|------|---------|----------|---------------------------------------|
| start| Integer | Yes      | First L2 block number in the range.   |
| end  | Integer | Yes      | Last L2 block number in the range.    |

### Example response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    { /* block 1000333 in the format of ledger_getL2BlockByNumber */ },
    { /* block 1000334 in the format of ledger_getL2BlockByNumber */ },
    { /* block 1000335 in the format of ledger_getL2BlockByNumber */ },
  ]
}
```

---

## `ledger_getHeadL2Block`

**Description**  
Returns the **latest** (head) L2 block.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getHeadL2Block","params":[],"id":1}' \
https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getHeadL2Block",
  "params": [],
  "id": 1
}
```

### Parameters  
_None_

### Example response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": { /* latest L2 block in the format of ledger_getL2BlockByNumber */ }
}
```

---

## `ledger_getHeadL2BlockHeight`

**Description**  
Returns the block number of the latest L2 block.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getHeadL2BlockHeight","params":[],"id":1}' \
https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getHeadL2BlockHeight",
  "params": [],
  "id": 1
}
```

### Parameters  
_None_

### Example response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0xbed0d6"
}
```

---

## `ledger_getL2GenesisStateRoot`

**Description**  
Returns the 32‑byte genesis state root for L2.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getL2GenesisStateRoot","params":[],"id":1}' \
https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getL2GenesisStateRoot",
  "params": [],
  "id": 1
}
```

### Parameters  
_None_

### Example response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x785c6d461111b5e9fdc515e0955d55baddfd8eb2dd2769342cc58060d6f5e0c5"
}
```

---

## `ledger_getSequencerCommitmentsOnSlotByNumber`

**Description**  
Returns the sequencer commitment(s) found in the specified DA slot height.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getSequencerCommitmentsOnSlotByNumber","params":[88001],"id":1}' \
https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getSequencerCommitmentsOnSlotByNumber",
  "params": [88001],
  "id": 1
}
```

### Parameters  

| Name   | Type    | Required | Description               |
|--------|---------|----------|---------------------------|
| height | Integer | Yes      | L1 DA slot height.        |

### Example response

If there are sequencer commitments in the Bitcoin block:

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
    },
    {
      "merkleRoot": "0x3112c2c5a447a27c71b064f15f409cb8297d8db2200feb3b52996b2b47bbebc7",
      "index": "0xba7",
      "l2EndBlockNumber": "0xb7a9db"
    }
  ]
}
```

Otherwise:
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": null
}
```

---

## `ledger_getSequencerCommitmentsOnSlotByHash`

**Description**  
Returns sequencer commitment(s) found in the DA slot identified by its L1 block hash.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getSequencerCommitmentsOnSlotByHash","params":["788636f732f490c5072582c7dcde85e34d737c8a4d98e792a59c9d0100000000"],"id":1}' \
https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getSequencerCommitmentsOnSlotByHash",
  "params": ["788636f732f490c5072582c7dcde85e34d737c8a4d98e792a59c9d0100000000"],
  "id": 1
}
```

### Parameters  

| Name | Type       | Required | Description                      |
|------|------------|----------|----------------------------------|
| hash | Hex string | Yes      | 32‑byte DA slot hash to query.   |

### Example response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    { /* Same format as the ledger_getSequencerCommitmentsOnSlotByNumber response */ },
    { /* Same format as the ledger_getSequencerCommitmentsOnSlotByNumber response */ },
    { /* Same format as the ledger_getSequencerCommitmentsOnSlotByNumber response */ }

  ]
}
```

---

## `ledger_getSequencerCommitmentByIndex`

**Description**  
Fetches a sequencer commitment by its global index.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getSequencerCommitmentByIndex","params":["0xba5"],"id":1}' \
https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getSequencerCommitmentByIndex",
  "params": ["0xba5"],
  "id": 1
}
```

### Parameters  

| Name  | Type    | Required | Description                    |
|-------|---------|----------|--------------------------------|
| index | Hex     | Yes      | Commitment index (0‑based).    |

### Example response
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

## `ledger_getVerifiedBatchProofsBySlotHeight`

**Description**  
Returns **verified** ZK batch proof(s) for the specified DA slot height.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getVerifiedBatchProofsBySlotHeight","params":[85890],"id":1}' \
https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getVerifiedBatchProofsBySlotHeight",
  "params": [85890],
  "id": 1
}
```

### Parameters  

| Name   | Type    | Required | Description              |
|--------|---------|----------|--------------------------|
| height | Integer | Yes      | DA slot height (L1).     |

### Example response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    {
      "proof": "0x0300000000000000000000000000000081670c116a84c0351f18c3...", // a big proof ...
      "proofOutput": {
        "stateRoots": [
          "0x5e2a5b8da99b65c5eba4b36f2368321dad95c08c641d698b09ca6756caac589e",
          "0x182a9543bf64a90e78f7d810dedebdf4da3322752db5ee5533fae564e31033f7",
          "0x9bbfcb9ff685636c6a113149d7b5c48968ecad41dc35d1ca2aa781dee613ccd3",
          "0xab1a688214f78fd14e7c74af62ea0d11a397ac17575c3607a236b6a686a14ee4"
        ],
        "finalL2BlockHash": "0x8fe769cdb2d34f60b5a625caa8697ae1d1f213b558c9dc9e681059f18732e608",
        "stateDiff": {
          "0x412f612f0201edff3b3ee593dbef54e2fbdd421070db55e2de2aebe75f398bd85ac97ed364": "0xf4fd64e249e658fdb748f83190e47f0b1cd5e87fe30d005c257d7a94cca458e7167e690000000000",
          "0x452f482f0000000000000000": "0xadc5e9c92c1368c62f0bad65bc54fa6c32cb1b76178a0253a076e751efd64e5c",
          "0x452f482f0100000000000000": "0x7750698f30ea5fa90900aa6817521ef8832c474582904d2688453ff0a2ccc3b5",
          // ...
          // ...
          // a big stateDiff ...
        },
        "lastL2Height": "0xae0e97",
        "sequencerCommitmentIndexRange": [
          "0x92c",
          "0x92e"
        ],
        "sequencerCommitmentHashes": [
          "0x8a9cd0b9fda255370073abff4176416665d14552d5f0d671998ca89523041b36",
          "0x0dd6fd7f8d409ce65c389bae936502129d718ef0683758c7c1b2abbcd42b72f0",
          "0xaf42ca3ba4cdff0e4571d017732e4f0124f623ddf8b87f8eb267b7634af320f1"
        ],
        "lastL1HashOnBitcoinLightClientContract": "0xa1717b35aa14be01ce037d1a346c4c1714afcfa5e8a66a35af5ead0000000000",
        "previousCommitmentIndex": "0x92b",
        "previousCommitmentHash": "0xa37a45539d40448d22f6da7978ca58f5845b72a5043129941817099471198749"
      }
    }
  ]
}
```
---

## `ledger_getLastVerifiedBatchProof`

**Description**  
Returns the most recent verified batch proof.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getLastVerifiedBatchProof","params":[],"id":1}' \
https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getLastVerifiedBatchProof",
  "params": [],
  "id": 1
}
```

### Parameters  
_None_

### Example response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    { /* Same format as the ledger_getVerifiedBatchProofsBySlotHeight response */ },
  }
}
```

---

## `ledger_getLastScannedL1Height`

**Description**  
Returns the highest L1 block number the node has scanned so far.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"ledger_getLastScannedL1Height","params":[],"id":1}' \
https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "ledger_getLastScannedL1Height",
  "params": [],
  "id": 1
}
```

### Parameters  
_None_

### Example response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x15e53"
}
```

---