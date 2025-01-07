# Volition Model

Volition is a special type of data availability solution that combines off-chain and on-chain data storage. Traditionally, there are two categories of rollups regarding the data storage:

- **On-chain Data Availability** (e.g. Citrea): All transaction data / state diffs are stored on the L1 chain. Full nodes can simply scan L1 blocks to sync with the network. This is the most secure and trustless* way to provide data for the network; however, not the cheapest.
    - `Trustless` here is in the context of data only and it indicates that chain data is always available as long as the L1 chain is secure. This is pretty much the standard & correct definition in this context.

- **Off-chain Data Availability** (i.e. Validiums): All transaction data / state diffs are stored off-chain. Validiums are mostly cheaper than pure rollups; however, they rely on some other DA solutions and DACs. It is arguably less secure than using Bitcoin directly for our case and it introduces more trust assumptions to the system.

Volition, compared to two categories above, offers a hybrid approach - it allows users to choose where to publish transactions (+ relevant data), and choose the security of on-chain data availability or the cost efficiency of off-chain data availability.  Essentially there will be two states for two DA branches, and latest Citrea state will be the combination of these two branches.

It's important to note that regardless of the DA preference, all transactions would still be process by the STF (State Transition Function), and will be verified via ZK proofs. 

There are two major challenges in this direction:

a) Complexity: It's definitely not trivial to implement a volition model (none exist in production as of October 2024). It also requires modifications to the Citrea STF and overall architecture, particularly in terms of sequencing, proving, and running full nodes.

b) Guarantees: For users who prefer to use an alternative DA branch, trust assumptions will be different. It is essential to have a very secure off-chain DA solution for volition purposes, as well as to work on attack scenarios, bugs, and other potential issues and scenarios.

-----

The idea is on the roadmap and it will be actively implemented after the mainnet launch upon more research.
