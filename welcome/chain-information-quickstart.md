# Chain Information - Quickstart

## Core Parameters

<table data-header-hidden><thead><tr><th valign="middle">Config</th><th></th></tr></thead><tbody><tr><td valign="middle"><strong>Network Name</strong></td><td>Citrea Testnet</td></tr><tr><td valign="middle"><strong>Chain ID</strong></td><td>5115</td></tr><tr><td valign="middle"><strong>Currency Symbol*</strong></td><td>cBTC</td></tr><tr><td valign="middle"><strong>Currency Decimals</strong></td><td>18</td></tr><tr><td valign="middle"><strong>Currency Name</strong></td><td>Citrea Bitcoin</td></tr><tr><td valign="middle"><strong>HTTP RPC URL</strong></td><td><a href="https://rpc.testnet.citrea.xyz/">https://rpc.testnet.citrea.xyz</a></td></tr><tr><td valign="middle"><strong>Block Explorer</strong></td><td><a href="https://explorer.testnet.citrea.xyz/">https://explorer.testnet.citrea.xyz</a><br><a href="https://5115.testnet.routescan.io/">https://5115.testnet.routescan.io/</a></td></tr><tr><td valign="middle"><strong>Network Type</strong></td><td>EVM-compatible ZK Rollup (Type II zkEVM)</td></tr><tr><td valign="middle"><strong>Block Time</strong></td><td>2 seconds</td></tr><tr><td valign="middle"><strong>Block Gas Limit</strong></td><td>10,000,000</td></tr><tr><td valign="middle"><strong>Latest Supported EVM Version</strong></td><td>Pectra**</td></tr><tr><td valign="middle"><strong>Settlement &#x26; Data Availability Layer</strong></td><td>Bitcoin</td></tr><tr><td valign="middle"><strong>Genesis Specs</strong></td><td><a href="https://github.com/chainwayxyz/citrea/blob/nightly/resources/genesis/testnet/evm.json">evm.json</a></td></tr><tr><td valign="middle"><strong>Currency Logo</strong></td><td><a href="https://drive.google.com/drive/folders/1e9VFs4v3_mBs-WAW-FzVgTnPvYyJ8pvF?usp=sharing">cBTC Logo</a></td></tr><tr><td valign="middle"><strong>Chain Logo</strong></td><td><a href="https://drive.google.com/drive/folders/1PyBMVom9sN5Qe529vCT8jBq3Q8fcHoS5?usp=sharing">Citrea Brand Kit</a></td></tr></tbody></table>

<sup>\*native currency, not ERC-20</sup>\ <sup>\*\*differences explained in the</sup> [<sup>Execution Environment</sup>](https://docs.citrea.xyz/essentials/execution-environment) <sup>page</sup>

***

## Kickstart Development&#x20;

### Add Citrea to your Wallet / Code

For wallets, you can either use the information above, click & add using [**Chainlist**](https://chainlist.org/chain/5115)**,** or the following JSON as the baseline:

```json
{
  "chainId": "0x13FB",
  "chainName": "Citrea Testnet",
  "nativeCurrency": { "name": "cBTC", "symbol": "cBTC", "decimals": 18 },
  "rpcUrls": ["https://rpc.testnet.citrea.xyz"],
  "blockExplorerUrls": ["https://explorer.testnet.citrea.xyz", "https://5115.testnet.routescan.io/"]
}
```

| Network Name       | Citrea Testnet                                                                                       |
| ------------------ | ---------------------------------------------------------------------------------------------------- |
| Default RPC URL    | [https://rpc.testnet.citrea.xyz](https://rpc.testnet.citrea.xyz/)                                    |
| ChainID            | 5115                                                                                                 |
| Currency Symbol    | cBTC                                                                                                 |
| Block Explorer URL | <p><a href="https://explorer.testnet.citrea.xyz/"><https://explorer.testnet.citrea.xyz/></a><br></p> |

### Get cBTC to deploy smart contracts to Citrea Testnet

cBTC is used as the native currency to cover gas, which is necessary to deploy smart contracts & use the network.

You can receive testnet funds at <https://citrea.xyz/faucet> to get started.

If you have BTC on Bitcoin Testnet4, you can also use [Garden Finance](https://testnet.garden.finance/?input-chain=bitcoin_testnet\&input-asset=BTC\&output-chain=citrea_testnet\&output-asset=cBTC) or [Atomiq](https://testnet4.atomiq.exchange/) to swap them into cBTC.

***

## Where to go from here?

Here are quick links you might be looking for:

* [RPC Endpoints](https://docs.citrea.xyz/developer-documentation/rpc-documentation)
  * includes support for `eth_` endpoints, along with useful Bitcoin & finality related endpoints of Citrea
* [Ecosystem Tooling](https://docs.citrea.xyz/developer-documentation/ecosystem-tooling)
  * partners to check out for Interop, Oracles, Indexers, Wallets, Account Abstraction...
* [Citrea Execution Environment](https://docs.citrea.xyz/essentials/execution-environment)
  * explainers on subtle differences of Citrea compared to Ethereum and other EVM environments
* [System Contracts](https://docs.citrea.xyz/developer-documentation/system-contracts)
  * a set of pre-deployed smart contracts on Citrea that can be used to verify Bitcoin transactions, execute bridge logic, and more
