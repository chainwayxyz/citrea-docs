# Security Properties

In terms of security properties, Citrea enables improvements in several areas compared to current Bitcoin scaling solutions.

### Validity

*Only correct state updates are accepted, and there's a way to validate on Bitcoin.*

Citrea executes transactions off-chain in an EVM-compatible environment using RISCZero zkVM, and then for every batch it posts a commitment and a ZK proof to Bitcoin. This proof attests to thousands of Citrea blocks at a time and includes the initial state root, the latest state root, and the compact state diff of the corresponding batch. Therefore, anyone with a Citrea node and a Bitcoin node can independently verify the batch result.&#x20;

The ZK circuits of Citrea also include blockspace proving - it scans Bitcoin for on-chain commitments as well. Ordering is preserved inside the proof, and the proof itself shows the prover generated the valid proof for a valid state transition, as it cannot do so with an invalid state transition.&#x20;

As for the verification on Bitcoin L1 itself, Bitcoin Script cannot natively verify ZK Proofs. Citrea uses BitVM to *optimistically verify* ZK proofs for the Clementine bridge. For more details, please refer to the [Clementine](https://docs.citrea.xyz/essentials/clementine-trust-minimized-bitcoin-bridge) page.

### Data Availability

*Anyone can construct the Citrea state using Bitcoin only.*

Citrea uses **Bitcoin as its DA (Data Availability) layer**. Instead of posting full transactions, it publishes **state differences (state diffs)** plus the latest state root to Bitcoin. A full node can rebuild the entire rollup by scanning Bitcoin blocks, extracting valid batches, and applying the posted state diffs - so the rollup remains reconstructible “as long as Bitcoin lives.”&#x20;

Posting state diffs is a different mechanism compared to traditional sidechains, as sidechains post a very small cryptographic commitment only, which does not let anyone recover the current state of the sidechains.&#x20;

The trade-off is Bitcoin’s limited bandwidth and fees. Using Bitcoin as the DA inherits strong liveness, recoverability, and auditability (no DA committees to trust), but it also inherits Bitcoin’s blockspace & fee constraints.

### Re-Org Resistance

*You cannot reorder or rewrite Citrea history without re-orging Bitcoin.*

Every Citrea block header is baked into the state transition function. Once a batch containing those headers is **pushed to Bitcoin and confirmed**, **re-orging Citrea would require re-orging Bitcoin**; practically, after \~6 BTC confirmations, such a re-org is considered highly unlikely.

Block producers and nodes operate with **direct access to the Bitcoin network**, and nodes validate Citrea blocks against the referenced Bitcoin blockhash. This tight coupling means Citrea can’t fork independently for already-anchored history - the L2’s practical finality horizon tracks Bitcoin’s, while pre-anchor head movement is bounded by the usual challenge/anchoring cadence.

### Force Transactions

After mainnet launch, Citrea will implement a force-inclusion mechanism from its Bitcoin anchoring and blockspace-proving design: a user can post a minimal L1 commitment on Bitcoin, and Citrea nodes will be required to scan Bitcoin and treat such commitments as must-include inputs. Ordering will be preserved inside the ZK proof - any batch that skips or reorders a valid force transaction observed before the cutoff cannot generate a valid proof and will be rejected during verification on Bitcoin.

If block producers fail or refuse service beyond that window, users will be able to unilaterally progress or exit. Because Citrea uses Bitcoin for data availability and publishes state diffs plus the latest state root, any full node can reconstruct the state and produce Merkle witnesses for balances/state in that scenario. This will enable users to finalize withdrawals or enforce effects without sequencer cooperation, preserving liveness and fund recoverability while inheriting Bitcoin’s auditability and constraints.

{% hint style="warning" %}
The force transaction feature will be implemented post-mainnet.
{% endhint %}
