# Block Production

<figure><img src="../../../.gitbook/assets/block_prod.png" alt=""><figcaption><p>Block Production in Citrea</p></figcaption></figure>

In Citrea, the entity responsible for producing blocks is called the "sequencer." A sequencer, unlike a validator or miner, doesn't need validations over produced blocks from other sequencers or nodes because every block produced by sequencers undergoes a zero-knowledge proving process, which acts as a natural and trustless validation mechanism over blocks.

The sequencer builds blocks using its own local mempool. Anyone can a send transaction to sequencer's mempool using its RPC endpoints or a full node. In case of censorship, there is a force transaction mechanism that falls back to Bitcoin and guarantees transactions will be included in the next batch.

The sequencer is only responsible for ordering and publishing blocks. It can neither steal users' funds nor freeze them thanks to ZK proofs, force transaction mechanism, and on-chain data availability.
