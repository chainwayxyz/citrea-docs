# Running a Configurable Citrea Devnet Node with Docker Compose

This document will guide you through setting up and running a Citrea Devnet full node using Docker Compose. This setup will allow you to verify and analyze the chain, and use its RPC for development and deployment.

## Overview

To join the Citrea Devnet, you need to:
1. Run a Bitcoin Node in Signet mode that syncs with our custom Signet.
2. Run a Citrea Node that syncs with the Citrea Devnet.

Citrea is a ZK-Rollup on Bitcoin, so it requires a Bitcoin client synced with the Citrea Signet Network.

## Why a Custom Bitcoin Signet?

Citrea's development and testing require a controlled environment to simulate various scenarios, ensuring the robustness and security of our system. By running a custom Signet, we can test everything needed.

### Step-by-Step Setup

### Step 1: Install Docker

Install Docker from here: [Docker Installation Guide](https://docs.docker.com/get-docker/)

### Step 2: Set Up Docker Compose

Create a `docker-compose.yml` file with the following content:

```yaml
services:
  citrea-bitcoin-signet:
    image: chainwayxyz/citrea-bitcoin-signet:devnet
    container_name: bitcoin-signet
    environment:
      - SIGNETCHALLENGE=512102653734c749d5f7227d9576b3305574fd3b0efdeaa64f3d500f121bf235f0a43151ae
      - RPCUSER=citrea
      - RPCPASSWORD=citrea
      - ADDNODE=signet.citrea.xyz:38333
    volumes:
      - citrea-bitcoin-signet-data:/mnt/task/btc-data
    ports:
      - "38332:38332"
      - "38333:38333"
    networks:
      - citrea-devnet-network

  citrea-full-node:
    depends_on:
      - citrea-bitcoin-signet
    image: chainwayxyz/citrea-full-node:devnet
    platform: linux/amd64
    container_name: full-node
    environment:
      - ROLLUP__PUBLIC_KEYS__SEQUENCER_PUBLIC_KEY=52f41a5076498d1ae8bdfa57d19e91e3c2c94b6de21985d099cd48cfa7aef174
      - ROLLUP__PUBLIC_KEYS__SEQUENCER_DA_PUB_KEY=039cd55f9b3dcf306c4d54f66cd7c4b27cc788632cd6fb73d80c99d303c6536486
      - ROLLUP__PUBLIC_KEYS__PROVER_DA_PUB_KEY=03fc6fb2ef68368009c895d2d4351dcca4109ec2f5f327291a0553570ce769f5e5
      - ROLLUP__DA__NETWORK=signet
      - ROLLUP__STORAGE__PATH=/mnt/task/citrea-db
      - ROLLUP__RPC__BIND_HOST=0.0.0.0
      - ROLLUP__RPC__MAX_CONNECTIONS=1000
      - ROLLUP__RPC__BIND_PORT=8080
      - JSON_LOGS=1
      - ROLLUP__DA__NODE_URL=http://citrea-bitcoin-signet:38332/wallet/citrea
      - ROLLUP__DA__NODE_USERNAME=citrea
      - ROLLUP__DA__NODE_PASSWORD=citrea
      - ROLLUP__RUNNER__SEQUENCER_CLIENT_URL=https://rpc.devnet.citrea.xyz
      - ROLLUP__RUNNER__INCLUDE_TX_BODY=true
    volumes:
      - citrea-full-node-data:/mnt/task/citrea-db
    ports:
      - "8080:8080"
    networks:
      - citrea-devnet-network

volumes:
  citrea-bitcoin-signet-data:
  citrea-full-node-data:

networks:
  citrea-devnet-network:
    driver: bridge
```

### Step 3: Run Docker Compose

Navigate to the directory where you saved the `docker-compose.yml` file and run:

```sh
docker-compose up -d
```

This command will start the Bitcoin Signet node and the Citrea full node. After a minute, the node will start to sync with the network.

### Configuration Details

- **Bitcoin Signet Node:**
  - Uses a custom Signet challenge.
  - Configured with Citrea-specific settings.

- **Citrea Full Node:**
  - Connects to the Bitcoin Signet node for data availability.
  - Uses predefined public keys for various roles.
  - Configured to run the Citrea Devnet.


If you have any questions or need further assistance, please visit our Discord and ask in the #developer-chat channel.

Enjoy!