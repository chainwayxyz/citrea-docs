# Trustless Settlement

In order to achive trustless settlement for Citrea, there needs to be opcode change(s).

Mainly, an opcode that can verify ZK proofs and a [covenant opcode](https://covenants.info) would be required to implement completely trustless Bitcoin settlement. Specifically, it is theoretically possible to implement a trustless bridge using OP_CAT opcode, which is disabled by Satoshi in 2010 due to a security concern. 

In the current architecture, BitVM/Clementine provides a trust-minimized settlement with 1-of-N honest minority trust assumption - which is a great improvement over the existing insecure implementations of sidechains. In case a suitable opcode is added to Bitcoin, Citrea will also adopt a cheaper/securer/faster/trustless solution if possible.