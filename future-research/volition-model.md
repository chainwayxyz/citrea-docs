# Volition Model

Volition is a special type of data availability solution that combines off-chain and on-chain data storage.

Traditionally, there are two categories of rollups regarding the data storage:

* **On-chain Data Availability** (e.g. Citrea): All transaction data / state diffs are stored on the L1 chain. Full nodes can simply scan L1 blocks to sync with the network. This is the most secure and trustless\* way to provide data for the network.
  * `Trustless` here is in the context of data only and it indicates that chain data is always available as long as the L1 chain is secure.
* **Off-chain Data Availability** (i.e. Validiums): All transaction data / state diffs are stored off-chain. Validiums are mostly cheaper than pure rollups; however, they rely on some other DA solutions and DACs. It introduces more trust assumptions to the system.

Volition, compared to two categories above, offers a hybrid approach - it allows users to choose where to publish transactions (or relevant data), and choose the security of on-chain data availability or the cost efficiency of off-chain data availability. There will be two states for two DA branches, and the latest Citrea state will be the combination of these two branches.

![Volition Model Diagram](https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2Fgit-blob-cf96881fd7d69b2c5ba05be25b843a1e73ea2f09%2Fvolition.png?alt=media)

Regardless of the DA preference, all transactions will still be processed by the STF (State Transition Function), and will be verified via ZK proofs.

There are two major challenges in this direction:

a) Complexity: Volition implementation requires modifications to the Citrea STF and overall architecture, particularly in terms of sequencing, proving, and running full nodes.

b) Guarantees: For users who prefer to use an alternative DA branch, trust assumptions will be different as the off-chain DA solution does not provide the same trust assumptions as Bitcoin L1.

***

The concept is on the roadmap and it will be actively implemented after the mainnet launch upon more research.
