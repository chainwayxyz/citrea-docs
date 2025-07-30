# Oracles & VRFs

### Redstone
[Redstone](https://redstone.finance) is a modular oracle network that provides secure price feeds for blockchain protocols.

Following price feeds are provided by Redstone on Citrea Testnet: `BTC/USD`, `ETH/USD`, `USDT/USD`, `pxETH/USD`, `solvBTC/USD`

You can find the contract addresses & feeds on the [Redstone documentation](https://app.redstone.finance/app/feeds/?networks=5115&page=1&sortBy=popularity&sortDesc=false&perPage=32).

---

### Blocksense  
[Blocksense](https://blocksense.network) is a decentralized oracle protocol designed to scale data feeds using zero-knowledge rollups. It allows anyone to create secure oracles through a permissionless process. 

Following price feeds are provided with a Chainlink-compatible interface by Blocksense on Citrea, with a heartbeat of 5 minutes: `BTC`, `ETH`, `EURC`, `USDT`, `USDC`, `PAXG`, `TBTC`, `WBTC`, `WSTETH`. 

You can find the contract addresses & feeds on the [Blocksense documentation](https://docs.blocksense.network/docs/contracts/deployed-contracts#citrea-testnet).

---

### Stork 
[Stork](https://stork.network) provides real-time price feeds using a pull-oracle model. It allows developers to fetch the latest price data on demand, ensuring they always have access to the most current information.

You can check the Stork documentation [here](https://docs.stork.network/) to kickstart your development. Stork contracts deployed on Citrea are at the following address: [0x266795f5A45AEc26aBF7E1c923dC15Cbb1A4Ed96](https://explorer.testnet.citrea.xyz/address/0x266795f5A45AEc26aBF7E1c923dC15Cbb1A4Ed96?tab=contract)

---

### Supra dVRF  

Supra dVRF delivers randomness that’s:

- Low-latency
- Tamper-proof (via threshold signatures + secret sharing)
- Unbiased + unpredictable (based on blockhash, nonce & user input)
- Verifiable on-chain with cryptographic proof

You can find the documentation for Supra dVRF [here](https://docs.supra.com/dvrf/v2-guide) and the contracts deployed on Citrea at the following addresses: 

- **Router**: [0xDF666c76E859D6541FF17E1C6c215e46BfA563B2](https://explorer.testnet.citrea.xyz/address/0xDF666c76E859D6541FF17E1C6c215e46BfA563B2?tab=contract)
- **Deposit**: [0xcF99ab8c2AABC04349139484BAFC26f480Ef4cE4](https://explorer.testnet.citrea.xyz/address/0xcF99ab8c2AABC04349139484BAFC26f480Ef4cE4?tab=contract)