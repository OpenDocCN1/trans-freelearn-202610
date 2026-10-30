While one transaction is modifying a database table, another transaction could be reading from the same table. The type of read can be dirty-read, a non-repeatable read, or a phantom read. A dirty-read occurs when a row has been modified by a transaction, but has not been committed, and is being read by a different transaction. If the transaction that modifies the row rolls back the transaction, then the value retrieved by the second transaction would be erroneous. A non-repeatable transaction occurs when one transaction reads a row while the other transaction modifies it. The first transaction re-reads the row obtaining a different value. A phantom read occurs when one transaction retrieves a result set with a WHERE condition, while the other transaction adds a row that meets the WHERE condition. The first transaction re-runs to generate a result set that has an additional row. The default transaction level can be obtained with the getTransactionLevel() method:

```
int transactionLevel=connection.getTransactionIsolation();
```

The different transaction isolation levels are listed in following table:

| Transaction Isolation Level | Description |
|---|---|
| TRANSACTION_NONE | Transactions are not supported. |
| TRANSACTION_READ_COMMITTED | Dirty-reads cannot be done. Non-repeatable reads and phantom reads can be done. |
| TRANSACTION_REPEATABLE_READ | Dirty reads and non-repeatable reads cannot be done. Phantom reads can be done. |
| TRANSACTION_SERIALIZABLE | Dirty-reads, non-repeatable reads and phantom reads cannot be done. |

The transaction isolation level can be set with the setTransactionIsolation(int level) method:

```
connection.setTransactionIsolation(level);
```

# Savepoint Interface

Savepoint is a new interface in JDBC 3.0 specification. A Savepoint is a point within a transaction up to which the changes made in the transaction are rolled back, if the transaction is rolled back with the rollback() method. All changes before the savepoint are implemented when a transaction is rolled back. A savepoint is set with the overloaded setSavepoint() method:

```
Savepoint savepoint=connection.setSavepoint();
Savepoint savepoint=connection.setSavepoint("savepointName");
```