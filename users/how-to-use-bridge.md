
## Welcome to Citrea Devnet!

In this tutorial, we will briefly go over steps for you to get some cBTC's from our Bridge faucet that runs on our custom Bitcoin Signet. This tutorial will enable you to join & use the network. 

{% hint style="warning" %}
Citrea native $BTC is referred to as $cBTC in Citrea. cBTC in Citrea Devnet have **no real value** and they’re only used for testing purposes. Also keep in mind that **there’s no Citrea token**, and please beware of scams.
{% endhint %}

Let's start!

## Step 0: Install an EVM Wallet 

Since Citrea is an EVM-compatible ZK-rollup, you need an EVM-compatible wallet installed in your browser.

You can visit our wallet [guide](install-a-wallet.md) to check some EVM wallets that you can install & use. Following the same page, you can also find a detailed guide on installing an EVM wallet from scratch.

## Step 1: Go to our Bridge page 

![Homepage](/.gitbook/assets/user/1Homepage.png)

Visit our website, [citrea.xyz](https://citrea.xyz/bridge), and click the 🟠Bridge to Citrea🟠 button.

> Citrea is an EVM-compatible ZK-rollup on Bitcoin and uses BTC as its native token. To get started, you need to bridge test BTCs to Citrea Public Devnet. Citrea Public Devnet runs in our custom Bitcoin signet. Therefore, use our bridge website to auto deposit from our faucet to Citrea Devnet.

## Step 2: Connect Your Wallet

Click on the Connect Wallet button, pick the wallet you want to use from the popup, then give necessary permissions to network. 

![Connect Wallet](/.gitbook/assets/user/2Deposit.png)

If you have another network enabled in your wallet, you may see a 🔴Wrong Network🔴 message instead of the blue 🔵Connect Wallet🔵. In that case, click on the Wrong Network button to change your network to Citrea Devnet.

## Step 3: Check your EVM Address

Now, if you've successfully connected your wallet, you should be able to see your address in the text box.

![Wallet Connected](/.gitbook/assets/user/3WalletConnected.png)

## Step 4: Generate a Recovery Taproot Address

It's time to generate a taproot address for recovery. If you want a more technical explanation, you can visit [here](taproot-recovery-address.md).

Now, click on the 🔵Generate New🔵 button to get a system-generated, completely **self-custodial** Recovery Taproot Address.

## Step 5: (Optional) Download Recovery Taproot Address Information

Upon clicking, a popup will be opened with the information regarding your Recovery Taproot Address. Even though funds at Devnet phase have no financial value, you can still download it by clicking the button to familiarize yourself with the process of trustless claiming.

In case you missed the popup, you can generate a new one by clicking again. It's fine.

![Taproot Address Popup](/.gitbook/assets/user/4Popup.png)

## Step 6: Bridge BTCs to Citrea Devnet

![Faucet Page](/.gitbook/assets/user/5FaucetPage.png)

> Here's a summary of important fields you need to know:
> 
> - Unique Deposit Address: You need to send your BTC to this address to get cBTC on Citrea.
>
> - Receiver Address: cBTCs on Citrea will be minted & sent to this EVM address. This is the address that is fetched from your connected EVM wallet.
> 
> - Recovery Taproot Address: In case your deposit gets stuck on the way, you can claim it back using this address after some time (or you can simply create a new deposit in Devnet).

Now, click on the 🟢Faucet Link🟢 button to let the auto deposit mechanism to send 0.01 BTC to the unique deposit address. Then, it will be automatically bridged to the Citrea Devnet. 

The website may ask you to do the verification (or you may be automatically verified in the faucet popup as well).

![Faucet Popup](/.gitbook/assets/user/6FaucetPopup.png)

## Step 7: Check Your Wallet

Congrats! Your deposit transaction will be verified & completed after enough confirmations pass - and that's it! 

![Pending](/.gitbook/assets/user/7Pending.png)
![Estimated](/.gitbook/assets/user/8Estimated.png)
![Completed](/.gitbook/assets/user/9Completed.png)

You can check your balance from your wallet by clicking on it, and start using cBTCs! You can send some funds to your friends, to dApps, or get some more cBTCs!

 If you're interested in developing on Citrea, you're more than welcome to visit our simple development guide from [here](/developer-documentation/deployment-guide/README.md).

![Metamask After Deposit Confirmation](/.gitbook/assets/user/10Metamask.png)

