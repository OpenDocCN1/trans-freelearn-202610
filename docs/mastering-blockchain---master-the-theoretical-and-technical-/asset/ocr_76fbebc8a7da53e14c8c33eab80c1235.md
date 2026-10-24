# Distributed ledger

Blockchain and world state are two main elements of the distributed ledger. Blockchain is simply a linked list of blocks (as introduced in earlier chapters) and world ledger is a key-value database. This database is used by smart contracts to store relevant states during execution by the transactions. The blockchain consists of blocks that contain transactions. These transactions contain chaincode, which runs transactions that can result in updating the world state. Each node saves the world state on disk in RocksDB. The following diagram shows a typical block in the Hyperledger Fabric with the relevant fields:

![](bbox=[0.411, 0.285, 0.59, 0.475])

The fields shown in the preceding diagram are as follows:

+   - **Version:** Used for keeping track of changes in the protocol.
+   - **Timestamp:** Timestamp in UTC epoch time, updated by block proposer.
+   - **Transaction hash:** This field contains the Merkle root hash of the transactions in the block.
+   - **State hash:** This is the Merkle root hash of the world state.
+   - **Previous hash:** This is the previous block's hash, which is calculated after serializing the block message and then creating the message digest by applying the SHA3 SHAKE256 algorithm.
+   - **Consensus metadata:** This is an optional field that can be used by the consensus protocol to provide some relevant information about the consensus.
+   - **Non-Hash data:** This is some metadata that is stored with the block but is not hashed. This feature makes it possible to have different data on different peers. It also provides the ability to discard data without any impact on the blockchain.