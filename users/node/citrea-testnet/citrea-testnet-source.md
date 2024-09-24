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

Let's install necessary developer tools first:

```sh
make install-dev-tools
```

Then compile Citrea by running the following:

```sh
SKIP_GUEST_BUILD=1 cargo build --release
```

> 
> Optionally, if you wish to verify ZK-Proofs, then you'll need to compile our ZK VM binaries inside Docker. To do so, follow instructions to install Docker [here](https://docs.docker.com/engine/install/). Then, you should remove `SKIP_GUEST_BUILD=1` and add `REPR_GUEST_BUILD=1` before `cargo build` command:
> 
> ```sh 
> REPR_GUEST_BUILD=1 cargo build --release
> ```
>

#### Step 4: Run Citrea

Look through the `rollup_config.toml` and apply changes as you wish, if you modified any Bitcoin RPC configs, change corresponding values under `[da]`. Then, you can run the full node by the following:

```sh
./target/release/citrea --da-layer bitcoin --rollup-config-path ./resources/configs/testnet/rollup_config.toml --genesis-paths ./resources/genesis/testnet
```

If everything is correct, you should see some logs that looks like this:

```sh
2024-09-20T07:46:40.910416Z  INFO citrea_fullnode::runner: New State Root after soft confirmation #273314 is: RootHash("52b3c30f68667a7f25707534d2dc21d9071e7b776cb19df54166108fb6858c91")
2024-09-20T07:46:43.259511Z  INFO citrea_fullnode::runner: Running soft confirmation batch #273315 with hash: 0x1e387d251804ef906006dd0dd3c392fdb891c81ef14cb531cf51e7ee3e72e0e0 on DA block #46991
2024-09-20T07:46:43.267653Z  INFO citrea_fullnode::runner: New State Root after soft confirmation #273315 is: RootHash("1c04ab455e3b36da5a780b510e9d5bb1210ce6ca6027a11ac70e145bf33525d8")
2024-09-20T07:46:44.500370Z  INFO citrea_fullnode::runner: Running soft confirmation batch #273316 with hash: 0xd7ac25afe89c509d58a044271de3cfd720c4143a7dd07a86c615b99db1a26c83 on DA block #46991
```