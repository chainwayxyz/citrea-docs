# Run Citrea Full Node

Running a Citrea node is permissionless - anyone can run a full node for development & security purposes.

The easiest way to run a Citrea Full Node is to use the `docker-compose.yml` that we prepared for you:

### Step 1: Install Docker

Follow instructions to install Docker [here](https://docs.docker.com/engine/install/).

### Step 2: Run using Docker Compose

```
curl https://raw.githubusercontent.com/chainwayxyz/citrea/nightly/docker/docker-compose.yml --output docker-compose.yml
docker compose -f docker-compose.yml up
```

This spins up a Bitcoin Testnet4 and Citrea Full Node for you to use quickly.

{% hint style="info" %}
Please note that **there are no financial incentives** to run a Citrea Full Node. It's for your own development setup and security practices.
{% endhint %}

***

We've combined these two methods using a Docker Compose file above. However, if you do not want to use Docker Compose & do your own modifications, please proceed with the rest of this section.

To sync with Citrea Testnet, you essentially need to:

1. [Run a Bitcoin Testnet4 Node](https://docs.citrea.xyz/developer-documentation/run-a-node/bitcoin-testnet4)
2. [Run a Citrea Full Node](https://docs.citrea.xyz/developer-documentation/run-a-node/citrea-testnet)

### Step 3: Check the sync status

You can check the status with the following command (you may need to arrange the URL at the end based on your setup):

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

## Hardware Requirements for running a node

A Linux/Mac/Windows system with a configuration of

* 8 GB RAM
* 2 TB SSD (NVMe recommended)
* 4 core CPU (if you're using cloud)
* 25+ Mbps network connection

should satisfy the minimum requirements to run a Citrea node. Allocating more resources improves the syncing speed.

***

If you encounter any problems during the node running even though you have a system that fits the requirements, please visit our [Discord](https://discord.gg/citrea) and let us know by opening a ticket.
