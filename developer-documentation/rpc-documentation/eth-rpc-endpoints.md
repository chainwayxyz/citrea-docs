# Ethereum JSON-RPC Endpoints

Complete reference for Ethereum-compatible JSON-RPC API endpoints supported by Citrea. These endpoints follow the standard Ethereum JSON-RPC specification and provide compatibility with existing Ethereum tooling and libraries.

## Base URLs

- **Local node:** `http://0.0.0.0:8080`
- **Public Testnet:** `https://rpc.testnet.citrea.xyz`

> **Quick Start**: You can immediately start using these endpoints with a full node or the Testnet RPC (`https://rpc.testnet.citrea.xyz`). No setup required!

---

## Supported Ethereum JSON-RPC Methods

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

- **`eth_maxPriorityFeePerGas`** - Returns suggested priority fee (tip) per gas

- **`eth_maxFeePerGas`** - Returns estimated maximum fee per gas

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

- **[`eth_getBlockByNumber`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_getblockbynumber)** - Returns block by number

- **[`eth_getUncleByBlockHashAndIndex`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_getunclebyblockhashAndindex)** - Returns null (no uncles in Citrea)

#### Transaction Methods

- **[`eth_sendRawTransaction`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_sendrawtransaction)** - Submits a signed transaction

- **[`eth_getTransactionByHash`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_gettransactionbyhash)** - Returns transaction details by hash

- **[`eth_getTransactionByBlockHashAndIndex`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_gettransactionbyblockhashAndindex)** - Returns transaction by block hash and index

- **[`eth_getTransactionByBlockNumberAndIndex`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_gettransactionbyblocknumberandindex)** - Returns transaction by block number and index

- **[`eth_getTransactionReceipt`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_gettransactionreceipt)** - Returns transaction receipt by hash

#### Call & Execution

- **[`eth_call`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_call)** - Executes a read-only call

- **[`eth_estimateGas`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_estimategas)** - Returns gas estimate for a transaction

#### Logs

- **[`eth_getLogs`](https://ethereum.org/en/developers/docs/apis/json-rpc#eth_getlogs)** - Returns logs matching filter criteria

---

## Additional Ethereum Extensions

The following methods are implemented in Citrea but are extensions beyond the standard Ethereum JSON-RPC specification:

### State Proofs & Access Lists

- **[`eth_getProof`](https://docs.blastapi.io/blast-documentation/apis-documentation/core-api/ethereum/eth_getproof)** - Returns account and storage proof (EIP-1186)

- **[`eth_createAccessList`](https://geth.ethereum.org/docs/interacting-with-geth/rpc/ns-eth#eth-createaccesslist)** - Generates access list for transaction (EIP-2930)

### Bulk Operations

- **`eth_getBlockReceipts`** - Returns all transaction receipts for a block

### Citrea-Specific Extensions

- **[`eth_estimateDiffSize`](./citrea-rpc-documentation.md#eth_estimatediffsize)** - Estimates L1 data diff size for rollup transaction

### Subscription Methods (WebSocket only)

- **[`eth_subscribe`](https://docs.blastapi.io/blast-documentation/apis-documentation/core-api/ethereum/eth_subscribe)** - Creates subscription for events

- **[`eth_unsubscribe`](https://docs.blastapi.io/blast-documentation/apis-documentation/core-api/ethereum/eth_unsubscribe)** - Cancels active subscription

---

## Notes

- All implemented methods follow the standard Ethereum JSON-RPC specification format
- Use WebSocket connections for subscription methods (`eth_subscribe`/`eth_unsubscribe`)
- Use `eth_getLogs` for log filtering instead of filter-based methods
- Uncle block methods always return null as Citrea doesn't produce uncle blocks
- Mining-related methods are not applicable to Citrea as it's a rollup
- Please visit [Citrea-Specific RPC Documentation](./citrea-rpc-documentation.md)  for Citrea-specific endpoints