# Optimistic Verification

BitVM is based on fraud proofs, which means it is a protocol that merely verifies the execution of a program using fraud proofs. Whole program is never being executed on-chain. If the result provided by the operator gets challenged by a verifier, the execution gets verified on-chain with a series of challenge-response protocol. If the off-chain result is correct, on-chain footprint is minimal.

Even though the BitVM paradigm is optimistic, Citrea is a ZK rollup that has optimistic settlement. As all the proofs and data is inscribed in Bitcoin, nodes accept proofs and verify them locally. The settlement only happens at checkpoints, which happens in every few months. Between checkpoint times, withdrawals are front covered by an operator and later claimed during settlement.
