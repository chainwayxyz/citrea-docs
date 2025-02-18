# Mempool

Citrea has a private mempool (memory pool) as a temporary staging area for pending transactions before they are included in a block. 

Once a transaction is submitted, some basic checks are as follows:
- Is there enough space in the mempool? (see configs section below)
- Does it fit into account limits? (see configs section below)
- Does the transaction have a valid nonce that is bigger or equal than the current nonce?
- Does the address have enough balance?

If the transaction passes these checks, it is added to the mempool. 

#### Configuration

Citrea uses a private mempool, inaccessible via RPC, which prioritizes transactions based on their fee.

Citrea mempool has custom [SubPools](https://reth.rs/docs/reth_transaction_pool/enum.SubPool.html#variants) that uses `reth-transaction-pool` architecture. Namely:
- **Queued**: Includes transactions not ready to be included in the next block due to lack of funds, nonce, etc.
- **BaseFee**: Includes transactions not ready to be included in the next block due to insufficient base fee.
- **Pending**: Includes transactions ready to be included in the next block.

For **Queued**, **BaseFee**, and **Pending** pools, Citrea has the 10x configuration limits compared to default values of Ethereum: **100,000** transactions per subpool with **200 MB** limit.

{% hint style="note" %}
> Citrea does not have a Blob pool as it does not support blob transactions at the moment. 
{% endhint %}

One more custom mempool configuration is the number of transaction slots per account, `max_account_slots`. Citrea has a maximum of **64 slots per account** in the mempool to prevent spamming.
