# Deploy a Smart Contract Using Remix

This guide will walk you through deploying a smart contract using [Remix](https://remix.ethereum.org/), a web-based IDE for writing and deploying smart contracts.

Before you begin, make sure you did the following:
* Add Citrea Devnet to your Metamask wallet. If you haven't done this yet, follow the [guide](TBD).
* Deposit some test BTC to your Citrea Devnet wallet. If you haven't done this yet, follow the [guide](TBD).

## Step 1: Open Remix

Open [Remix](https://remix.ethereum.org/) in your browser. You should see the following screen:

![Remix-1](/.gitbook/assets/remix/1.png)

Select contracts folder from the left sidebar and click on the `1_Storage.sol` file to open it. This file contains a simple smart contract that stores an integer value on Citrea.

## Step 2: Compile the Smart Contract

Click on the `Solidity Compiler` tab from the left sidebar. You should see the following screen:

![Remix-2](/.gitbook/assets/remix/2.png)

Click on the `Compile 1_Storage.sol` button to compile the smart contract. Compiling the smart contract will generate the ABI and Bytecode required for deploying the smart contract.

## Step 3: Open the Deploy & Run Transactions Tab

Click on the `Deploy & run transactions` tab from the left sidebar. You should see the following screen:

![Remix-3](/.gitbook/assets/remix/3.png)

## Step 4: Select Citrea Devnet as the Environment and Deploy the Smart Contract

Click on the `Environment` dropdown and select `Injected Provider - Metamask`. This will automatically connect Remix to your Metamask wallet. Make sure you have selected Citrea Devnet in your Metamask wallet. Click on the `Deploy` button to deploy the smart contract.

![Remix-4](/.gitbook/assets/remix/4.png)

## Step 5: Confitm the Transaction

 Metamask will prompt you to confirm the transaction. Click on the `Confirm` button to deploy the smart contract.

![Remix-5](/.gitbook/assets/remix/5.png)

## Step 6: Interact with the Smart Contract

Once the smart contract is deployed, you can interact with it using the Remix IDE. You can call the `store` function to set the integer value and the `retrieve` function to get the integer value.

![Remix-6](/.gitbook/assets/remix/6.png)