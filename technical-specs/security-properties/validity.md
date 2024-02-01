# Validity

Citrea provides validity with a trustless and scalable computation method; ZK-STARKs. Citrea proofs are succinct and complete, meaning a single proof proves the full rollup since genesis to date.

Citrea proofs are easily verifiable in any device. A Bitcoin light node (SPV) is enough to retrieve and verify the rollup. Verification inside a single Bitcoin script not possible because of the size and opcode limitations of Bitcoin script, however it is possible to optimistically verify the proof in Bitcoin script through BitVM.&#x20;

BitVM is a computing paradigm to express Turing-complete Bitcoin contracts. Using BitVM, Citrea proof verification is optimistically done on Bitcoin with fraud proofs. Any verifier (or guard) from the comittee can punish the prover (or proposer) if it submits an invalid Citrea proof, thus withdrawal set. Thanks to full ZK verification in Bitcoin with BitVM, not only the withdrawals are verified, but the full Citrea blockchain is verified in Bitcoin. This is the first time an L2 is trustlessly verified in Bitcoin, fundamentally different from sidechains including but not limited to drivechains.
