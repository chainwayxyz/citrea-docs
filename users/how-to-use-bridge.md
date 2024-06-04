
## Welcome to Citrea Devnet!

In this tutorial, we will briefly go over steps for you to get some cBTC's from out Bridge faucet. This will enable you to join & use the network. 

{% hint style="warning" %}
Citrea is now at the Devnet phase and it has launched for the testing purposes only. In that sense, BTCs and cBTCs that you're going to utilize during this phase (and this tutorial) have **no real value**. Keep in mind that **there's no Citrea token**, and please beware of scams.
{% endhint %}

Let's start!

## Step 0: Install an EVM Wallet 

Since Citrea is an EVM-compatible ZK-rollup, you need an EVM-compatible wallet in installed in your browser.

You can visit our wallet [guide](install-a-wallet.md) to check some EVM wallets that you can install & use, if you do not have any wallets installed. Along with that, if you want a more detailed explanation, follow the same page on installing a wallet (Metamask, as an example) from scratch. 

## Step 1: Go to [citrea.xyz](https://citrea.xyz)

![Homepage](/.gitbook/assets/user/1Homepage.png)

## Step 2: Go to Bridge page

Visit our Bridge page by clicking the 🟠Bridge to Citrea🟠 button.

> Citrea is an EVM-compatible ZK-rollup on Bitcoin. It means that you need BTCs, and you need to bridge them to the Citrea.
> 
>
> Through our faucet & bridge, you will get some Devnet BTCs and they will automatically be deposited to the Citrea Devnet very easily in a few clicks. 


## Step 3: Connect Your Wallet

Click on the Connect Wallet button, pick the wallet you want to use from the popup, then give necessary permissions to network. 

![Connect Wallet](/.gitbook/assets/user/2Deposit.png)

If you had another network enable in your wallet, you may see a 🔴Wrong Network🔴 message in a red button, rather than the blue 🔵Connect Wallet🔵 here. In that case, you can simply click on the button and change your network from the popup very easily.

## Step 4: Check your EVM Address

Now, if you've successfully connected your wallet, you should be able to see your address in the text box.

![Wallet Connected](/.gitbook/assets/user/3WalletConnected.png)

## Step 5: Generate a Recovery Taproot Address

It's time to generate a taproot address for recovery. If you want a more technical explanation, you can visit [here](taproot-recovery-address.md).

Now, click on the 🔵Generate New🔵 button to get a system-generated, completely **self-custodial** Recovery Taproot Address.

## Step 6: (Optional) Download Recovery Taproot Address Information

Upon clicking, a popup will be opened with the information regarding your Recovery Taproot Address. While it's not that necessary for Devnet, you can download it by clicking the button.

In case you missed the popup, you can generate a new one by clicking again. It's fine.

![Taproot Address Popup](/.gitbook/assets/user/4Popup.png)

## Step 7: Bridge BTCs to Citrea Devnet

![Faucet Page](/.gitbook/assets/user/5FaucetPage.png)

> The page above has a couple of important fields for you to familiarize. Let's go over them simply:
> 
> - Unique Deposit Address: You need to send your BTC to this address to be minted.
>
> - Receiver Address: cBTC's on Citrea will be minted to this EVM address. This is the address that is fetched from your connected EVM wallet.
> 
> - Recovery Taproot Address: In case your deposit gets stuck on the way, you can claim it back using this address after some time (or you can simply create a new deposit in Devnet).

Now, we will first send 0.01 BTC to the unique deposit address on your behalf. Then it will be automatically bridged to the Citrea Devnet. 

Let's click on the 🟢Faucet Link🟢 button, and do the verification (you may be automatically verified in the faucet popup as well).

![Faucet Popup](/.gitbook/assets/user/6FaucetPopup.png)

## Step 8: Check Your Wallet

Congrats! Your deposit transaction will be verified & completed after enough confirmations pass - and that's it! 

![Pending](/.gitbook/assets/user/7Pending.png)
![Estimated](/.gitbook/assets/user/8Estimated.png)
![Completed](/.gitbook/assets/user/9Completed.png)

You can check your amount from your wallet by clicking on it, and can start to use it quickly! You can send some funds to your friends, to dApps, or get some more cBTC's and start developing on Citrea! If you're interested in the latter, you're more than welcome to visit our simple development guide from [here](/developer-documentation/deployment-guide/README.md).

![Metamask After Deposit Confirmation](/.gitbook/assets/user/10Metamask.png)

