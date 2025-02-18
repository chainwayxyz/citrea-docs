# Soft Confirmations

Issued by the sequencer, soft confirmations in Citrea provide soft-finality for transactions in a block, offering users rapid feedback while the rollup awaits finalization on the DA layer. They represent interim state updates within the Citrea rollup.

#### Structure

Each soft confirmation encapsulates essential information about a block:

-   **Rollup Block Information:** The information of corresponding rollup block.
-   **Previous Hash**: The hash of the preceding soft confirmation, forming a chain of blocks that prevents replay attacks and ensures correct ordering.
-   **DA Block Information**: The height, hash, and the transaction commitment data of the associated DA block on Bitcoin.
-   **L1 Fee Rate**: The fee rate on Bitcoin L1, relevant for calculating transaction costs.
-   **Transactions**: Transaction data included in the block.
-   **State Root**: The root hash of the rollup state after applying the transactions in this soft confirmation.
-   **Signature**: The sequencer's signature on the soft confirmation data.
-   **Public Key**: The sequencer's public key used to verify the signature.
-   **Deposit Data**: Information about deposit transactions from the BitVM-based Clementine bridge.
-   **Timestamp**: The timestamp of the block, recording when it was produced by the sequencer.

The structure above is signed and distributed to the network. 

Soft confirmations form the basis for [sequencer commitments](./sequencer-commitments.md), which are published to the DA layer for finalization.
