# Ethereum JSON-RPC Endpoints

Complete reference for Ethereum-compatible JSON-RPC API endpoints supported by Citrea. These endpoints provide compatibility with existing EVM tooling and libraries.

## Base URLs

- **Local node:** `http://0.0.0.0:8080`
- **Public Testnet:** `https://rpc.testnet.citrea.xyz` 
- **Alternative RPC Providers:** Check [BlastAPI](https://blastapi.io) as an alternative RPC service with generous free limits & better rate limits for Citrea Testnet.

---

## Supported JSON-RPC Methods

### Web3 Methods

- **[`web3_clientVersion`](https://ethereum.org/en/developers/docs/apis/json-rpc#web3_clientversion)** - Returns the current client version

- **[`web3_sha3`](https://ethereum.org/en/developers/docs/apis/json-rpc#web3_sha3)** - Returns Keccak-256 hash of the given data

### Net Methods

- **[`net_version`](https://ethereum.org/en/developers/docs/apis/json-rpc#net_version)** - Returns the current network ID

### Ethereum Methods

#### Synchronization & Status

- **[`eth_syncing`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_syncing)** - Returns sync status object or false if fully synced

- **[`eth_chainId`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_chainid)** - Returns the chain ID

#### Gas & Fees

- **[`eth_gasPrice`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_gasprice)** - Returns current gas price estimate

- **[`eth_feeHistory`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_feehistory)** - Returns historical base fees and reward percentiles (EIP-1559)

- **[`eth_maxPriorityFeePerGas`](https://docs.blastapi.io/blast-documentation/apis-documentation/core-api/ethereum/eth_maxpriorityfeepergas)** - Returns suggested priority fee (tip) per gas

- **`eth_maxFeePerGas`** - Returns estimated maximum fee per gas
    - `curl -H "Content-type: application/json" -X POST --data '{"jsonrpc":"2.0","method":"eth_maxFeePerGas","params":[],"id":1}' https://rpc.testnet.citrea.xyz | jq .`

#### Account & Balance

- **[`eth_blockNumber`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_blocknumber)** - Returns the latest block number

- **[`eth_getBalance`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_getbalance)** - Returns account balance at given address and block

- **[`eth_getStorageAt`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_getstorageat)** - Returns storage value at address/key for given block

- **[`eth_getTransactionCount`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_gettransactioncount)** - Returns nonce for an address

- **[`eth_getCode`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_getcode)** - Returns contract bytecode at address

#### Block Information

- **[`eth_getBlockTransactionCountByHash`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_getblocktransactioncountbyhash)** - Returns number of transactions in block by hash

- **[`eth_getBlockTransactionCountByNumber`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_getblocktransactioncountbynumber)** - Returns number of transactions in block by number

- **[`eth_getBlockByHash`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_getblockbyhash)** - Returns block by hash 
    - *Note*: There is an extra field in the response, `l1FeeRate`.

- **[`eth_getBlockByNumber`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_getblockbynumber)** - Returns block by number
    - *Note*: There is an extra field in this response, `l1FeeRate`.

- **[`eth_getUncleByBlockHashAndIndex`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_getunclebyblockhashAndindex)** - Returns null (no uncles in Citrea)

#### Transaction Methods

- **[`eth_sendRawTransaction`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_sendrawtransaction)** - Submits a signed transaction

- **[`eth_getTransactionByHash`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_gettransactionbyhash)** - Returns transaction details by hash

- **[`eth_getTransactionByBlockHashAndIndex`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_gettransactionbyblockhashAndindex)** - Returns transaction by block hash and index

- **[`eth_getTransactionByBlockNumberAndIndex`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_gettransactionbyblocknumberandindex)** - Returns transaction by block number and index

- **[`eth_getTransactionReceipt`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_gettransactionreceipt)** - Returns transaction receipt by hash
    - *Note*: There are extra fields in the response, `l1FeeRate` and `l1DiffSize`.

#### Call & Execution

- **[`eth_call`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_call)** - Executes a read-only call

- **[`eth_estimateGas`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_estimategas)** - Returns gas estimate for a transaction
    - *Note*: eth_estimateGas carries a tiny overhead for the L1 fee costs. For a more precise estimation that separates L1 and L2 costs, please use [`eth_estimateDiffSize`](./citrea-rpc-documentation.md#eth_estimatediffsize) (Citrea-specific).

#### Logs

- **[`eth_getLogs`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_getlogs)** - Returns logs matching filter criteria

### State Proofs & Access Lists

- **[`eth_getProof`](https://docs.blastapi.io/blast-documentation/apis-documentation/core-api/ethereum/eth_getproof)** - Returns account and storage proof (EIP-1186)

- **[`eth_createAccessList`](https://geth.ethereum.org/docs/interacting-with-geth/rpc/ns-eth#eth-createaccesslist)** - Generates access list for transaction (EIP-2930)

### Bulk Operations

- **[`eth_getBlockReceipts`](https://docs.blastapi.io/blast-documentation/erigon-api/eth_getblockreceipts)** - Returns all transaction receipts for a block
    - *Note*: There are extra fields in some objects of the response, such as `l1FeeRate` and `l1DiffSize`.

### Citrea-Specific Extensions

- **[`eth_estimateDiffSize`](./citrea-rpc-documentation.md#eth_estimatediffsize)** - Estimates L1 data diff size for rollup transaction

### Debug Endpoints

- **[`debug_traceTransaction`](https://geth.ethereum.org/docs/interacting-with-geth/rpc/ns-debug#debug-tracetransaction)** - Traces execution of a transaction (requires full node)

- **[`debug_traceBlockByHash`](https://geth.ethereum.org/docs/interacting-with-geth/rpc/ns-debug#debug-traceblockbyhash)** - Traces all transactions in a block by hash

- **[`debug_traceBlockByNumber`](https://geth.ethereum.org/docs/interacting-with-geth/rpc/ns-debug#debug-traceblockbynumber)** - Traces all transactions in a block by number

### Subscription Methods (WebSocket only)

- **[`eth_subscribe`](https://docs.blastapi.io/blast-documentation/apis-documentation/core-api/ethereum/eth_subscribe)** - Creates subscription for events

- **[`eth_unsubscribe`](https://docs.blastapi.io/blast-documentation/apis-documentation/core-api/ethereum/eth_unsubscribe)** - Cancels active subscription

---

## Notes

- Almost all methods follow the standard Ethereum JSON-RPC specification formats (see notes above)
- You can use WebSocket connections for subscription methods (`eth_subscribe`/`eth_unsubscribe`) on your full node
- You can use `eth_getLogs` for log filtering instead of filter-based methods
- Please visit [Citrea-Specific RPC Documentation](./citrea-rpc-documentation.md)  for Citrea-specific endpoints