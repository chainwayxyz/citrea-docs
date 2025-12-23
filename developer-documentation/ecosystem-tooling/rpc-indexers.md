# RPC & Indexers

## RPC Providers

Citrea provides a free RPC endpoint for developers to use: <https://rpc.testnet.citrea.xyz>. For small projects & simple queries, you may use this endpoint safely as it offers generous rate limits.

However, for larger projects or production use, we recommend [setting up your own RPC node](https://docs.citrea.xyz/developer-documentation/run-a-node) or a dedicated RPC provider.

### Alchemy

[Alchemy](https://www.alchemy.com/docs/reference/citrea-api-quickstart) is a high-performance RPC provider that offers a generous free tier and stronger paid plans for higher usage for Citrea. You may refer to their [webpage](https://www.alchemy.com/pricing) for more details on the usage.

***

## Indexers

### Goldsky

[Goldsky](https://goldsky.com/) is a Web3-native, real-time data platform that lets you index and query on-chain data without wrangling infrastructure. Built for effortless scale and backed by 24 × 7 support, it frees you to ship features instead of managing servers.

Citrea supports both [Goldsky Subgraphs](https://docs.goldsky.com/subgraphs/introduction) and [Goldsky Mirrors](https://docs.goldsky.com/mirror/introduction). Subgraphs provide high-performance, custom-modeled queries with smart tagging for precise filtering for your development, and Mirrors stream live blockchain events straight into your own database and keep data continuously in sync.

As a developer building on Citrea, you can utilize Goldsky products at a discount. Check out the Citrea page on their [documentation](https://docs.goldsky.com/chains/citrea?utm_source=citrea\&utm_medium=docs) for more information and kickstart your development!

*Note*: The correct Citrea slug for Goldsky is `citrea-testnet-tangerine`.

### Envio

[Envio](https://envio.dev) is a super-fast data platform on Citrea with HyperIndex and HyperSync support.

Using [Envio HyperIndex](https://docs.envio.dev/docs/HyperIndex/overview) you can transform on-chain events into structured, queryable databases with GraphQL APIs.

[Envio HyperSync](https://docs.envio.dev/docs/HyperSync/overview) is Envio's high-performance blockchain data engine that serves as a direct replacement for traditional RPCs for read-only endpoints, delivering up to 2000x faster data access.

### SubQuery

[SubQuery](https://subquery.network/) is an open‑source indexing framework that turns raw Citrea events into fast, flexible GraphQL APIs. It supports multi‑chain projects in a single workspace, lets you run multiple workers behind custom RPC endpoints for higher throughput, and can be self‑hosted or pushed to the decentralised SubQuery Network for SLA‑backed serving.

* **Get started:** follow the [Quick‑Start guide](https://subquery.network/doc/quickstart/quickstart.html) to scaffold and run a project in minutes.
* **Starter repo:** clone the template for [Citrea Testnet](https://github.com/subquery/ethereum-subql-starter/tree/main/Citrea/citrea-testnet-starter).
* **Deployment options:** run locally, host with OnFinality/Traceye, or publish to the [SubQuery Network](https://subquery.network/doc/indexer/run_publish/introduction.html) for decentralized uptime and rewards.
