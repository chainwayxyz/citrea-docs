# Pre-Confirmations

The sequencer's ordering promise is only trusted until the next Bitcoin block. The Merkle root of the soft blocks (batch tree) is inscribed in Bitcoin every 10 minutes. After the state root is inscribed, its validity and data availability is asserted inside the zk circuit, preventing any change in the ordering of transactions. This mechanism ensures that ordering of the transactions cannot be changed after it is inscribed in Bitcoin.

With this method, Citrea ensures that ordering finality is not delayed until the batch proof while keeping the full data publishing cost as low as possible. For future work, Citrea will introduce a multi-sequencer network, which will reduce ordering trust assumptions to near-zero.
