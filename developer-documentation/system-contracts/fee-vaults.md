# Fee Vaults

Fee vaults collect native cBTC fees and forward them to configured recipients. All vault variants share the same implementation today and differ only by address.

| Vault              | Address                                                                                                                                |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| BaseFeeVault       | [`0x3100000000000000000000000000000000000003`](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000003) |
| L1FeeVault         | [`0x3100000000000000000000000000000000000004`](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000004) |
| PriorityFeeVault   | [`0x3100000000000000000000000000000000000005`](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000005) |
| FailedDepositVault | [`0x3100000000000000000000000000000000000007`](https://explorer.testnet.citrea.xyz/address/0x3100000000000000000000000000000000000007) |

In genesis, the owner of each vault is set to the configured fee vault owner and the initial `recipient` and minimum withdraw values are populated (`0.5 cBTC` by default in deployment scripts). The implementations are upgradeable through the proxy admin so behavior can diverge across vaults if needed later. `FailedDepositVault` follows the same logic and simply serves as the catch-all destination for bridge deposits that could not be delivered to their intended recipients.

<figure><img src="https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2FGLQ2Uo4nnRIicjpCkYqg%2Fimage.png?alt=media&#x26;token=8d9aa22f-24a5-4267-b59f-3e4a0107171a" alt=""><figcaption></figcaption></figure>

### State

```solidity
address public recipient;
uint256 public minWithdraw;
```

The `receive()` hook accepts native cBTC from any sender. Execution nodes credit fees directly to the appropriate vault address: base fees accumulate in `BaseFeeVault`, L1 gas reimbursements accumulate in `L1FeeVault`, and priority tips accumulate in `PriorityFeeVault`. When a bridge deposit transfer fails on the recipient side, the bridge forwards the funds into `FailedDepositVault`. Each vault therefore acts as a passive accumulator until a withdrawal flushes its balance to an operational account.

### Access Control

Ownership is handled through [`Ownable2StepUpgradeable`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/blob/master/contracts/access/Ownable2StepUpgradeable.sol). Only the owner can adjust the recipient or the minimum withdrawal threshold, but anyone can trigger withdrawal once the balance is high enough. That design lets operators regularly sweep fees without exposing owner keys to every caller.

### Functions

```solidity
function withdraw() external
```

`withdraw` sends the entire vault balance to the configured recipient. It reverts if the recipient is unset or if the balance is below `minWithdraw`, so operators typically size `minWithdraw` to avoid dust withdrawals or to amortize L1 payout costs. The transfer uses a low-level call to avoid gas stipends interfering with cBTC delivery. On success the function emits `Withdrawal` with the amount paid out so accounting systems can reconcile accrual versus sweep amounts.

```solidity
function setRecipient(address _recipient) external onlyOwner
```

`setRecipient` rotates the payout address and rejects the zero address to avoid accidental burns. Events include both old and new values to keep monitoring systems in sync.

```solidity
function setMinWithdraw(uint256 _minWithdraw) external onlyOwner
```

`setMinWithdraw` tunes the lower bound for successful withdrawals. Raising the value delays sweeps until more fees accrue; lowering it enables more frequent but smaller payouts. The function emits `MinWithdrawUpdated` so fee collection dashboards can adapt thresholds.

### Events

```solidity
event Withdrawal(address recipient, uint256 amount);
event RecipientUpdated(address oldRecipient, address newRecipient);
event MinWithdrawUpdated(uint256 oldMinWithdraw, uint256 newMinWithdraw);
```

`Withdrawal` gives an on-chain record of every sweep. The update events track governance actions over the vault configuration, which is especially important for anyone following how fee flows are redirected or throttled over time.
