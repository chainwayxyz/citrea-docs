
# Running a configurable Citrea Devnet Node

Welcome aboard! 

In this customized setup document, we will briefly go through the steps to run a Citrea Devnet full node that you can use to verify & analyze the chain, and also be able to use its RPC for development, deployment, and much more ideas that you come up with!

To join the Citrea Devnet properly, we need to do two things: Running a Bitcoin Node in Signet mode that syncs with our custom Signet, and running a Citrea Node that syncs with the Citrea Devnet.

> **Why a Custom Bitcoin Signet?**
>
> To be able to answer this, we should first visit on another question: **Why Devnet**?
>
> Well, on our mission to scale Bitcoin securely up until now, we've implemented two major components. The first one is the sequencer & full node implementation of [Citrea](https://github.com/chainwayxyz/citrea), and the second one is [Clementine](https://github.com/chainwayxyz/clementine) - a BitVM based trust-minimized two-way Peg program for bridging. While we're shipping fast and carefully, testing always plays a crucial role for the software development. Given the environment and importance of our product, it's our duty to ensure that our code & infrastructure is tested against bugs, possible issues, and possible attacks.
>
> In that sense, we've decided run Citrea as a public Devnet first. It will be open for everyone - users can try and feel it, developers can start to build on it & hack it, we can see that it's working as expected and move it to the next stages. It will help us test everything except BitVM fraud proofs, which is work-in-progress nowadays and going to be tested in the later phases.
> 
> If the idea so far makes sense, now we can focus on the DA aspect. Citrea is a rollup - that means, we're publishing our data (not only a hash) to the DA. Along with that, we're also settling on the DA layer as well. Given the state of Bitcoin script and its capabilities, we should also be able to test everything from the DA aspect where Bitcoin is used. Thus, we've decided to run a custom Signet - a Signet that is configurable by us so that we can test **anything** that we want. We should be ready for any bugs, any edge cases, any attacks that happens on the layer 1 - all to ensure that everything will continue to work as expected.
> 
> Please note that there's no Citrea token, and any asset in the Citrea Devnet does not possess any financial value. We're already providing a [faucet](https://example.com) for our Devnet. For users, for developers, for curious minds to try, it will be more than enough. If you think this is not enough or you have some other ideas, please visit our Discord and tell us. We will be more than happy to help.

Now, if everything is clear, let's start with the Bitcoin Signet Setup!

## Bitcoin Signet Setup

Citrea is a ZK-Rollup on **Bitcoin**. Thus, to be able to verify the network underneath, Bitcoin, one needs to run a Bitcoin client that is synced with the Citrea Signet Network. This is also necessary as Full Nodes gather some information on Bitcoin, too. 

For this step, we've prepared a simple Docker container that you can run really easily.

### Step 1: Install Docker

Install Docker from here: https://docs.docker.com/get-docker/

### Step 2: Setup Bitcoin Signet

Download/Clone the Bitcoin Signet Container
```sh
git clone https://github.com/chainwayxyz/bitcoin_signet && cd bitcoin-signet
```

### Step 3: Run Bitcoin Client

Run it the Bitcoin client with the following command:
```sh
docker run -d --name bitcoin-signet-client-instance \
--env MINERENABLED=0 \
--env SIGNETCHALLENGE=512102653734c749d5f7227d9576b3305574fd3b0efdeaa64f3d500f121bf235f0a43151ae \
--env ADDNODE=signet.citrea.xyz:38333 -p 38332:38332 \
bitcoin-signet
```

Ta-da! That's it for Bitcoin Part. Now, let's do the Citrea side.

## Citrea Client Setup

Before building & running a Citrea client, we need to do a couple of configurations. Let's briefly go over them.


### Step 4: Install Rust

Citrea is built using Rust, so we need to install Rust. If you don't have it, install it from [here](https://www.rust-lang.org/tools/install).

### Step 5: Clone the source code

Let's download/clone the repository from the latest tag:
```sh
git clone https://github.com/chainwayxyz/citrea (add tag here)
```

### Step 6: Edit rollup config file

For a Citrea Full Node to run successfully, we first need to edit our config file. 

Config for Citrea Devnet resides in the `configs/devnet/` folder. Since we're running a Bitcoin Signet container in your local system, we need to edit the `configs/devnet/rollup_config.toml` file.

There are several fields to modify:

##### On DA Layer 
```toml
[da]
node_url = "http://0.0.0.0:38332"
node_username = "bitcoin"                                     
node_password = "bitcoin"
network = "signet"
```

##### Full Node RPC 

The host & port here are the host & port that your full node RPC uses - you can keep it like this, or change in anyway you want.
```toml
[rpc]
bind_host = "127.0.0.1"
bind_port = 12345
```

##### Runner Config
Here, `sequencer_client_url` is the client url to connect to the sequencer. In this case, it's `https://rpc.devnet.citrea.xyz`. 

Along with that, `include_tx_body` is a flag for saving the Soft Batches locally. It's not necessary for the execution of full nodes, so you can keep it off if you don't have enough space. You can also set it to `true` if you want to check it out.

So, an example that may work you is the following:

```toml
[runner]
sequencer_client_url = "https://rpc.devnet.citrea.xyz"
include_tx_body = false
```

The above config will work fine with the Docker command above, so unless you have an expertise, we advise you to not to change them.

### Step 7: Check Genesis files

Most of the time, blockchains start with a Genesis file - some addresses, some contracts, comes with some custom touches when a chain is started as they're necessary for the overall execution to work. In Citrea, we have two important system contracts: [BitcoinLightClient](https://github.com/chainwayxyz/citrea/blob/nightly/crates/evm/src/evm/system_contracts/src/BitcoinLightClient.sol) and [Bridge](https://github.com/chainwayxyz/citrea/blob/nightly/crates/evm/src/evm/system_contracts/src/Bridge.sol). These contracts have been put at the Genesis block through a genesis configuration file. Thus, for a full node to be able to succeed at syncing, it needs to be aware of this custom configutation. 

You can find the genesis files of Citrea Devnet in the `configs/devnet/genesis-files/` folder.

### Step 8: Build the project

Perfect! Now that we've completed our configuration, we can process to build and run Citrea.

Now, that we have the repository, configs, and Rust ready, we can build the project. You can do the following:
```sh
SKIP_GUEST_BUILD=1 make build
```

Here, `make build` command simply runs `cargo build --release`, telling Rust to build the project in release mode (takes a couple of minutes to build, but runs quite fast). The flag before, `SKIP_GUEST_BUILD=1`, is a way of telling the system to skip the parts relevant to proving - [Risc0](https://www.risczero.com/) as it's not necessary for the full nodes of Devnet to run. However, if you also want to build that way, you can remove the flag, and install the `Risc0` components with the following command first:

```sh
make install-dev-tools
```

It may take a while depending on your system & network speed.

### Step 9: Run!

Awesome! Since we've now completed building the project, we can run it. Your executable will reside in the `./target/release` folder, so the following command works for you:

```sh 
./target/release/citrea --da-layer bitcoin --rollup-config-path devnet-configs/devnet.toml --genesis-paths devnet-configs/genesis
```

------------------------

If you have any questions, please don't hesitate visit our Discord and ask us in the #developer-chat channel. 

Enjoy!