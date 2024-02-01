# Block Production



<figure><img src="../../../.gitbook/assets/block_prod.png" alt=""><figcaption><p>Block Production in Citrea</p></figcaption></figure>

In Citrea, the one that is responsible for producing blocks are called sequencer. Sequencer, unlike a validator or miner, doesn't need validations over produced blocks from other sequencers or nodes because every block produced by sequencers undergoes proving, which acts as a natural and trustless validation mechanism over blocks.

Sequencer builds blocks using its own local mempool. Anyone can a send transaction to sequencer's mempool using its RPC endpoints or a full node. In case of censorship, there is a force transaction mechanism that fallbacks to Bitcoin and guaranteed to be included in the next batch.

Sequencer is only responsible for ordering and publishing blocks. It can neither steal users' funds nor freeze them thanks to ZK proofs, force transaction mechanism, and on-chain data availability.
