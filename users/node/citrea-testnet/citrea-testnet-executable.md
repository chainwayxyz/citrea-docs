## Run a Citrea Testnet Node using pre-built binary

We've complied an executable for you to run a Citrea Full Node directly in MacOS / Linux. For the following steps, we assume that you have a Bitcoin Testnet4 node running. If not, please visit [here](../bitcoin-testnet4/README.md).

#### Step 0: Make citrea folder and move in

To tidy things up, you can run the following and make a separate folder:

```sh
mkdir citrea

cd citrea
```

#### Step 1: Download binary executable

We're hosting our binary in the [Citrea](https://github.com/chainwayxyz/citrea/releases) repository. Please download the suitable executable for your configuration.

#### Step 2: Download genesis & testnet config

To sync correctly, config & genesis files should be provided correctly. You can download & extract them using the following:

```sh
curl https://raw.githubusercontent.com/chainwayxyz/citrea/nightly/resources/configs/testnet/rollup_config.toml --output rollup_config.toml

curl https://static.testnet.citrea.xyz/genesis.tar.gz --output genesis.tar.gz
tar -xzvf genesis.tar.gz
```

#### Step 3: Run!

Based on your operating system, run one of the following:

Mac:
```sh
./citrea-v0.5.0-osx-arm64 --da-layer bitcoin --rollup-config-path ./rollup_config.toml --genesis-paths ./genesis
```

Linux:
```sh
./citrea-v0.5.0-linux-amd64 --da-layer bitcoin --rollup-config-path ./rollup_config.toml --genesis-paths ./genesis
```

If the paths are correct, the full node will start to sync. You can check the status with the following command (assuming the rollup_config toml is the same as provided):

```sh
curl -X POST --header "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"citrea_syncStatus","params":[], "id":31}' http://0.0.0.0:8080
```

A sample response (fields may vary based on the sync status):

```json
{
  "jsonrpc": "2.0",
  "id": 31,
  "result": {
    "l1Status": {
      "Synced": 46916
    },
    "l2Status": {
      "Syncing": {
        "headBlockNumber": 252441,
        "syncedBlockNumber": 123425
      }
    }
  }
}
```

-----

If you do not want to use the provided binary executable to setup a Citrea node, you can also build it from the source code [here](citrea-testnet-source.md).