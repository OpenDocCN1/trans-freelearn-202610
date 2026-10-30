Databases which do not include a JDBC driver can be selected. If one such driver is selected, add the driver zip or JAR file to the `CLASSPATH` variable in the `startWebLogic` script; else add the JAR or zip file to the server `lib` directory. A JDBC driver to a DBMS can be used, by specifying 'Other', and entering the driver class name manually to the console. Any Type 4 JDBC driver can be manually added to the classpath in the start-weblogic scripts, and need not to be put in the server or lib directory:

![](bbox=[0.44, 0.27, 0.56, 0.48])

In the **Transaction Options** window, the transaction attributes of the data source are specified. If a XA driver is selected, global transactions would be automatically supported with Two-Phase commit transaction protocol. A global, distributed, or XA transaction is one that involves multiple DBMSes or resources, and which require a 2-Phase commit protocol to ensure an all-or-nothing resolution of the transaction. A global transaction is managed by a Transaction Manager using JTA. For a non-XA data source to support global transactions, check the **Global Transactions** checkbox. Select the protocol which supports global transactions. Different transaction protocols are listed in following table:

| Transaction Protocol | Description |
|---|---|
| Logging Last Resource (LLR). | LLR optimization provides a better performance as compared to an XA JDBC driver, for insert, update, and delete operations. XA driver provides with a better performance for read operations. Recommended over a two-phase commit. |
| Two-Phase Commit. | Emulates participation in a global transaction using JTA. |
| One-Phase Commit. | This is the default setting. Only one resource may participate in the global transaction with one-phase commit. |