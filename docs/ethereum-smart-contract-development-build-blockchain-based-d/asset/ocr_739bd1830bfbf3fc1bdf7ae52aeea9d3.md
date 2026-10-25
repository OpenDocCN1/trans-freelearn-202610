# Restricting access

Access restriction is a common functionality for smart contracts. In earlier chapters, we discussed how external contracts exploited the access related vulnerability in the Parity library contract to self-destruct the entire wallet library and block $300 million-worth of ethers. We can never restrict any human or computer from reading the contract's state. We can make it a bit difficult by using encryption.

The only viable thing for a developer to keep in mind is that the state of a contract is public, and then design it in a way without problems. We can, however, restrict the number of entities allowed to make modifications to a contract by using function modifiers on the entity addresses. Such addresses can be maintained on an **access control list (ACL)**, but with a caveat, that the read-only access will be available to anyone for a public blockchain.

Modifiers are used to change the body of a function. If such modifiers are used, they prepend a check that only passes if the function is called from a certain address.

This design implementation is a kind of structural design pattern.