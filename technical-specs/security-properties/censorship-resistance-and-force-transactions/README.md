# Censorship Resistance and Force Transactions

Citrea blocks are produced by a sequencer or decentralized sequencer set. Citrea implements a mechanism called "Force Transactions". Force transactions are directly inscribed in Bitcoin using a specific envelope and serialization format. Thanks to blockspace proofs, Citrea sequencer must include forced transactions inscribed in Bitcoin into Citrea blocks.&#x20;

Citrea provers have the ability to scan Bitcoin blocks through inclusion and completeness proofs, and thus have access to all forced transactions sent to Bitcoin. If any of the forced transactions is not included from the given Citrea block within some delay, the proof generation fails for that batch. This results in slashing the sequencer's bond for proposing an invalid Citrea block. There is also an escape hatch mechanism if Citrea halts because of the censorship.

The maximum time a sequencer can delay to include the forced transactions will be determined based on the two-way peg mechanism's parameters.&#x20;

With Force Transactions, Citrea keeps the same level of censorship-resistance as Bitcoin even though bootstrapping a sufficient number of sequencers is often enough for L2 censorship resistance.

Forced transactions will be ready during the initial release with an all-or-nothing mechanism, meaning the sequencer cannot selectively skip the forced transactions. The escape hatch will be enabled during later releases.
