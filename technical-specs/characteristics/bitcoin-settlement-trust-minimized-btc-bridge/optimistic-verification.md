# Optimistic Verification

BitVM is based on fraud proofs, which means it is a protocol that merely verifies the execution of a program using fraud proofs. The whole program is never being executed on-chain. If the result provided by the operator gets challenged by a verifier, the execution gets verified on-chain with a series of challenge-response transactions. If the off-chain result is correct, the on-chain footprint is minimal.

Even though the BitVM paradigm is optimistic, Citrea is a ZK rollup that has optimistic settlement. As all the proofs and data are inscribed in Bitcoin, nodes accept proofs and verify them locally. The settlement only happens at checkpoints, which happen every few months. Between checkpoint times, withdrawals are fronted by an operator and later claimed during settlement.
