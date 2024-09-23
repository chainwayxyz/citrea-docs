# Deploy a Token

This guide will walk you through deploying a smart contract using OpenZeppelin Smart Contract Wizard, a web-based tool for generating smart contracts, and Remix, a web-based IDE for writing and deploying smart contracts.

Before you begin, make sure that you have cBTCs in your account and Citrea Testnet is added to your wallet. 
If these requirements are not met, follow [the bridge guide](../../users/how-to-use-bridge.md) to get some cBTC and add the network to your wallet.

## Step 1: Open OpenZeppelin Smart Contract Wizard

Open [OpenZeppelin Smart Contract Wizard](https://wizard.openzeppelin.com/) in your browser. You should see the following screen:

![OpenZeppelin-1](/.gitbook/assets/token/1.png)

Select the `ERC20` template from the list of available templates. This template will generate a standard ERC20 token smart contract.

## Step 2: Specify the Token Details

Fill in the token details such as `Name`, `Symbol`, `Decimals`, and `Initial Supply`. You can also specify the `Mintable` and `Burnable` options if you want to allow minting and burning of tokens.

![OpenZeppelin-2](/.gitbook/assets/token/2.png)

## Step 3: Move to the Deployment

Click `Open in Remix` to open the generated smart contract in Remix. After this step, you need to compile your smart contract, select the environment, and deploy it. The rest of the guide is the same as the [Deploy a Smart Contract Using Remix](deploy-a-smart-contract-using-remix.md) guide, from the step 2.

![OpenZeppelin-3](/.gitbook/assets/token/3.png)

# Congratulations!

You have successfully deployed a token on Citrea using Remix. If you have any questions or need help, feel free to ask in the [Citrea Discord](https://discord.gg/citrea).