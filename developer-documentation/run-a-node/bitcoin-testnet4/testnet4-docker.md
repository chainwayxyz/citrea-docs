# Testnet4 Docker Setup

Using the available Docker image is fairly easy to run a Bitcoin Testnet4 node.

### Step 1: Install Docker

Follow instructions to install Docker [here](https://docs.docker.com/engine/install/).

### Step 2: Run Testnet4 node using Docker:

Pull the correct Bitcoin image and run:

```sh
docker run -d \
  --name bitcoin-testnet4 \
  -p 18443:18443 \
  -p 18444:18444 \
  bitcoin/bitcoin:29 \
  -printtoconsole \
  -testnet4=1 \
  -rest \
  -rpcbind=0.0.0.0 \
  -rpcallowip=0.0.0.0/0 \
  -rpcport=18443 \
  -rpcuser=citrea \
  -rpcpassword=citrea \
  -server \
  -txindex=1
```

Bitcoin Testnet4 node should be running in your system. You can check it with the following command:

```sh
curl --user citrea --data-binary '{"jsonrpc": "1.0", "id": "curltest", "method": "getblockcount", "params": []}' -H 'content-type: text/plain;' http://0.0.0.0:18443
```

The `--user` flag and the password are the `rpcuser` and `rpcpassword` values that you used to run the Docker command above.

Now you can proceed to run the Citrea client from [here](https://docs.citrea.xyz/developer-documentation/run-a-node/citrea-testnet/citrea-testnet-executable).

***

If you do not want to use Docker to setup a Bitcoin node, you can build it from the source code [here](https://docs.citrea.xyz/developer-documentation/run-a-node/bitcoin-testnet4/testnet4-source).
