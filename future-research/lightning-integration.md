# Lightning Integration

Trustless atomic swaps between Citrea and Lightning Network are being actively implemented at the moment through third party applications, such as [Citrex](https://citrex.xyz). This will allow users of Citrea to pay Lightning invoices directly from the Citrea network or on- and off-ramp from Citrea without needing to leverage the Bitcoin base layer.

More technically, for both ways of the swap the smart contract on Citrea simply emulates the Lightning's HTLC. An LP (Liquidity Provider) behaves as a proxy on Lightning and it is responsible for all cross-network interactions - with liquidity maintained on both sides.

![Lightning Atomic Swap Diagram](/.gitbook/assets/lightning_atomic_swap.png)

For the path from Lightning to Citrea, here's a basic explanation:
- The receiver from the LN side creates an invoice and sends it to the sender on Citrea side.
- Sender locks cBTC on the HTLC contract with the invoice.
- LP makes the payments on LN on behalf of the sender.
- The preimage of the invoice gets revealed to the LP upon payment.
- LP uses the preimage to claim the cBTC from the HTLC contract.

