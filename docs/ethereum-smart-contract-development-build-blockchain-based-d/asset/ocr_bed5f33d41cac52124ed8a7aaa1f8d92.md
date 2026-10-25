```
├─ logs // The node logs
├─ node.conf // The node's configuration files
├─ persistence.mv.db // The node's database
└─ plugins // The CorDapps JARs installed on the node
```

A node can have four broad categories of functionality:

+   - **Network map**: This provides a way to resolve identities to physical node addresses and associated public keys
+   - **Notary**: This witnesses state spends and has the final say in whether a transaction is a double-spend or not
+   - **Oracle**: This links the ledger to the outside world by providing facts that affect the validity of transactions
+   - **Regular mode**: This starts protocols communicating with other nodes, notaries, and oracles and evolves their private ledger