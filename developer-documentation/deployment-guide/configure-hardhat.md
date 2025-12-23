# Configure Hardhat

## Configure Hardhat

[Hardhat](https://hardhat.org/) is a development environment to compile, deploy, test, and debug your Ethereum software. It helps developers manage the entire development cycle of their smart contracts. Hardhat is a popular choice for developers who want to build, test, and deploy smart contracts on the Ethereum network.

This guide will walk you through configuring Hardhat to work with Citrea. You will learn how to set up Hardhat to deploy smart contracts on Citrea Testnet.

### Hardhat Installation

Before you begin, make sure you have Node.js installed on your machine. If you haven't installed Node.js yet, you can download it from the [official website](https://nodejs.org/).

To install Hardhat, run the following command in your terminal:

```bash
npm install --save-dev hardhat
```

This command will install Hardhat as a development dependency in your project.

### Create a Hardhat Project

To create a new Hardhat project, run the following command in your terminal:

```bash
npx hardhat init
```

Select the default options when prompted. This command will create a new Hardhat project in your current directory.

### Configure Hardhat for Citrea

To configure Hardhat for Citrea, you need to update the `hardhat.config.js` file in your project directory. Open the `hardhat.config.js` file in your code editor and add the following configuration:

```javascript
module.exports = {
    // ...rest of your config
    networks: {
        citrea: {
            url: "https://rpc.testnet.citrea.xyz",
            chainId: 5115,
            accounts: ["YOUR_PRIVATE_KEY"],
        },
    },
    // ...rest of your config
};
```

Replace `YOUR_PRIVATE_KEY` with your private key. Make sure to keep your private key secure and never share it with anyone. You can obtain a private key from your Citrea Testnet wallet (e.g., MetaMask).

### Implement Your Smart Contract and Deploy

For the rest of the development process, you can follow the official Hardhat documentation to compile, deploy, test, and debug your smart contracts. You can find more information on the [Hardhat website](https://hardhat.org/).

Once you have implemented your smart contract, you can deploy it to Citrea Testnet using the following command:

```bash
npx hardhat run --network citrea scripts/deploy.js
```

This command will deploy your smart contract to the Citrea Testnet network. You can interact with your smart contract using the Citrea Testnet explorer or other tools.

## Congratulations!

You have successfully configured Hardhat to work with Citrea. You can now start building and deploying smart contracts on Citrea Testnet. If you have any questions or need help, feel free to ask in the [Citrea Discord](https://discord.gg/citrea).
