# Bridge

Serves as a facilitator of deposits and withdrawals between Bitcoin and Citrea. It verifies Bitcoin transactions for deposits and manages withdrawal requests to be processed on the Bitcoin network. 

You can find the contract in the address `0x31000...0002`, [here](https://explorer.devnet.citrea.xyz/address/0x3100000000000000000000000000000000000002).

## Deposit Structure

The following is an explanation regarding the `DepositParams` structure, which is used to represent deposits made to Citrea from the Bitcoin network using [Clementine](https://github.com/chainwayxyz/clementine).

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

To understand more, please check the way deposit params are used along with the `BitcoinLightClient` contract from both our [documentation](./bitcoin-light-client.md) and [explorer](https://explorer.devnet.citrea.xyz/address/0x3100000000000000000000000000000000000001?tab=contract).

## State Structure

```solidity
bool public initialized;
```
A flag indicating whether the contract has been initialized.

---

```solidity
uint256 public constant DEPOSIT_AMOUNT = 0.01 ether;
```
The fixed amount for deposits and withdrawals, set to 0.01 ether (cBTC).

---

```solidity
address public operator;
```
The address of the privileged operator who can process user deposits.

---

```solidity
uint256 public requiredSigsCount;
```
The number of signatures required in the deposit script for verification.

---

```solidity
bytes public depositScript;
```
The expected deposit script for all L1 (Bitcoin) deposits.

---

```solidity
bytes public scriptSuffix;
```
The suffix of the deposit script that follows the receiver address.

---

```solidity
mapping(bytes32 => bool) public spentWtxIds;
```
A mapping to keep track of spent witness transaction IDs to prevent double-spending.

---

```solidity
bytes32[] public withdrawalAddrs;
```
An array to store Bitcoin addresses for withdrawal requests.

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
function initialize(bytes calldata _depositScript, bytes calldata _scriptSuffix, uint256 _requiredSigsCount, address _owner) external onlySystem
```
Initializes the bridge contract and sets the initial deposit script parameters.

| Parameters    | Description |
|---------------|-------------|
| `bytes _depositScript` | The deposit script expected in the witness field for all L1 deposits |
| `bytes _scriptSuffix` | The suffix of the deposit script that follows the receiver address |
| `uint256 _requiredSigsCount` | The number of signatures contained in the deposit script |
| `address _owner` | The address to be set as the owner of the contract |

---

```solidity
function setDepositScript(bytes calldata _depositScript, bytes calldata _scriptSuffix, uint256 _requiredSigsCount) external onlyOwner
```
Updates the expected deposit script parameters.

| Parameters    | Description |
|---------------|-------------|
| `bytes _depositScript` | The new deposit script |
| `bytes _scriptSuffix` | The part of the deposit script that succeeds the receiver address |
| `uint256 _requiredSigsCount` | The number of signatures needed for deposit transaction |

---

```solidity
function deposit(DepositParams calldata p) external onlyOperator
```
Processes a deposit from Bitcoin to Citrea by verifying the Bitcoin transaction and sending configured bridge amount of cBTC to the recipient. Can only be called by the designated operator.

| Parameters    | Description |
|---------------|-------------|
| `DepositParams p` | Struct containing deposit transaction details from Bitcoin |

---

```solidity
function withdraw(bytes32 bitcoin_address) external payable
```
Accepts configured bridge amount of cBTC from the sender and records a withdrawal request to be processed on Bitcoin.

| Parameters    | Description |
|---------------|-------------|
| `bytes32 bitcoin_address` | The Bitcoin address of the receiver |

---

```solidity
function batchWithdraw(bytes32[] calldata bitcoin_addresses) external payable
```
Batch version of `withdraw` that can accept multiple withdrawal requests at once.

| Parameters    | Description |
|---------------|-------------|
| `bytes32[] bitcoin_addresses` | Array of Bitcoin addresses for the receivers |

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
event Deposit(bytes32 wtxId, address recipient, uint256 timestamp)
```
Emitted when a deposit is successfully processed.

| Parameters    | Description |
|---------------|-------------|
| `bytes32 wtxId` | The witness transaction ID of the Bitcoin transaction |
| `address recipient` | The Citrea address receiving the deposited cBTC |
| `uint256 timestamp` | The timestamp of the deposit |

---

```solidity
event Withdrawal(bytes32 bitcoin_address, uint256 index, uint256 timestamp)
```
Emitted when a withdrawal request is recorded.

| Parameters    | Description |
|---------------|-------------|
| `bytes32 bitcoin_address` | The Bitcoin address for the withdrawal |
| `uint256 index` | The index of the withdrawal in the `withdrawalAddrs` array |
| `uint256 timestamp` | The timestamp of the withdrawal request |

---

```solidity
event DepositScriptUpdate(bytes depositScript, bytes scriptSuffix, uint256 requiredSigsCount)
```
Emitted when the deposit script parameters are updated.

| Parameters    | Description |
|---------------|-------------|
| `bytes depositScript` | The new deposit script |
| `bytes scriptSuffix` | The new script suffix |
| `uint256 requiredSigsCount` | The new required signatures count |

---

```solidity
event OperatorUpdated(address oldOperator, address newOperator)
```
Emitted when the operator address is updated.

| Parameters    | Description |
|---------------|-------------|
| `address oldOperator` | The previous operator address |
| `address newOperator` | The new operator address |
