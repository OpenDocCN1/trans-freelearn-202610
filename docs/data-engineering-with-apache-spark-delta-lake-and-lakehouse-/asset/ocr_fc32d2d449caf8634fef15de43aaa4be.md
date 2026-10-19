# Understanding isolation levels

In Delta Lake, the isolation level of a table defines the degree to which the transaction must be isolated from modifications that are being made by concurrent transactions. There are two isolation levels, as follows:

+   - The Serializable isolation level is strong, which means the write and read operations exist in a serial sequence.
+   - The WriteSerializable isolation level is the default. In this isolation level, only the write operations exist in a serial sequence.

We will check them out as follows:

+   1. The isolation level can be queried for every transaction in history:

```
%sql
SELECT version, operation, isolationLevel
from (DESCRIBE HISTORY store_orders);
```

This results in the following output:

![](bbox=[0.22, 0.55, 0.78, 0.75])

Figure 6.40 – Transaction history isolationLevel

+   2. Although the default isolation level for delta tables is WriteSerializable, it can be changed as desired. We can alter the isolation of the store_orders table to the Serializable isolation level:

```
%sql
ALTER TABLE store_orders SET TBLPROPERTIES ('delta.isolationLevel' = 'Serializable')
```