
# Running a configurable Citrea Devnet Node

This guide goes through the customized setup.

To join the Citrea Devnet properly, we need to do two things: Running a Bitcoin Node in Signet mode that syncs with our custom Signet, and running a Citrea Node that syncs with the Citrea Devnet.

> **Why a Custom Bitcoin Signet?**
>
> We're running our custom Bitcoin Signet, because:
> - There's not enough testnet or signet BTC available to public.
> - There are deep reorgs happening on testnet.
> 
> Using our own signet (which has only block time modification of 1 minute compared to public signet), we can provide better bridge experience for testing purposes.

## Step 1: Bitcoin Signet Setup

For this step, we've prepared a simple Docker container.

### Step 1.1: Install Docker

Install Docker from here: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

### Step 1.2: Setup Bitcoin Signet

Clone the Bitcoin Signet Container and navigate to the folder
```sh
git clone https://github.com/chainwayxyz/bitcoin_signet && cd bitcoin_signet
```

### Step 1.3: Build Signet Container

```sh
docker build -t bitcoin-signet .
```

### Step 1.4: Run Signet Container

Run it the Bitcoin client with the following command:

```sh
docker run -d --name bitcoin-signet-client-instance \
--env MINERENABLED=0 \
--env SIGNETCHALLENGE=512102653734c749d5f7227d9576b3305574fd3b0efdeaa64f3d500f121bf235f0a43151ae \
--env BITCOIN_DATA=/mnt/task/btc-data \
--env ADDNODE=signet.citrea.xyz:38333 -p 38332:38332 \
bitcoin-signet
```

Citrea Signet Bitcoin should be running in your system now. 

## Step 2: Citrea Client Setup

### Step 2.1: Install Rust

If you don't have it, install it from [here](https://www.rust-lang.org/tools/install).

### Step 2.2: Clone the source code

Let's clone the repository from the latest tag:
```sh
git clone https://github.com/chainwayxyz/citrea --branch=v0.4.1 && cd citrea
```

### Step 2.3: Edit rollup config file

Config for Citrea Devnet resides in the `configs/devnet/` folder, `configs/devnet/rollup_config.toml` file. There are several fields to modify, you can use the following if your Bitcoin Signet is working:

<!-- ##### On DA Layer  -->
```toml
# DA Config
[da] 
node_url = "http://0.0.0.0:38332"
node_username = "bitcoin"                                     
node_password = "bitcoin"
network = "signet"

# Full Node RPC - The host & port here are the host & port that your full node RPC uses, do not change if you're not sure how it works.
[rpc] 
bind_host = "127.0.0.1"
bind_port = 12345

[runner]
sequencer_client_url = "https://rpc.devnet.citrea.xyz"
include_tx_body = false # Setting false to save space by not saving Soft Batches locally. 
```
The above config will work fine with the Docker command above, so unless you have an expertise, we advise you to not to change them.

### Step 2.4: Build the project

If all above works fine, build the project:

```sh
SKIP_GUEST_BUILD=1 make build-release
```

This may take from 5 to 15 minutes.

If you want to see proving (optional), remove the `SKIP_GUEST_BUILD` flag and run the following first:

```sh
make install-dev-tools
```

If you also want to see less logs, you can put `RUST_LOG=info` to the beginning of the command.

### Step 2.5: Run

Run the following to start running the Citrea binary.

```sh 
./target/release/citrea --da-layer bitcoin --rollup-config-path configs/devnet/rollup_config.toml --genesis-paths configs/devnet/genesis-files
```

------------------------

If you have any questions, please don't hesitate visit our [Discord](https://discord.gg/invite/citrea) and ask us in the #developer-chat channel. 
