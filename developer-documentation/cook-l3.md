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

##### Step 1.1 Install Celestia Light Node
Follow the steps [here](https://docs.celestia.org/how-to-guides/celestia-node) to install Celestia Light Node. You may install the latest version.

##### Step 1.2 Install Celestia Light Node
Run the following to start a Celestia Mocha Light Node:

```bash
celestia light init --p2p.network mocha
celestia light start --core.ip rpc-mocha.pops.one --p2p.network mocha
```

#### Step 2: DA Server Setup

##### Step 2.1 Build DA Server
Let's clone the `op-plasma-celestia` repository first and build the `da-server`:

```bash
git clone https://github.com/celestiaorg/op-plasma-celestia
cd op-plasma-celestia

make da-server
```

This will form the `da-server` binary in the `bin` folder.

##### Step 2.2 Run DA Server
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

#### Step 3: Setup Optimism repositories and configure environment

##### Step 3.1: Clone repostiories
Clone `op-geth` and Optimism mono-repostiory, then checkout to `v1.9.2` for development purposes:

```bash
git clone https://github.com/ethereum-optimism/op-geth.git

git clone https://github.com/ethereum-optimism/optimism.git
cd optimism
git checkout v1.9.2
```

##### Step 3.2: Setup environment
Then, let's setup our environment using `.envrc` file (alternatively you can set them in your shell):

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

##### Step 3.3: Run configuration scripts
Move to `packages/contracts-bedrock` folder for configs, and run the script:

```bash
cd packages/contracts-bedrock
./scripts/getting-started/config.sh
```

Later in the same folder, let's run the deployment script using `forge` to deploy the necessary contracts on Citrea:

```bash
forge install

DEPLOY_CONFIG_PATH=./deploy-config/getting-started.json forge script scripts/deploy/Deploy.s.sol:Deploy --private-key $GS_ADMIN_PRIVATE_KEY --broadcast --rpc-url $L1_RPC_URL
```

And also run the L3 genesis configuration script:

```bash
DEPLOY_CONFIG_PATH=./deploy-config/getting-started.json CONTRACT_ADDRESSES_PATH=./deployments/5115-deploy.json forge script scripts/L2Genesis.s.sol:L2Genesis --sig "runWithStateDump()"
```

#### Step 4: Generate node configurations

Navigate to the `op-node` and generate the configuration for the L3 node:

```bash
cd ~/optimism/op-node

go run cmd/main.go genesis l2   --deploy-config ../packages/contracts-bedrock/deploy-config/getting-started.json   --l1-deployments ../packages/contracts-bedrock/deployments/5115-deploy.json   --outfile.l2 genesis.json --l2-allocs ../packages/contracts-bedrock/state-dump-511551155115-fjord.json --outfile.rollup rollup.json   --l1-rpc $L1_RPC_URL

openssl rand -hex 32 > jwt.txt
cp genesis.json ~/op-geth
cp jwt.txt ~/op-geth
```

#### Step 5: Initialize and start op-geth

Navigate to the `op-geth` folder and initialize the genesis config:

```bash
cd ~/op-geth

mkdir datadir
make geth
build/bin/geth init --state.scheme=hash --datadir=datadir genesis.json
```

Then, start the `op-geth`:

```bash
./build/bin/geth   --datadir ./datadir   --http   --http.corsdomain="*"   --http.vhosts="*"   --http.addr=0.0.0.0   --http.api=web3,debug,eth,txpool,net,engine   --ws   --ws.addr=0.0.0.0   --ws.port=8546   --ws.origins="*"   --ws.api=debug,eth,txpool,net,engine   --syncmode=full   --gcmode=archive   --nodiscover   --maxpeers=0   --networkid=511551155115   --authrpc.vhosts="*"   --authrpc.addr=0.0.0.0   --authrpc.port=8551   --authrpc.jwtsecret=./jwt.txt   --rollup.disabletxpoolgossip=true
```

#### Step 6: Add DA-config to op-node

Navigate to `op-node` folder and add the following to the `rollup.json`:

```json
  "alt_da": {
    "da_commitment_type": "GenericCommitment",
    "da_challenge_contract_address": "0x0000000000000000000000000000000000000000",
    "da_challenge_window": 1000,
    "da_resolve_window": 2000
  },
```

#### Step 7: Run op-node

Build and run `op-node`:

```bash
make op-node

cd ~/optimism/op-node

./bin/op-node   --l2=http://localhost:8551   --l2.jwt-secret=./jwt.txt   --sequencer.enabled   --sequencer.l1-confs=5   --verifier.l1-confs=4   --rollup.config=./rollup.json   --rpc.addr=0.0.0.0   --p2p.disable   --rpc.enable-admin   --p2p.sequencer.key=$GS_SEQUENCER_PRIVATE_KEY   --l1=$L1_RPC_URL   --l1.rpckind=$L1_RPC_KIND --altda.enabled=true --altda.da-service=true --l1.beacon=$L1_RPC_URL --l1.beacon.ignore=true --altda.da-server=http://localhost:3100 --l1.trustrpc=true --l1.http-poll-interval=2s
```

#### Step 8: Run op-batcher

```bash
make op-batcher
./bin/op-batcher   --l2-eth-rpc=http://localhost:8545   --rollup-rpc=http://localhost:9545   --poll-interval=1s   --sub-safety-margin=6   --num-confirmations=1   --safe-abort-nonce-too-low-count=3   --resubmission-timeout=30s   --rpc.addr=0.0.0.0   --rpc.port=8548   --rpc.enable-admin   --max-channel-duration=25   --l1-eth-rpc=$L1_RPC_URL   --private-key=$GS_BATCHER_PRIVATE_KEY --altda.enabled=true --altda.da-service=true --altda.da-server=http://localhost:3100
```

#### Step 9: Run op-proposer

Navigate to `op-proposer` folder and run the proposer:

```bash
make op-proposer

cd ~/optimism/op-proposer

./bin/op-proposer   --poll-interval=2s   --rpc.port=8560   --rollup-rpc=http://localhost:9545   --l2oo-address=$(cat ../packages/contracts-bedrock/deployments/5115-deploy.json | jq -r .L2OutputOracleProxy) --private-key=$GS_PROPOSER_PRIVATE_KEY   --l1-eth-rpc=$L1_RPC_URL
```

-----

That's it, enjoy your L3 on Citrea!