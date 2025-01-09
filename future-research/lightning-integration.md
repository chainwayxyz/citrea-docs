# Lightning Integration


Trustless atomic swaps between Citrea and Lightning Network are being actively implemented at the moment. This will allow users of Citrea to pay Lightning invoices directly from the Citrea network or on- and off-ramp from Citrea without needing to leverage the Bitcoin base layer.

More technically, for both ways of the swap the smart contract on Citrea simply emulates the Lightning's HTLC. An LSP (Lightning Service Provider) behaves as a proxy on Lightning and it is responsible for all cross-network interactions - with liquidity maintained on both sides.

![Lightning Atomic Swap Diagram](/.gitbook/assets/lightning_swap.png)

For the path from Lightning to Citrea, here's a basic explanation:
- Citrea user generates a hash from a preimage and sends it to Lightning user.
- A route to LSP from the Lightning user is formed.
- Regular Lightning payment procedure applies from the user to LSP.
- LSP then locks the amount that is send in CBTC to the smart contract on Citrea. This can only be unlocked with the preimage of that has that's been sent along the route.
- When Citrea user withdraws the CBTC, the preimage is revealed publicly. Everyone uses the preimage to spend their UTXO, and the swap is complete.

