# # Fee schedule

Gas is charged in three scenarios as a prerequisite to the execution of an operation:

+   - The computation of an operation
+   - For contract creation or message call
+   - Increase in the usage of memory

A list of instructions and various operations with the gas values has been provided previously in the chapter.

# # Messages

Messages, as defined in the yellow paper, are the data and value that are passed between two accounts. A message is a data packet passed between two accounts. This data packet contains data and value (amount of ether). It can either be sent via a smart contract (autonomous object) or from an external actor (externally owned account) in the form of a transaction that has been digitally signed by the sender.

Contracts can send messages to other contracts. Messages only exist in the execution environment and are never stored. Messages are similar to transactions; however, the main difference is that they are produced by the contracts, whereas transactions are produced by entities external (externally owned accounts) to the Ethereum environment.

A message consists of the components mentioned here:

+   1. Sender of the message
+   2. Recipient of the message
+   3. Amount of Wei to transfer and message to the contract address
+   4. Optional data field (Input data for the contract)
+   5. Maximum amount of gas that can be consumed

Messages are generated when CALL or DELEGATECALL Opcodes are executed by the contracts.