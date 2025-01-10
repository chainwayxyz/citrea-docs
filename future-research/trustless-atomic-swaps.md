# Trustless Atomic Swaps with Bitcoin

Trustless atomic swaps between Citrea and Bitcoin are being actively implemented at the moment. These swaps would allow users to on- and off-ramp from Citrea without using the peg mechanism.

A very basic atomic swap works as follows:

- There's a smart contract on Citrea that is used to lock some amount of cBTC. This smart contract also utilizes BitcoinLightClient on Citrea to verify Bitcoin transactions on chain (see below).
- User A locks some cBTC into the smart contract, and demands an equivalent amount of Bitcoin in return.
- Some User B sees the request, and locks the equivalent amount of Bitcoin into the desired address of User A in the Bitcoin network.
- Upon finalization of the transaction, user B can then submit the transaction details to the Atomic Swap smart contract on Citrea, which verifies the transaction and lets user B to claim the locked cBTC.

![Basic Atomic Swap Diagram](/.gitbook/assets/atomic_swap.png)

In the flow above, the atomic swap is basically completed trustlessly as there are no intermediaries involved. Different implementation of atomic swaps can use preimages and HTLCs to make the swap more robust.

There are several native atomic swaps projects being implemented at the moment, to be revealed in the future.