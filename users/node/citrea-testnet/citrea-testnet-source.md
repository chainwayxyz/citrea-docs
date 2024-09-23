## Run Citrea Testnet by building from the source

You may want to do some changes, compile, and run the Citrea on your local. 

#### Step 1: Install Rust

If you don't have it, install it from [here](https://www.rust-lang.org/tools/install).

#### Step 2: Clone the source code

Let's clone the repository and checkout the latest tag:
```sh
git clone https://github.com/chainwayxyz/citrea
cd citrea
git fetch --tags
git checkout $(git describe --tags `git rev-list --tags --max-count=1`)
```

#### Step 3: Build Citrea

Compile Citrea by running command:

```sh
cargo build --release
```


Optinally, if you wish to verify ZK-Proofs, then you'll need to compile our ZK VM binaries inside Docker. Follow instructions to install Docker [here](https://docs.docker.com/engine/install/). Then, you should add `REPR_GUEST_BUILD=1` before `cargo build` command.

```sh 
REPR_GUEST_BUILD=1 cargo build --release
```

#### Step 4: Run Citrea

Look through the `rollup_config.toml` and apply changes as you wish, if you modified any Bitcoin RPC configs, change corresponding values under `[da]`. Then, you can run the full node by the following:

```sh
./target/release/citrea --da-layer bitcoin --rollup-config-path ./resources/configs/testnet/rollup_config.toml --genesis-paths ./resources/genesis/testnet
```