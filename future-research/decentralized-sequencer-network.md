# Decentralized Sequencer Network

Citrea contributors are exploring solutions to support multiple sequencers in consensus without affecting latency and finality. One possible approach is to implement a PoS-like layer but for only sequencing blocks. The source of truth for users will always be zero-knowledge proofs in Bitcoin. The decentralized sequencing layer will reduce the short term trust in sequencers to protect the ordering, as the ordering will be finalized on the sequencing layer in a single slot.

There are several consensus mechanisms that are being experimented such as CometBFT, Hotstuff, and MonadBFT.
