# Trustless Atomic Swaps

Trustless atomic swaps allow users to on- and off-ramp between Citrea and Bitcoin without using the peg mechanism. They are being actively implemented through third parties and live on Citrea Testnet, such as [Citrex](https://citrex.xyz) and [Garden Finance](https://garden.finance).

A very basic atomic swap works as follows:

* A smart contract on Citrea is used to lock some amount of cBTC. This smart contract also utilizes BitcoinLightClient on Citrea to verify Bitcoin transactions on-chain (see below).
* User A locks some cBTC into the smart contract, and demands an equivalent amount of Bitcoin in return.
* Some User B sees the request, and locks the equivalent amount of Bitcoin into the desired address of User A in the Bitcoin network.
* Upon finalization of the transaction, user B can then submit the transaction details to the Atomic Swap smart contract on Citrea, which verifies the transaction and lets user B claim the locked cBTC.

Here's a basic diagram of the process:

![Basic Atomic Swap Diagram](https://4199298141-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FtFU3ZD7rSzMi2uz6wz9W%2Fuploads%2Fgit-blob-702bc24b767e5b1416577f815188a8a152d5e6ae%2Fatomic_swap.png?alt=media)
