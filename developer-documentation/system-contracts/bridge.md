# Bridge

The Citrea <> Bitcoin bridge contract, utilized by [Clementine](https://docs.citrea.xyz/essentials/clementine-trust-minimized-bitcoin-bridge). It validates Bitcoin deposits using the on-chain light client, mints cBTC on Citrea, and records withdrawal intents so BTC can be paid out on L1 using Clementine.

```solidity
address constant BRIDGE = 0x3100000000000000000000000000000000000002;
```

That proxy at `0x3100…0002` lives on testnet: [explorer link](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000002). The implementation sits at `0x3200…0002`, managed by the proxy admin.

### Dependencies & Constants

```solidity
address constant BITCOIN_LIGHT_CLIENT = 0x3100000000000000000000000000000000000001;
address constant SYSTEM_CALLER = 0xdeaDDeADDEaDdeaDdEAddEADDEAdDeadDEADDEaD;
address constant SCHNORR_PRECOMPILE = 0x0000000000000000000000000000000000000200;
address constant FAILED_DEPOSIT_VAULT_PREDEPLOY = 0x3100000000000000000000000000000000000007;
uint256 constant SAT_TO_WEI = 10**10; // 1 sat = 1e10 wei
uint64 constant PAYOUT_ANCHOR_OUTPUT_AMOUNT = 240; // sats
```

All validation leans on the light client at `0x3100…0001`. Mutations can be triggered by the system caller and, for most flows, by the operator once configured. Schnorr verification delegates to the [precompile at `0x200`](https://docs.citrea.xyz/developer-documentation/schnorr-secp256r1).

If a deposit transfer into cBTC reverts, the contract forwards the amount to a failed deposit vault that defaults to the [FailedDepositFeeVault](https://docs.citrea.xyz/developer-documentation/system-contracts/fee-vaults) at `0x3100…0007`.

Values in Bitcoin are expressed in satoshis; conversions rely on the fixed `SAT_TO_WEI` ratio, and `safeWithdraw` expects a payout anchor of `240` sats to account for Taproot dust.

Because the light client only ingests Bitcoin blocks after they clear Citrea’s configured Bitcoin finality depth, bridge actions that rely on transaction inclusion proofs also experience this delay. A Bitcoin deposit does not become provable to the bridge until its block has reached the required confirmations and been written through `setBlockInfo`. This is a protection against re-orgs; if Bitcoin ever reorged deeper than that buffer, on-chain state could not be rewritten and operators would need to coordinate an out-of-band recovery.

{% hint style="warning" %}
On Citrea Testnet, the buffer is `100` Bitcoin blocks because Testnet4 occasionally sees very deep reorgs; deposits and withdrawals therefore appear on Citrea only after roughly that many confirmations.
{% endhint %}

### Data Structures

The bridge mirrors Clementine’s transaction representation.

| Transaction field   | Explanation                                                                                                                                   |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `version (bytes4)`  | Raw version bytes from the serialized transaction.                                                                                            |
| `flag (bytes2)`     | Marker for Segwit; expected to be `0x0001` for Taproot deposits.                                                                              |
| `vin (bytes)`       | Serialized inputs vector exactly as it appears on-chain.                                                                                      |
| `vout (bytes)`      | Serialized outputs vector, used to locate the deposit output and reconstruct the sighash.                                                     |
| `witness (bytes)`   | Concatenated witness stack for each input; for deposits the first item is the aggregated signature, followed by the script and control block. |
| `locktime (bytes4)` | Serialized `nLockTime`.                                                                                                                       |

| MerkleProof field           | Explanation                                                 |
| --------------------------- | ----------------------------------------------------------- |
| `intermediateNodes (bytes)` | Concatenated 32-byte siblings from the witness Merkle path. |
| `blockHeight (uint256)`     | Height of the Bitcoin block that included the transaction.  |
| `index (uint256)`           | Leaf index within the witness tree.                         |

| UTXO field          | Explanation                                                     |
| ------------------- | --------------------------------------------------------------- |
| `txId (bytes32)`    | *Little-endian* transaction id for the output being referenced. |
| `outputId (bytes4)` | *Little-endian* output index inside that transaction.           |

`shaScriptPubkeys` is an additional parameter for deposit and replace calls. It is the precomputed `sha_scriptpubkeys` Taproot component that stands in for the previous output script during BIP-341 sighash reconstruction, since the transaction itself cannot reveal the prevout script.

### Roles & Pausing

Ownership follows [*Ownable2Step*](https://docs.openzeppelin.com/contracts/4.x/api/access#Ownable2Step). The owner configures scripts, operator, vault, and withdrawal amount, and can pause or unpause the contract. The operator relays deposits and replacement transactions; it starts as the system caller to cover genesis bootstrapping. The system caller can always initialize and call deposit directly. `replaceDeposit` also stays callable while paused so operators can fix deposit records even if user-facing entry points are frozen.

### State Overview

```solidity
bool public initialized;
address public operator;
uint256 public depositAmount;
address public failedDepositVault;
bytes public depositPrefix;
bytes public depositSuffix;
bytes public replacePrefix;
bytes public replaceSuffix;
UTXO[] public withdrawalUTXOs;
bytes32[] public depositTxIds;
mapping(bytes32 => bool) public processedTxIds;
mapping(bytes32 => bool) public usedWithdrawalUTXO;
uint256 public optimisticWithdrawAmountSats;
```

Initialization of the bridge runs once to seed the script parts and `depositAmount`. That amount must be non-zero, convertible to sats without remainder (`depositAmount % SAT_TO_WEI == 0`), and small enough to fit into a `uint64` when expressed in sats so Bitcoin-side logic cannot overflow.

The `failedDepositVault` defaults to the predeploy, but owners can redirect it to a managed recovery contract. Deposit scripts are split into prefix and suffix to make recipient extraction deterministic; replacement scripts follow the same pattern but must embed the exact aggregated key used by the deposit script. Deposits write txids into `depositTxIds`; replacements mutate those entries while `processedTxIds` blocks reuse of any seen txid.

Withdrawals queue `UTXO` pairs, and a hash of each pair in `usedWithdrawalUTXO` prevents duplicate withdrawal intents. `optimisticWithdrawAmountSats` defaults to `depositAmount / SAT_TO_WEI` minus the 240-sat anchor, and `safeWithdraw` enforces it on payout transactions.

### Initialization & Configuration

```solidity
function initialize(bytes calldata depositPrefix, bytes calldata depositSuffix, uint256 depositAmount) external
```

Callable only by the system caller. It seeds the deposit script parts, validates the deposit amount (non-zero, divisible by `SAT_TO_WEI`, representable in `uint64` sats), and requires the prefix to be at least 34 bytes so the aggregated key and opcodes are fully present. The call computes `optimisticWithdrawAmountSats = depositAmount / SAT_TO_WEI - PAYOUT_ANCHOR_OUTPUT_AMOUNT`, sets the `operator` to the system caller, and points `failedDepositVault` to the predeploy. Any second invocation reverts to keep bootstrap deterministic.

```solidity
function setDepositScript(bytes calldata prefix, bytes calldata suffix) external onlyOwner
```

Updates the script template the bridge expects in deposit witness data. The prefix must be at least 34 bytes to cover the aggregated key plus Taproot opcodes. Changing the script effectively rotates the target Musig key or spending policy and should be coordinated with off-chain signers.

```solidity
function setReplaceScript(bytes calldata prefix, bytes calldata suffix) external onlyOwner
```

Sets the script template for replacement transactions. The contract extracts the aggregated key from the prefix and checks it matches the key embedded in the current depositPrefix, preventing a replacement that would be valid for a different signer set.

```solidity
function setFailedDepositVault(address newVault) external onlyOwner
```

Redirects failed deposit transfers to a different vault. Zero addresses are rejected so failed funds cannot be burned accidentally.

```solidity
function setOptimisticWithdrawAmountSats(uint256 amount) external onlyOwner
```

Tunes the payout amount that safeWithdraw expects. This is separate from depositAmount and allows adjustments for fee conditions on Bitcoin without changing the deposit denomination.

```solidity
function setOperator(address newOperator) external onlyOwner
```

Moves the operator role. The new address cannot be zero, and once set it gains access to deposit and replaceDeposit while the system caller retains deposit access.

```solidity
function pause() external
function unpause() external
```

Owner or operator can stop or resume deposit, withdraw, batchWithdraw (and safeWithdraw).&#x20;

### Deposit Flow

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2FvRk9rpcBVCKWxM1qQJZk%2Fimage.png?alt=media&#x26;token=c7233dbc-5bb3-4e54-b5dd-0da9ba1151c4" alt=""><figcaption></figcaption></figure>

```solidity
function deposit(
    Transaction calldata moveTx,
    MerkleProof calldata proof,
    bytes32 shaScriptPubkeys
) external onlySystemOrOperator whenNotPaused
```

The system caller and the operator can invoke deposit. The function begins by decoding the transaction fields, confirming that witness item counts match `vin` entries, computing the wtxid, and proving inclusion through the light client using the provided Merkle proof. It enforces exactly one input for the deposit transaction. The witness must contain three elements: the aggregated Schnorr signature, the Taproot script, and the control block. Using `shaScriptPubkeys`, the bridge reconstructs the [BIP-341](https://en.bitcoin.it/wiki/BIP_0341) sighash and verifies the signature against the aggregated key embedded in `depositPrefix`.

Replay protection relies on `processedTxIds`: any transaction id seen in `deposit` or `replaceDeposit` causes an immediate revert. A fresh txid is appended to `depositTxIds` so off-chain systems can correlate `depositId` with txid history, while the corresponding wtxid remains available in the event for Taproot-aware tooling. The contract then inspects the witness script and requires it to equal `depositPrefix` concatenated with a 20-byte recipient address and `depositSuffix`. The address in the middle is treated as the cBTC recipient.

Finally, the bridge transfers `depositAmount` of cBTC to that recipient. If the transfer reverts-for example because a contract rejects the payment or triggers a revert due to fallback logic-the bridge diverts the funds to the `failedDepositVault` instead. In both outcomes it emits a Deposit-like event so operators can reconcile on-chain balances with Bitcoin deposits.

Each successful deposit’s `depositId` matches its index in `depositTxIds` and is included in the `Deposit` event. Off-chain systems can map `depositId` back to the original Bitcoin txid for proof-of-reserve or auditing purposes.

### Replacing Deposits

```solidity
function replaceDeposit(
    Transaction calldata replaceTx,
    MerkleProof calldata proof,
    uint256 idToReplace,
    bytes32 shaScriptPubkeys
) external onlyOperator
```

Replacement covers scenarios where the original Move transaction must be swapped with a new Clementine transaction. The bridge validates the structure exactly like `deposit`, enforces the single input rule, and verifies the Taproot signature using `shaScriptPubkeys` and the aggregated key. It rejects any txid that has appeared before via `processedTxIds`.

The witness script must equal `replacePrefix` concatenated with the txid being replaced and `replaceSuffix`. That txid is read from `depositTxIds[idToReplace]`; the check ensures a replacement can only target a known deposit id. After validation, the bridge overwrites that array entry with the new txid and emits `DepositReplaced` so observers can link the new on-chain Bitcoin transaction to the existing deposit slot.

### Withdrawals

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2FSPR9ZssOPjYOfenXCu0K%2Fimage.png?alt=media&#x26;token=1a63283e-d60d-4cda-aabd-a21b101446dd" alt=""><figcaption></figcaption></figure>

```solidity
function withdraw(bytes32 txId, bytes4 outputId) public payable whenNotPaused
```

`withdraw` burns or locks exactly `depositAmount` of cBTC (the function reverts unless `msg.value` equals that amount) and records a withdrawal intent tied to a specific UTXO on Bitcoin. The `txId`/`outputId` pair represents the dust input the user will spend in their payout transaction using `SIGHASH_ALL|ANYONECANPAY`. The contract hashes the pair and ensures it has never been used by checking `usedWithdrawalUTXO`, then appends the `UTXO` to `withdrawalUTXOs` and emits `Withdrawal` with the index and timestamp. Once recorded, operators can include the entry when constructing payout PSBTs.

```solidity
function batchWithdraw(bytes32[] calldata txIds, bytes4[] calldata outputIds) external payable whenNotPaused
```

`batchWithdraw` is a vectorized form of `withdraw`. It charges `depositAmount` times the number of provided UTXOs and applies the same uniqueness check to each hash before appending them in order. The event mirrors the single-withdraw path so downstream systems can index both.

```solidity
function safeWithdraw(
    Transaction calldata prepareTx,
    MerkleProof calldata prepareProof,
    Transaction calldata payoutTx,
    bytes calldata blockHeader,
    bytes calldata withdrawalAddressPubKey
) external payable
```

`safeWithdraw` is deliberately heavy but self-contained. The caller supplies a prepare transaction and proof to show inclusion via the light client, using the legacy txid path with the provided `blockHeader`. The payout transaction must have exactly one input and one output, with a valid witness structure, and the output value must equal `optimisticWithdrawAmountSats`. The output script pubkey is checked against `withdrawalAddressPubKey` to ensure the funds are headed to the claimed address.

The function confirms the payout input spends `prepareTx` and that the spent output is P2TR - it extracts the Taproot key, rebuilds the `SIGHASH_SINGLE|ANYONECANPAY` digest, and verifies the Schnorr signature through the precompile. On success it emits `SafeWithdrawal` and internally calls `withdraw` with the spent txid and index, enforcing the same uniqueness and `depositAmount` payment.

```solidity
function getWithdrawalCount() external view returns (uint256)
```

Returns the length of `withdrawalUTXOs` so clients can iterate or poll without fetching the full array.

### Helpers & Utilities

```solidity
function getAggregatedKey() external view returns (bytes32)
```

Extracts the aggregated [MuSig](https://bitcoinops.org/en/topics/musig/) key from the current `depositPrefix` so off-chain tooling can confirm which signer set is active. Internally the bridge also relies on byte comparison helpers and a `taggedHash` helper to mirror Taproot hashing. These do not expose new state but are essential for reconstructing sighashes faithfully.

### Events

```solidity
event Deposit(bytes32 wtxId, bytes32 txId, address recipient, uint256 timestamp, uint256 depositId);
event DepositTransferFailed(bytes32 wtxId, bytes32 txId, address recipient, uint256 timestamp, uint256 depositId);
event DepositReplaced(uint256 index, bytes32 oldTxId, bytes32 newTxId);
event Withdrawal(UTXO utxo, uint256 index, uint256 timestamp);
event SafeWithdrawal(Transaction payoutTx, UTXO spentUtxo, uint256 index);
event DepositScriptUpdate(bytes depositPrefix, bytes depositSuffix);
event ReplaceScriptUpdate(bytes replacePrefix, bytes replaceSuffix);
event FailedDepositVaultUpdated(address oldVault, address newVault);
event OptimisticWithdrawAmountSet(uint256 amount);
event OperatorUpdated(address oldOperator, address newOperator);
```

`Deposit` and `DepositTransferFailed` include both the wtxid and the legacy txid alongside the `depositId`, making it easy to correlate Taproot-level proof material with the bridge’s internal slot. `DepositReplaced` records the exact txid swap so watchers can audit history. `Withdrawal` and `SafeWithdrawal` surface the full `UTXO` or payout transaction involved, letting off-chain agents reconstruct the exact intent that was accepted. Script update events publish the current template, while `FailedDepositVaultUpdated`, `OptimisticWithdrawAmountSet`, and `OperatorUpdated` give an authoritative trail for configuration changes.
