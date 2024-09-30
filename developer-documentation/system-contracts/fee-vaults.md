# Fee Vaults

Fee vaults are simple contracts that collect the three types of fees in Citrea.

You can find these contracts in the following addresses.

| Fee Vault    | Address |
|---------------|-------------|
| `BaseFeeVault` | [`0x3100000000000000000000000000000000000003`](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000003)|
| `L1FeeVault` | [`0x3100000000000000000000000000000000000004`](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000004)|
| `PriorityFeeVault` | [`0x3100000000000000000000000000000000000005`](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000005)|

Currently all these three contracts have the same logic, yet they can be upgraded in case the need arises to do a programmatic redirection of fees.

## State Structure

```solidity
address public recipient;
```
Address to send the accumulated fees.

---

```solidity
uint256 public minWithdraw;
```
Minimum required fee amount to be accumulated in the contract before it can be withdrawn to the `recipient` address, this is 0 by default.

## Access Control Structure

```solidity
modifier onlyOwner()
```
Ensures that only the contract owner can call the function (inherited from Ownable).

## Functions

```solidity
function withdraw() external
```
Sends the accumulated fees to recipient address.

---

```solidity
function setRecipient(address _recipient) external onlyOwner
```
Changes the recipient address.

| Parameters    | Description |
|---------------|-------------|
| `address _recipient` | Address of the new recipient |

---

```solidity
function setMinWithdraw(uint256 _minWithdraw) external onlyOwner
```
Changes the minimum withdraw amount.

| Parameters    | Description |
|---------------|-------------|
| `address _minWithdraw` | New minimum withdraw amount |

## Events

```solidity
event RecipientUpdated(address oldRecipient, address newRecipient)
```
Emitted when the recipient address is changed.

| Parameters    | Description |
|---------------|-------------|
| `address oldRecipient` | Old recipient address |
| `address newRecipient` | New recipient address |

---

```solidity
event MinWithdrawUpdated(uint256 oldMinWithdraw, uint256 newMinWithdraw)
```
Emitted when the minimum withdraw amount is changed.

| Parameters    | Description |
|---------------|-------------|
| `uint256 oldMinWithdraw` | Old minimum withdraw amount |
| `uint256 newMinWithdraw` | New minimum withdraw amount |
