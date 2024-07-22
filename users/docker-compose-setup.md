# Running a Configurable Citrea Devnet Node with Docker Compose

This guide goes through the simpler Docker Compose setup. It launches a custom Bitcoin Signet and a Citrea full node for quick syncing.

### Step 1: Install Docker

Install Docker from here: [Docker Installation Guide](https://docs.docker.com/get-docker/)

### Step 2: Set Up Docker Compose

Download the `docker-compose.yml` file from the repository: [https://raw.githubusercontent.com/chainwayxyz/citrea/v0.4.5/docker-compose.yml](https://raw.githubusercontent.com/chainwayxyz/citrea/v0.4.5/docker-compose.yml)

> `ROLLUP__RUNNER__INCLUDE_TX_BODY` can be set to `false` if soft batches are not needed - it saves some space.

### Step 3: Run Docker Compose

Navigate to same directory with `docker-compose.yml` file, and run the command in terminal:

```sh
docker compose up -d
```

This command will start the Bitcoin Signet node and the Citrea full node. If you want to follow the logs, you can run the following:

```sh
docker logs --follow full-node
```

After a minute, the node will start to sync with the network.

_Optionally, you can type `ulimit -n 5000` in the terminal to increase the maximum number of open file descriptors to 5000. This can speed up syncing by allowing more files to be open simultaneously, but be aware it may affect overall system performance during the sync process._

-------------------------

If you have any questions or need further assistance, please visit our [Discord](https://discord.gg/citrea) and ask in the #developer-chat channel.