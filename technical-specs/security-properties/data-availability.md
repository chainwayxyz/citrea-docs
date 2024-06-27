# Data Availability

<figure><img src="../../.gitbook/assets/da (1).png" alt=""><figcaption><p>DA in Citrea</p></figcaption></figure>

Data availability by definition represents the availability of data needed to transact for everyone. Unlike existing L2 solutions on Bitcoin, Citrea keeps all the data needed to transact on the rollup in Bitcoin.

Because the data bandwidth is limited on Bitcoin, Citrea keeps the data in a special form called state differences (state diffs in short). Instead of recording the full Citrea transaction data like signature witnesses in Bitcoin, Citrea records the state diff between two consecutive batches. The state diff is constrained by the zk circuit, which means it cannot be altered without invalidating the proof.

A full node can retrieve the full rollup state by only looking into Bitcoin blocks. It can examine Bitcoin blocks one-by-one, extract the valid batch proofs, and apply the state diffs to the local state. Once it reaches to the latest Bitcoin block, the full node has the full Citrea state. Liveness is crucial to recover from catastrophic events as well as censorship resistance for nodes. Citrea lives as long as Bitcoin lives thanks to it's on-chain data availability.

For low security use cases, Citrea heavily explores the volition model. The volition model is a special type of L2 that has both on-chain and off-chain data.
