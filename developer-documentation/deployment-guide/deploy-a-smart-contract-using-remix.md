# Deploy a Smart Contract Using Remix

This guide will walk you through deploying a smart contract using [Remix](https://remix.ethereum.org/), a web-based IDE for writing and deploying smart contracts.

Before you begin, make sure that you have cBTCs in your account and Citrea Testnet is added to your wallet. 
If these requirements are not met, follow [the faucet guide](../../users/how-to-use-faucet.md) to get some cBTC and add the network to your wallet.

Note: The images below may say Devnet, but everything also applies for Citrea Testnet as well.

## Step 1: Open Remix

Open [Remix](https://remix.ethereum.org/) in your browser. You should see the following screen:

![Remix-1](/.gitbook/assets/remix/1.png)

Select contracts folder from the left sidebar and click on the `1_Storage.sol` file to open it. This file contains a simple smart contract that stores an integer value on Citrea.

## Step 2: Compile the Smart Contract

Click on the `Solidity Compiler` tab from the left sidebar. You should see something like the following:

![Remix-2](/.gitbook/assets/remix/2.png)

First, click on the advanced configurations section and then select `Shanghai` as the EVM version. After these, click on the `Compile 1_Storage.sol` button to compile the smart contract. Compiling the smart contract will generate the ABI and Bytecode required for deploying the smart contract.

> Normally, it's not necessary to select Shanghai as the EVM version. However, Citrea does not support Cancun update at the moment. 
>
> If you try to deploy a more complex contract using default configs, there might be some opcode issues. That's why we advise you to pick Shanghai version for now.

## Step 3: Open the Deploy & Run Transactions Tab

Click on the `Deploy & run transactions` tab from the left sidebar. You should see the following screen:

![Remix-3](/.gitbook/assets/remix/3.png)

## Step 4: Select Citrea Testnet as the Environment and Deploy the Smart Contract

Click on the `Environment` dropdown and select `Injected Provider - Metamask`. This will automatically connect Remix to your Metamask wallet. Make sure you have selected Citrea Testnet in your Metamask wallet. Click on the `Deploy` button to deploy the smart contract.

![Remix-4](/.gitbook/assets/remix/4.png)

## Step 5: Confirm the Transaction

 Metamask will prompt you to confirm the transaction. Click on the `Confirm` button to deploy the smart contract.

![Remix-5](/.gitbook/assets/remix/5.png)

## Step 6: Interact with the Smart Contract

Once the smart contract is deployed, you can interact with it using the Remix IDE. You can call the `store` function to set the integer value and the `retrieve` function to get the integer value.

![Remix-6](/.gitbook/assets/remix/6.png)


# Congratulations!

You have successfully deployed a smart contract on Citrea using Remix. You can now start building decentralized applications on Citrea. If you have any questions or need help, feel free to ask in the [Citrea Discord](https://discord.gg/citrea).