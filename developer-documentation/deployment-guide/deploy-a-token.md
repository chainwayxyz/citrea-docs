# Deploy a Token

## Deploy a Token

This guide will walk you through deploying a smart contract using OpenZeppelin Smart Contract Wizard, a web-based tool for generating smart contracts, and Remix, a web-based IDE for writing and deploying smart contracts.

Before you begin, make sure that you have cBTCs in your account and Citrea Testnet is added to your wallet. If these requirements are not met, follow [the faucet guide](https://docs.citrea.xyz/essentials/how-to-use-faucet) to get some cBTC and add the network to your wallet.

### Step 1: Open OpenZeppelin Smart Contract Wizard

Open [OpenZeppelin Smart Contract Wizard](https://wizard.openzeppelin.com/) in your browser. You should see the following screen:

![OpenZeppelin-1](https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2Fgit-blob-f0bdee6901eefe404c51cb7358e11e508e46b57d%2F1.png?alt=media)

Select the `ERC20` template from the list of available templates. This template will generate a standard ERC20 token smart contract.

### Step 2: Specify the Token Details

Fill in the token details such as `Name`, `Symbol`, `Decimals`, and `Initial Supply`. You can also specify the `Mintable` and `Burnable` options if you want to allow minting and burning of tokens.

![OpenZeppelin-2](https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2Fgit-blob-5f59908dab3bd844059c74c899f026f54efe967d%2F2.png?alt=media)

### Step 3: Move to the Deployment

Click `Open in Remix` to open the generated smart contract in Remix. After this step, you need to compile your smart contract, select the environment, and deploy it. The rest of the guide is the same as the [Deploy a Smart Contract Using Remix](https://docs.citrea.xyz/developer-documentation/deployment-guide/deploy-a-smart-contract-using-remix) guide, from the step 2.

![OpenZeppelin-3](https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2Fgit-blob-29209b0f1326ecd26b6b5601e27e2d46e3bfd98e%2F3.png?alt=media)

## Congratulations!

You have successfully deployed a token on Citrea using Remix. If you have any questions or need help, feel free to ask in the [Citrea Discord](https://discord.gg/citrea).
