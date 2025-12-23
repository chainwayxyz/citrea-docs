# Deploy a Smart Contract Using Remix

## Deploy a Smart Contract Using Remix

This guide will walk you through deploying a smart contract using [Remix](https://remix.ethereum.org/), a web-based IDE for writing and deploying smart contracts.

Before you begin, make sure that you have cBTCs in your account and Citrea Testnet is added to your wallet. If these requirements are not met, follow [the faucet guide](https://docs.citrea.xyz/essentials/how-to-use-faucet) to get some cBTC and add the network to your wallet.

Note: The images below may say Devnet, but everything also applies to Citrea Testnet as well.

### Step 1: Open Remix

Open [Remix](https://remix.ethereum.org/) in your browser. You should see the following screen:

![Remix-1](https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2Fgit-blob-92484d2488cecd34a999a72937e85a28d2be33f4%2F1.png?alt=media)

Select the contracts folder from the left sidebar and click on the `1_Storage.sol` file to open it. This file contains a simple smart contract that stores an integer value on Citrea.

### Step 2: Compile the Smart Contract

Click on the `Solidity Compiler` tab from the left sidebar. You should see something like the following:

![Remix-2](https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2Fgit-blob-c3a3f3e48e5744f2e83b07fb9c179ada890e8a40%2F2.png?alt=media)

First, click on the advanced configurations section and then select `Shanghai` as the EVM version. After this, click on the `Compile 1_Storage.sol` button to compile the smart contract. Compiling the smart contract will generate the ABI and Bytecode required for deploying the smart contract.

> Normally, it's not necessary to select Shanghai as the EVM version. However, Citrea does not support Cancun update at the moment.
>
> If you try to deploy a more complex contract using default configs, there might be some opcode issues. That's why we advise you to pick Shanghai version for now.

### Step 3: Open the Deploy & Run Transactions Tab

Click on the `Deploy & run transactions` tab from the left sidebar. You should see the following screen:

![Remix-3](https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2Fgit-blob-c51662710a9ea866382cded87279adc2b91997ee%2F3.png?alt=media)

### Step 4: Select Citrea Testnet as the Environment and Deploy the Smart Contract

Click on the `Environment` dropdown and select `Injected Provider - MetaMask`. This will automatically connect Remix to your MetaMask wallet. Make sure you have selected Citrea Testnet in your MetaMask wallet. Click on the `Deploy` button to deploy the smart contract.

![Remix-4](https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2Fgit-blob-9005831a73a3a6489f3d4de695217e41bd56a8e8%2F4.png?alt=media)

### Step 5: Confirm the Transaction

MetaMask will prompt you to confirm the transaction. Click on the `Confirm` button to deploy the smart contract.

![Remix-5](https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2Fgit-blob-b8829a69f7ca7877a6a0e3ba44d26ecd38ebc1e3%2F5.png?alt=media)

### Step 6: Interact with the Smart Contract

Once the smart contract is deployed, you can interact with it using the Remix IDE. You can call the `store` function to set the integer value and the `retrieve` function to get the integer value.

![Remix-6](https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2Fgit-blob-8343c98749bf92c4b7ab7180add869fcd776b57f%2F6.png?alt=media)

## Congratulations!

You have successfully deployed a smart contract on Citrea using Remix. You can now start building decentralized applications on Citrea. If you have any questions or need help, feel free to ask in the [Citrea Discord](https://discord.gg/citrea).
