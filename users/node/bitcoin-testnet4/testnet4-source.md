
## Run Bitcoin Testnet4 node from source code

#### Step 1: Clone the repository for Testnet4

Clone the repository from the official Bitcoin Core repository:

```sh
git clone https://github.com/bitcoin/bitcoin.git
cd bitcoin
git checkout v28.0rc1
```

#### Step 2: Build Bitcoin Core

Follow the instructions to build the repository for [Linux](https://github.com/bitcoin/bitcoin/blob/v28.0rc1/doc/build-unix.md), [MacOS](https://github.com/bitcoin/bitcoin/blob/v28.0rc1/doc/build-osx.md), or [Windows](https://github.com/bitcoin/bitcoin/blob/v28.0rc1/doc/build-windows.md). You don't need to reclone the repo if you're asked to, since we've already done that.

#### Step 3: Run Testnet4 Node

After building, you can run the following:

```sh
bitcoind -testnet4 -daemon -txindex=1 -rpcbind=0.0.0.0 -rpcport=18443 -rpcuser=citrea -rpcpassword=citrea
```

The arguments you provide here are important - if you modify any of these, you may also need to modify some fields before running a Citrea client in the next steps. Please check everything carefully before proceeding.