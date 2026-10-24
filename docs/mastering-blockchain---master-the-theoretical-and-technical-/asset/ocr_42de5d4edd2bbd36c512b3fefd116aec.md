+   13. Using the referenced output transactions to get input values, verify that each input value, as well as the sum, is in the allowed range of 0-21 million BTC.
+   14. Reject the transaction if the sum of input values is less than the sum of output values.
+   15. Reject the transaction if the transaction fee would be too low to get into an empty block.

# Blockchain

Blockchain is a public ledger of a timestamped, ordered, and immutable list of all transactions on the bitcoin network. Each block is identified by a hash in the chain and is linked to its previous block by referencing the previous block's hash.

In the following structure of a block, a block header is described, followed by a detailed diagram that provides an insight into the blockchain structure.

# The structure of a block

| Bytes | Name | Description |
|---|---|---|
| 80 | Block header | This includes fields from the block header described in the next section. |
| variable | Transaction counter | The field contains the total number of transactions in the block, including the coinbase transaction. |
| variable | Transactions | All transactions in the block. |

# The structure of a block header

| Bytes | Name | Description |
|---|---|---|
| 4 | Version | The block version number that dictates the block validation rules to follow. |
| 32 | previous block header hash | This is a double SHA256 hash of the previous block's header. |
| 32 | merkle root hash | This is a double SHA256 hash of the merkle tree of all transactions included in the block. |