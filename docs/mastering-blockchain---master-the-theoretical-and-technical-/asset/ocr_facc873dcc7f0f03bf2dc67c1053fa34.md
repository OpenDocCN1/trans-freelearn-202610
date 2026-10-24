# EIP 161

This proposal has been made to allow the removal of empty accounts from the Ethereum blockchain. This was put in place as response to a denial of service attack on the Ethereum blockchain whereby the attacker(s) created a large number of empty accounts. This was possible due to the low cost of account creation. This is called state trie clearing which will allow removal of empty accounts by making a CALL instruction to the accounts.

# EIP 160

This suggestion has been made to increase EXP Opcode cost. The core idea behind this improvement proposal is to make cost of EXP operation proportional to the complexity of the operation. This proposal has helped to thwart denial of service attacks that arose from invoking computationally expensive operations.

# EIP 155

This improvement proposal is made to eliminate the possibility of replaying a transaction from a different chain on the main live Ethereum blockchain. For example, if a transaction is made on the Ethereum network then it would be able to be replayed on Ethereum classic network; this EIP addresses this issue. Also, any transactions that have already been made on the test network should not be able to replay on the main live network.

All of the above-mentioned improvements have resulted in a hard fork and were implemented as a response to recent Ethereum denial of service attacks. Improvement proposals are a key mechanism to introduce new enhancement and protocol level bug fixes to Ethereum blockchain. There are about 11 improvement proposals implemented or proposed for Ethereum. All of these are documented on GitHub at https://github.com/ethereum/EIPs/.

# Other challenges

Apart from security and privacy, discussed in the previous chapter, there are several other hurdles that should be addressed before mainstream adoption of blockchains can be realized. These include regulation, government control, immature technology, integration with existing systems, and implementation costs.