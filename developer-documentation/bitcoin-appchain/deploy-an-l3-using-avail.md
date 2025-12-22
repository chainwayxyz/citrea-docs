# Deploy an L3 using Avail

In this guide, we will walk through the process of deploying a Bitcoin Appchain (L3) on Citrea using the OP Stack, with [Avail](https://availproject.org/) as the data availability layer.

The following steps are appropriately modified from the [Optimism documentation](https://docs.optimism.io/operators/chain-operators/tutorials/create-l2-rollup#deploy-the-l1-contracts).

#### Prerequisites

* [Avail Network Info](https://docs.availproject.org/da/networks) (RPC Endpoints)
* [How to get Avail AppID](https://docs.availproject.org/da/build-with-avail/interact-with-avail-da/app-id)
* [How to get Avail Seed (Avail Account)](https://docs.availproject.org/user-guides/accounts#creating-an-account-on-avail-da)
* [Docker](https://docs.docker.com/engine/install/) (for running the Avail DA Server with docker-compose)
* [Foundry](https://getfoundry.sh/) for contract deployment
* [Git](https://git-scm.com/)
* [Go](https://go.dev/)

We recommend to open a separate folder in your filesystem for the rest of this guide.

{% hint style="success" %}
Appchains require strong hardware to function properly. For demo purposes, we used a machine with **AMD EPYC 9124 @ 3.71GHz** and **128 GB of RAM**.
{% endhint %}

#### Step 1: Setup Avail DA prerequisites

**Step 1.1 Create an Avail account (seed)**

To submit data to Avail DA, you will need an Avail account (seed phrase). Follow the steps [here](https://docs.availproject.org/user-guides/accounts#creating-an-account-on-avail-da).

**Step 1.2 Get funds to your address**

To write data into Avail DA, your Avail account must be funded with AVAIL tokens. Follow the steps [here](https://docs.availproject.org/da/build-with-avail/interact-with-avail-da/faucet).

**Step 1.3 Create an AppID**

Create an AppID on Avail DA and save the AppID value. Follow the steps [here](https://docs.availproject.org/da/build-with-avail/interact-with-avail-da/app-id).

#### Step 2: DA Server Setup

This introduces a sidecar DA Server for Optimism that interacts with Avail DA for posting and retrieving data.

**Step 2.1 Build Avail DA Server**

Let's clone the `avail-alt-da-server` repository first and build the `avail-da-server` binary:

```bash
git clone https://github.com/availproject/avail-alt-da-server.git
cd avail-alt-da-server

make da-server
```

This will create the `avail-da-server` binary in the `bin` folder.

**Step 2.2 Run `avail-da-server` binary (Option 1)**

Run your DA sidecar (use an Avail RPC endpoint from the Network Info page):

```bash
./bin/avail-da-server \
  --addr=localhost \
  --port=8000 \
  --avail.rpc=https://turing-rpc.avail.so/rpc \
  --avail.seed="<YOUR_AVAIL_SEED_PHRASE>" \
  --avail.appid=<YOUR_AVAIL_APP_ID>
```

**Step 2.3 Run using `docker` (Option 2)**

* Copy `.env.example` to `.env`. Fill the values inside.
* Run the following commands:

```bash
docker-compose build
docker-compose up
```

#### Step 3: Setup Optimism repositories and configure environment

**Step 3.1: Clone repositories**

Clone `op-geth` and Optimism mono-repository, then checkout to `v1.9.2` for development purposes:

```bash
git clone https://github.com/ethereum-optimism/op-geth.git

git clone https://github.com/ethereum-optimism/optimism.git
cd optimism
git checkout v1.9.2
```

**Step 3.2: Setup environment**

Then, let's setup our environment using the `.envrc` file (alternatively you can set them in your shell):

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
export L1_RPC_KIND=basic

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

If you're using `direnv`, run:

```bash
direnv allow .
```

**Step 3.3: Run configuration scripts**

Navigate to the `optimism` directory and generate the configuration:

```bash
# ~/optimism
cd packages/contracts-bedrock
./scripts/getting-started/config.sh
```

Later in the same folder, let's run the deployment script using `forge` to deploy the necessary contracts on Citrea:

```bash
forge install

DEPLOY_CONFIG_PATH=./deploy-config/getting-started.json \
  forge script scripts/deploy/Deploy.s.sol:Deploy \
  --private-key $GS_ADMIN_PRIVATE_KEY \
  --broadcast \
  --rpc-url $L1_RPC_URL
```

And also run the L3 genesis configuration script:

```bash
DEPLOY_CONFIG_PATH=./deploy-config/getting-started.json \
CONTRACT_ADDRESSES_PATH=./deployments/5115-deploy.json \
  forge script scripts/L2Genesis.s.sol:L2Genesis \
  --sig "runWithStateDump()"
```

#### Step 4: Generate node configurations

Navigate to the `op-node` and generate the configuration for the L3 node:

{% hint style="info" %}
Note: Make sure your `state-dump` file matches your chain ID. It should be in the format `../packages/contracts-bedrock/state-dump-<L2_CHAIN_ID>-fjord.json`.
{% endhint %}

```bash
cd ../../op-node

go run cmd/main.go genesis l2 \
  --deploy-config ../packages/contracts-bedrock/deploy-config/getting-started.json \
  --l1-deployments ../packages/contracts-bedrock/deployments/5115-deploy.json \
  --outfile.l2 genesis.json \
  --l2-allocs ../packages/contracts-bedrock/state-dump-511551155115-fjord.json \
  --outfile.rollup rollup.json \
  --l1-rpc $L1_RPC_URL

openssl rand -hex 32 > jwt.txt

# If you cloned `op-geth` next to `optimism`, this path should work:
cp genesis.json ../../op-geth
cp jwt.txt ../../op-geth
```

#### Step 5: Initialize and start op-geth

Navigate to the `op-geth` directory and initialize the genesis configuration:

```bash
cd ../../op-geth

mkdir datadir
make geth
build/bin/geth init --state.scheme=hash --datadir=datadir genesis.json
```

Then, start the `op-geth`:

```bash
./build/bin/geth \
  --datadir ./datadir \
  --http \
  --http.corsdomain="*" \
  --http.vhosts="*" \
  --http.addr=0.0.0.0 \
  --http.api=web3,debug,eth,txpool,net,engine \
  --ws \
  --ws.addr=0.0.0.0 \
  --ws.port=8546 \
  --ws.origins="*" \
  --ws.api=debug,eth,txpool,net,engine \
  --syncmode=full \
  --gcmode=archive \
  --nodiscover \
  --maxpeers=0 \
  --networkid=511551155115 \
  --authrpc.vhosts="*" \
  --authrpc.addr=0.0.0.0 \
  --authrpc.port=8551 \
  --authrpc.jwtsecret=./jwt.txt \
  --rollup.disabletxpoolgossip=true
```

#### Step 6: Add DA-config to op-node

Navigate to `op-node` folder and add the following to the `rollup.json`:

```bash
cd ../optimism/op-node
nano rollup.json
```

Add:

```json
"alt_da": {
  "da_commitment_type": "GenericCommitment",
  "da_challenge_contract_address": "0x0000000000000000000000000000000000000000",
  "da_challenge_window": 160,
  "da_resolve_window": 160
}
```

{% hint style="info" %}
Note: at the top of `rollup.json` the L1 blockhash may be incorrect with respect to L1 block number. Be sure to check and replace the hash. This is a bug in OP Stack repo.
{% endhint %}

Go to <https://explorer.testnet.citrea.xyz/> to check for the correct hash.

```json
{
  "genesis": {
    "l1": {
      "hash": "", // check and replace this hash in rollup.json
      "number": 15360561
    },
    "l2": {
      "hash": "0x550d7172f4bb962f75aa50a2c5176cfc5234166a08416169f356512293d05a37",
      "number": 0
    }
  }
}
```

#### Step 7: Run op-node

Go back to the main `optimism` directory, and build `op-node`:

```bash
cd ..
make op-node
```

Then let's run the node:

```bash
cd op-node

./bin/op-node \
  --l2=http://localhost:8551 \
  --l2.jwt-secret=./jwt.txt \
  --sequencer.enabled \
  --sequencer.l1-confs=5 \
  --verifier.l1-confs=4 \
  --rollup.config=./rollup.json \
  --rpc.addr=0.0.0.0 \
  --p2p.disable \
  --rpc.enable-admin \
  --p2p.sequencer.key=$GS_SEQUENCER_PRIVATE_KEY \
  --l1=$L1_RPC_URL \
  --l1.rpckind=$L1_RPC_KIND \
  --altda.enabled=true \
  --altda.da-service=true \
  --altda.da-server=http://localhost:8000 \
  --l1.beacon=$L1_RPC_URL \
  --l1.beacon.ignore=true \
  --l1.trustrpc=true \
  --l1.http-poll-interval=2s
```

#### Step 8: Run op-batcher

Go back to the main `optimism` directory, and build the `op-batcher`:

```bash
cd ..
make op-batcher
```

Then let's run the batcher:

```bash
cd op-batcher

./bin/op-batcher \
  --l2-eth-rpc=http://localhost:8545 \
  --rollup-rpc=http://localhost:9545 \
  --poll-interval=1s \
  --sub-safety-margin=6 \
  --num-confirmations=1 \
  --safe-abort-nonce-too-low-count=3 \
  --resubmission-timeout=30s \
  --rpc.addr=0.0.0.0 \
  --rpc.port=8548 \
  --rpc.enable-admin \
  --max-channel-duration=25 \
  --l1-eth-rpc=$L1_RPC_URL \
  --private-key=$GS_BATCHER_PRIVATE_KEY \
  --altda.enabled=true \
  --altda.da-service=true \
  --altda.da-server=http://localhost:8000
```

#### Step 9: Run op-proposer

Go back to optimism main folder, and build the op-proposer:

```bash
cd ..
make op-proposer
```

Then let's run the proposer:

```bash
cd op-proposer

./bin/op-proposer \
  --poll-interval=2s \
  --rpc.port=8560 \
  --rollup-rpc=http://localhost:9545 \
  --l2oo-address=$(cat ../packages/contracts-bedrock/deployments/5115-deploy.json | jq -r .L2OutputOracleProxy) \
  --private-key=$GS_PROPOSER_PRIVATE_KEY \
  --l1-eth-rpc=$L1_RPC_URL
```

***

That’s it! Enjoy your Bitcoin Appchain on Citrea!
