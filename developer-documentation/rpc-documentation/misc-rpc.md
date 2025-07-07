# Citrea JSON‑RPC: `citrea_`, `debug_`, and Trace Endpoints

> **Base URL examples**  
> • Local node — `http://0.0.0.0:8080`  
> • Public Testnet — `https://rpc.testnet.citrea.xyz`  
> Replace these with your node’s RPC URL and port as needed.

---

## Table of Contents
1. [`citrea_syncStatus`](#citrea_syncstatus)
2. [`citrea_sendRawDepositTransaction`](#citrea_sendrawdeposittransaction)
3. [`citrea_getLastCommittedL2Height`](#citrea_getlastcommittedl2height)
4. [`citrea_getLastProvenL2Height`](#citrea_getlastprovenl2height)
5. [`citrea_getL2StatusHeightsByL1Height`](#citrea_getl2statusheightsbyl1height)
6. [`debug_traceBlockByHash`](#debug_traceblockbyhash)
7. [`debug_traceBlockByNumber`](#debug_traceblockbynumber)
8. [`debug_traceTransaction`](#debug_tracetransaction)
9. [`debug_subscribe`](#debug_subscribe)
10. [`debug_unsubscribe`](#debug_unsubscribe)

---

## `citrea_syncStatus`

**Description**  
Returns the synchronization status of your Citrea node for both L1 (Bitcoin) and L2 (Citrea).

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json"   --data '{"jsonrpc":"2.0","method":"citrea_syncStatus","params":[],"id":1}'   http://0.0.0.0:8080
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

### Example response
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

---

## `citrea_sendRawDepositTransaction`

**Description**  
Submits a **raw deposit transaction** to the Citrea sequencer. The node first simulates the deposit; if valid, it is queued for inclusion.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json"   --data '{"jsonrpc":"2.0","method":"citrea_sendRawDepositTransaction","params":["0x01020304..."],"id":1}'   http://0.0.0.0:8080
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "citrea_sendRawDepositTransaction",
  "params": ["0x01020304..."],
  "id": 1
}
```

### Parameters
| Name    | Type      | Required | Description                                                   |
|---------|-----------|----------|---------------------------------------------------------------|
| deposit | hex bytes | **Yes**  | Raw serialized deposit tx data (hex‑encoded).                |

### Example response
```json
{ "jsonrpc": "2.0", "id": 1, "result": null }
```

---

## `citrea_getLastCommittedL2Height`

**Description**  
Returns the latest L2 block height that has been **committed** to Bitcoin (via sequencer commitment).

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json"   --data '{"jsonrpc":"2.0","method":"citrea_getLastCommittedL2Height","params":[],"id":1}' https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "citrea_getLastCommittedL2Height",
  "params": [],
  "id": 1
}
```

### Parameters  
_None_

### Example response
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

---

## `citrea_getLastProvenL2Height`

**Description**  
Returns the latest L2 block height that has been **proven** on Bitcoin with a validity proof.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json"   --data '{"jsonrpc":"2.0","method":"citrea_getLastProvenL2Height","params":[],"id":1}'   https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "citrea_getLastProvenL2Height",
  "params": [],
  "id": 1
}
```

### Parameters  
_None_

### Example response
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

---

## `citrea_getL2StatusHeightsByL1Height`

**Description**  
Returns committed and proven L2 heights as of a given Bitcoin block.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json"   --data '{"jsonrpc":"2.0","method":"citrea_getL2StatusHeightsByL1Height","params":[800000],"id":1}'   https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "citrea_getL2StatusHeightsByL1Height",
  "params": [800000],
  "id": 1
}
```

### Parameters
| Name      | Type    | Required | Description                                 |
|-----------|---------|----------|---------------------------------------------|
| l1_height | integer | **Yes**  | Bitcoin block height to query status at.    |

### Example response
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

---

## `debug_traceBlockByHash`

**Description**  
Returns full execution traces for all txs in a block, addressed by block hash.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json"   --data '{"jsonrpc":"2.0","method":"debug_traceBlockByHash","params":["0xcdaab76f4f92d9d9dbd9a4ad2e7d5d2e467a73564fab904f33cfb537e3c46487",{}],"id":1}'   https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "debug_traceBlockByHash",
  "params": ["0xcdaab76f4f92d9d9dbd9a4ad2e7d5d2e467a73564fab904f33cfb537e3c46487", {}],
  "id": 1
}
```

### Parameters
| Name      | Type   | Required | Description                          |
|-----------|--------|----------|--------------------------------------|
| blockHash | hash   | **Yes**  | 32‑byte block hash to trace.         |
| options   | object | No       | Tracer options object (default `{}`).|

### Example response
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
          },
          {
            "pc": 2,
            "op": "PUSH1",
            "gas": 78413,
            "gasCost": 3,
            "depth": 1,
            "stack": [
              "0x60"
            ]
          },
          // ... more ...
        ]
      },
      "txHash": "0xc1eed81fcc7ce4203bdc9f24157e03814c3dbdc678849cdd5b8499c6c2c2d952"
    }
  ]
}
```

---

## `debug_traceBlockByNumber`

**Description**  
Traces all txs in a block specified by number or tag.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json"   --data '{"jsonrpc":"2.0","method":"debug_traceBlockByNumber","params":["0xBF2249",{}],"id":1}'   http://0.0.0.0:8080
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "debug_traceBlockByNumber",
  "params": ["0xBF2249", {}],
  "id": 1
}
```

### Parameters
| Name        | Type         | Required | Description                       |
|-------------|--------------|----------|-----------------------------------|
| blockNumber | number / tag | **Yes**  | Block height or `"latest"`.       |
| options     | object       | No       | Tracer options.                   |

### Example response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [
    { // ... same as `debug_traceBlockByHash` response ... } 
  ]
}
```

---

## `debug_traceTransaction`

**Description**  
Traces a single transaction by hash.

### HTTP request
```sh
curl -X POST -H "Content-Type: application/json"   --data '{"jsonrpc":"2.0","method":"debug_traceTransaction","params":["0xc1eed81fcc7ce4203bdc9f24157e03814c3dbdc678849cdd5b8499c6c2c2d952",{}],"id":1}'   https://rpc.testnet.citrea.xyz
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "method": "debug_traceTransaction",
  "params": ["0xc1eed81fcc7ce4203bdc9f24157e03814c3dbdc678849cdd5b8499c6c2c2d952", {}],
  "id": 1
}
```

### Parameters
| Name   | Type   | Required | Description                  |
|--------|--------|----------|------------------------------|
| txHash | hash   | **Yes**  | Transaction hash to trace.   |
| options| object | No       | Tracer options.              |

### Example response
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    // ... same structure as `debug_traceBlockByHash` result for a single tx ...
  }
}
```

---
<!-- 
## `debug_subscribe`

**Description**  
Opens a WebSocket subscription to stream block traces.

### WebSocket request
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "debug_subscribe",
  "params": ["traceChain", 1000, 1010, {}]
}
```

### Parameters
| Name       | Type   | Required | Description                                               |
|------------|--------|----------|-----------------------------------------------------------|
| topic      | string | **Yes**  | Subscription topic (`"traceChain"`).                    |
| startBlock | number | **Yes**  | Start block (exclusive).                                  |
| endBlock   | number | **Yes**  | End block (inclusive).                                    |
| options    | object | No       | Tracer options.                                           |

### Server response
```json
{ "jsonrpc": "2.0", "id": 1, "result": "0x8f..." }
```

Subscription notifications:
```json
{
  "jsonrpc": "2.0",
  "method": "debug_subscription",
  "params": {
    "subscription": "0x8f...",
    "result": [ /* trace for a block */ ]
  }
}
```

---

## `debug_unsubscribe`

**Description**  
Cancels a debug subscription (WebSocket only).

### WebSocket request
```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "debug_unsubscribe",
  "params": ["0x8f..."]
}
```

### Parameters
| Name           | Type   | Required | Description                      |
|----------------|--------|----------|----------------------------------|
| subscriptionId | string | **Yes**  | ID returned by `debug_subscribe`.|

### Example response
```json
{ "jsonrpc": "2.0", "id": 2, "result": true }
```

--- -->


## `eth_estimateDiffSize`

**Description**  
Returns diff size of a potential transaction with gas consumption.

### HTTP request
```sh
curl https://rpc.testnet.citrea.xyz -X POST --data '{"method": "eth_estimateDiffSize", "params":[{"from": "0x0452663bfc46B86aC93d8220D9847cF5220D7642", "to": "0x5fbdb2315678afecb367f032d93f642f64180aa3", "value": "10000"}], "id": 1, "jsonrpc": "2.0"}' -H "Content-Type: application/json"
```

### JSON‑RPC body
```json
{
  "jsonrpc": "2.0",
  "params": [{"from": "0x0452663bfc46B86aC93d8220D9847cF5220D7642", "to": "0x5fbdb2315678afecb367f032d93f642f64180aa3", "value": "10000"}],
  "id": 1
}
```

### Parameters
| Name   | Type   | Required | Description                  |
|--------|--------|----------|------------------------------|
| from   | string | **Yes**  | Sender address.              |
| to     | string | **Yes**  | Recipient address.           |
| value  | string | **Yes**  | Amount to send (in wei).     |

For the rest any EVM transaction fields can be included as well, such as `gas`, `gasPrice`, `data`, etc.


### Example response
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
