## Build an OP Stack L3 on Citrea

In this guide, we will go over deploying a OP Stack L3 on Citrea that utilizes Celestia as DA. Such layers grant access to cBTC while reducing the costs directly.

The following steps are appropriately modified from the [Optimism documentation](https://docs.optimism.io/operators/chain-operators/tutorials/create-l2-rollup#deploy-the-l1-contracts).

### Prerequisites

- [Docker](https://docs.docker.com/engine/install/) for Celestia Light Node
- [Git](https://git-scm.com/)
- [Go](https://go.dev)
- [Foundry](https://getfoundry.sh/) for contract deployment

We recommend to open a separate folder in your filesystem for the rest of this guide.

#### Step 1: Install Celestia Light Node and Run Light Node

Follow the steps [here](https://docs.celestia.org/how-to-guides/celestia-node) to install Celestia Light Node. You may install the latest version.

Then, run the following to start a Celestia Mocha Light Node:

```bash
celestia light init --p2p.network mocha
celestia light start --core.ip rpc-mocha.pops.one --p2p.network mocha
```

#### Step 2: Start DA Server

Let's clone the `op-plasma-celestia` repository first and build the `da-server`:

```bash
git clone https://github.com/celestiaorg/op-plasma-celestia
cd op-plasma-celestia

make da-server
```

This will form the `da-server` binary in the `bin` folder.

Then, let's create a namespace:

```bash
export NAMESPACE=00000000000000000000000000000000000000$(openssl rand -hex 10)
```

Lastly, let's get the Celestia auth token for the light node and put it into `AUTH_TOKEN` env variable to start the DA server:

```bash
export AUTH_TOKEN=$(celestia light auth admin --p2p.network mocha)

./da-server --generic-commitment=true --celestia.server=http://localhost:26658 --celestia.auth-token=$AUTH_TOKEN --celestia.namespace=$NAMESPACE --addr=127.0.0.1 --port=3100
```

This will start the DA server.

#### Step 3: Setup Optimism repo and configure environment

Clone the Optimism repostiory and checkout to `v1.9.2` for development purposes:

```bash
git clone https://github.com/ethereum-optimism/optimism.git
cd optimism
git checkout v1.9.2
```

Then, let's setup our environment using `.envrc` file:

```bash
##################################################
#                 Getting Started                #
##################################################
# Admin account
export GS_ADMIN_ADDRESS=[YOUR_CITREA_ADDRESS]
export GS_ADMIN_PRIVATE_KEY=[YOUR_CITREA_PRIVATE_KEY]

# Batcher account
export GS_BATCHER_ADDRESS=[YOUR_CITREA_ADDRESS]
export GS_BATCHER_PRIVATE_KEY=[YOUR_CITREA_PRIVATE_KEY]

# Proposer account
export GS_PROPOSER_ADDRESS=[YOUR_CITREA_ADDRESS]
export GS_PROPOSER_PRIVATE_KEY=[YOUR_CITREA_PRIVATE_KEY]

# Sequencer account
export GS_SEQUENCER_ADDRESS=[YOUR_CITREA_ADDRESS]
export GS_SEQUENCER_PRIVATE_KEY=[YOUR_CITREA_PRIVATE_KEY]
##################################################
#              op-node Configuration             #
##################################################

# The kind of RPC provider, used to inform optimal transactions receipts
# fetching. Valid options: alchemy, quicknode, infura, parity, nethermind,
# debug_geth, erigon, basic, any.
export L1_RPC_KIND=any

##################################################
#               Contract Deployment              #
##################################################

# RPC URL for the L1 network to interact with
export L1_RPC_URL=https://rpc.testnet.citrea.xyz

# Salt used via CREATE2 to determine implementation addresses
# NOTE: If you want to deploy contracts from scratch you MUST reload this
#       variable to ensure the salt is regenerated and the contracts are
#       deployed to new addresses (otherwise deployment will fail)
export IMPL_SALT=$(openssl rand -hex 32)

# Name for the deployed network
export DEPLOYMENT_CONTEXT=getting-started

# Optional Tenderly details for simulation link during deployment
export TENDERLY_PROJECT=
export TENDERLY_USERNAME=

# Optional Etherscan API key for contract verification
export ETHERSCAN_API_KEY=

# Private key to use for contract deployments, you don't need to worry about
# this for the Getting Started guide.
export PRIVATE_KEY=
export L1_CHAIN_ID=5115
export L2_CHAIN_ID=511551155115
export L1_BLOCK_TIME=2
export L2_BLOCK_TIME=2
```

Then move to `packages/contracts-bedrock` folder for L3 contract configs, and run the script:

```bash
cd packages/contracts-bedrock
./scripts/getting-started/config.sh
```

Later in the same folder, let's run the deployment script using `forge`:

```bash
forge install
DEPLOY_CONFIG_PATH=./deploy-config/getting-started.json forge script scripts/deploy/Deploy.s.sol:Deploy --private-key $GS_ADMIN_PRIVATE_KEY --broadcast --rpc-url $L1_RPC_URL
```

And also run the L2 genesis configuration script:

```bash
DEPLOY_CONFIG_PATH=./deploy-config/getting-started.json CONTRACT_ADDRESSES_PATH=./deployments/5115-deploy.json forge script scripts/L2Genesis.s.sol:L2Genesis --sig "runWithStateDump()"
```



