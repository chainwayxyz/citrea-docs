# Bridge

Serves as a facilitator of deposits and withdrawals between Bitcoin and Citrea. It verifies Bitcoin transactions for deposits and manages withdrawal requests to be processed on the Bitcoin network. 

You can find the contract in the address `0x31000...0002`, [here](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000002).

## Deposit Structure

The following is an explanation regarding the `TransactionParams` structure, which is used to represent deposits made to Citrea from the Bitcoin network using [Clementine](https://github.com/chainwayxyz/clementine).

| Parameters    | Description |
|---------------|-------------|
| `version`            | Version field of a transaction                      |
| `flag`               | Marker + Flag of a segregated witness transaction   |
| `vin`                | Transaction inputs                                  |
| `vout`               | Transaction outputs                                 |
| `witness`            | Transaction witness                                 |
| `locktime`           | Transaction locktime                                |
| `intermediate_nodes` | The proof's intermediate nodes                      |
| `block_height`       | Block Height                                        |
| `index`              | The leaf's index in the tree (0-indexed)            |

To understand more, please check the way deposit params are used along with the `BitcoinLightClient` contract from both our [documentation](./bitcoin-light-client.md) and [explorer](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000001?tab=contract).

## State Structure

```solidity
bool public initialized;
```
A flag indicating whether the contract has been initialized.

---

```solidity
address public operator;
```
The address of the privileged operator who can process user deposits.

---

```solidity
bool[1000] public isOperatorMalicious;
```
A list of values indicating if a operator is marked as malicious.

---

```solidity
uint256 public depositAmount;
```
The fixed amount for deposits and withdrawals, set to 10 ether (cBTC) for Citrea Testnet.

---

```solidity
uint256 currentDepositId;
```
Index of the latest processed deposit.

---

```solidity
bytes public scriptPrefix;
```
The prefix part of deposit script for all L1 (Bitcoin) deposits.

---

```solidity
bytes public scriptSuffix;
```
The suffix part of the deposit script that follows the receiver address.

---

```solidity
bytes public slashOrTakeScript;
```
Script that is used for "slash or take" transactions on Bitcoin. These transactions are sent after an operator proves that they covered a withdrawal through `declareWithdrawFiller` and it is used as a part of the process that enables the operator to get back the amount they covered from N-of-N multisig on Bitcoin.

---

```solidity
UTXO[] public withdrawalUTXOs;
```
An array to store `ANYONE_CAN_PAY` UTXOs on Bitcoin that are utilized as withdraw intents that are covered by operators later on. This stored UTXO information is used to prove that an operator covered a certain withdrawal through `declareWithdrawFiller`.

---

```solidity
mapping(bytes32 => uint256) public txIdToDepositId;
```
A mapping that maps the transaction IDs of deposit transactions on Bitcoin to their deposit IDs on Citrea side.

---

```solidity
mapping(uint256 => uint256) public withdrawFillers;
```
A mapping that maps the withdraw ID to operator ID that covered the withdrawal with that withdraw ID.

## Access Control Structure

```solidity
modifier onlySystem()
```
Ensures that only the system caller (`0xdeaDDeADDEaDdeaDdEAddEADDEAdDeadDEADDEaD`) can call the function.

---

```solidity
modifier onlyOperator()
```
Ensures that only the designated operator can call the function.

---

```solidity
modifier onlyOwner()
```
Ensures that only the contract owner can call the function (inherited from Ownable).

## Functions

```solidity
function initialize(bytes calldata _scriptPrefix, bytes calldata _scriptSuffix, uint256 _depositAmount) external onlySystem
```
Initializes the bridge contract and sets the initial deposit script parameters.

| Parameters    | Description |
|---------------|-------------|
| `bytes _scriptPrefix` | The prefix part of deposit script for all L1 (Bitcoin) deposits. |
| `bytes _scriptSuffix` | The suffix part of the deposit script that follows the receiver address |
| `uint256 _depositAmount` | The fixed amount for deposits and withdrawals, set to 10 ether (cBTC) for Citrea Testnet. |

---

```solidity
function setDepositScript(bytes calldata _scriptPrefix, bytes calldata _scriptSuffix) external onlyOwner
```
Updates the expected deposit script parameters.

| Parameters    | Description |
|---------------|-------------|
| `bytes _scriptPrefix` | The prefix part of new deposit script for all L1 (Bitcoin) deposits. |
| `bytes _scriptSuffix` | The suffix part of the new deposit script that follows the receiver address. |

---

```solidity
function setSlashOrTakeScript(bytes calldata _slashOrTakeScript) external onlyOwner
```
Updates the expected slash or take script. See `slashOrTakeScript` in `State Structure` for detailed information.

| Parameters    | Description |
|---------------|-------------|
| `bytes _slashOrTakeScript` | New expected slash or take script. |

---

```solidity
function deposit(TransactionParams calldata moveTp) external onlyOperator
```
Processes a deposit from Bitcoin to Citrea by verifying the Bitcoin transaction and sending configured bridge amount of cBTC to the recipient. Can only be called by the designated operator.

| Parameters    | Description |
|---------------|-------------|
| `TransactionParams moveTp` | Struct containing deposit transaction details from Bitcoin |

---

```solidity
function withdraw(bytes32 txId, bytes4 outputId) external payable
```
Accepts configured bridge amount of cBTC from the sender and records a withdrawal request to be processed on Bitcoin. This recording is done by storing a UTXO consisting of a `txId` and an `outputId` which is the input for the `ANYONE_CAN_PAY` withdraw transaction on Bitcoin.

| Parameters    | Description |
|---------------|-------------|
| `bytes32 txId` | Transaction ID for the input of the withdraw transaction on Bitcoin |
| `bytes4 outputId` | Output index for the input of the withdraw transaction on Bitcoin |


---

```solidity
function batchWithdraw(bytes32[] calldata txIds, bytes4[] calldata outputIds) external payable
```
Batch version of `withdraw` that can accept multiple withdrawal requests at once.

| Parameters    | Description |
|---------------|-------------|
| `bytes32[] txIds` | Array of transaction IDs for the inputs of the withdraw transactions on Bitcoin |
| `bytes4[] outputIds` | Array of output indexes for the inputs of the withdraw transactions on Bitcoin |

---

```solidity
function getWithdrawalCount() external view returns (uint256)
```
Returns the total number of withdrawal requests made so far.

| Returns    | Description |
|------------|-------------|
| `uint256` | The count of withdrawals happened so far |

---

```solidity
function setOperator(address _operator) external onlyOwner
```
Sets the operator address that can process user deposits.

| Parameters    | Description |
|---------------|-------------|
| `address _operator` | Address of the privileged operator |

---

```solidity
function declareWithdrawFiller(TransactionParams calldata withdrawTp, uint256 inputIndex, uint256 withdrawId) external
```
Stores which operator filled a certain withdraw request. Operators should make sure this function is called before sending a "slash or take" transaction on Bitcoin to avoid being marked as malicious.

| Parameters    | Description |
|---------------|-------------|
| `TransactionParams withdrawTp` | Struct containing withdraw transaction details from Bitcoin |
| `uint256 inputIndex` | Input of the withdraw transaction that corresponds to the stored UTXO for the withdraw request denoted with `withdrawId` |
| `uint256 withdrawId` | The ID of stored withdraw request |

---

```solidity
function markMaliciousOperator(TransactionParams calldata slashOrTakeTp) external
```
Marks an operator as malicious if they sent a "slash or take" transaction on Bitcoin without first proving that they covered a withdrawal through `declareWithdrawFiller`. This information is later on used as evidence in the process of BitVM challenges on the Bitcoin side to prevent the operator stealing user funds from N-of-N multisig, as not being able to successfully call `declareWithdrawFiller` indicates that the operator is trying to claim funds from multisig even though they didn't cover a withdrawal request.

| Parameters    | Description |
|---------------|-------------|
| `TransactionParams slashOrTakeTp` | Struct containing slash or take transaction details from Bitcoin |

---

```solidity
function validateAndCheckInclusion(TransactionParams calldata tp) internal view returns (bytes32, uint256)
```
Reverts if the provided transaction is not properly structured or not an actual Bitcoin transaction that happened according to `BitcoinLightClient`.

| Parameters    | Description |
|---------------|-------------|
| `TransactionParams tp` | Struct containing transaction details from Bitcoin |

| Returns    | Description |
|------------|-------------|
| `bytes32` | Witness transaction ID of the provided transaction |
| `uint256` | Number of inputs of the provided transaction |

---

```solidity
function extractRecipientAddress(bytes memory _script) internal view returns (address)
```
Extracts the recipient from a given script.

| Parameters    | Description |
|---------------|-------------|
| `bytes memory _script` | The script to be processed |

| Returns    | Description |
|------------|-------------|
| `address` | The recipient |

---


## Events

```solidity
event Deposit(bytes32 wtxId, bytes32 txId, address recipient, uint256 timestamp, uint256 depositId)
```
Emitted when a deposit is successfully processed.

| Parameters    | Description |
|---------------|-------------|
| `bytes32 wtxId` | The witness transaction ID of the Bitcoin transaction |
| `bytes32 txId` | The transaction ID of the Bitcoin transaction |
| `address recipient` | The Citrea address receiving the deposited cBTC |
| `uint256 timestamp` | The timestamp of the deposit |
| `uint256 depositId` | The ID of the deposit |

---

```solidity
event Withdrawal(UTXO utxo, uint256 index, uint256 timestamp)
```
Emitted when a withdrawal request is recorded.

| Parameters    | Description |
|---------------|-------------|
| `UTXO utxo` | UTXO to store for the withdrawal request |
| `uint256 index` | The index of the withdrawal in the `withdrawalUTXOs` array |
| `uint256 timestamp` | The timestamp of the `withdraw` call |

---

```solidity
event DepositScriptUpdate(bytes scriptPrefix, bytes scriptSuffix)
```
Emitted when the deposit script parameters are updated.

| Parameters    | Description |
|---------------|-------------|
| `bytes scriptPrefix` | The new script prefix |
| `bytes scriptSuffix` | The new script suffix |

---

```solidity
event OperatorUpdated(address oldOperator, address newOperator)
```
Emitted when the operator address is updated.

| Parameters    | Description |
|---------------|-------------|
| `address oldOperator` | The previous operator address |
| `address newOperator` | The new operator address |

---

```solidity
event WithdrawFillerDeclared(uint256 withdrawId, uint256 withdrawFillerId)
```
Emitted when the operator filled a withdrawal request is declared.

| Parameters    | Description |
|---------------|-------------|
| `uint256 withdrawId` | The withdraw ID |
| `uint256 withdrawFillerId` | ID of the operator that filled the withdrawal request |

---

```solidity
event MaliciousOperatorMarked(uint256 operatorId)
```
Emitted when an operator is marked as malicious.

| Parameters    | Description |
|---------------|-------------|
| `uint256 operatorId` | ID of the operator that is marked as malicious |

---

```solidity
event SlashOrTakeScriptUpdate(bytes slashOrTakeScript)
```
Emitted when slash or take script is updated.

| Parameters    | Description |
|---------------|-------------|
| `bytes slashOrTakeScript` | New expected slash or take script |