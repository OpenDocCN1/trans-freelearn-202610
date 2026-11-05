+   - **Isolated** – When we withdraw money from the bank, it would be bad if our business partner was withdrawing at the same time, and we ended up with a negative balance.
+   - **Durable** – We expect completed transactions to survive hardware failures

Regarding atomicity, we have already practiced defined the beginning and end points for transactions by using @transactional and TransactionTemplate.

Spring Python supports propagation. Earlier, we stated that the default policy of @transactional is to start a new transaction (if none existed) and join an existing transaction (if one was already in progress). Spring Python conveniently lets us take the safe, atomic operations of withdraw and deposit, and combine them together into transfer, without having to interact with the SQL transaction APIs at all.

We also created another function, balance, to lookup the current balance of accounts. Since balance performs no updates, it doesn't require a transaction when run by itself. However, when called upon by an existing transaction, we want it to join in as if it was part of the transaction. This is accomplished by providing @transactional with a propagation override:

```
@transactional(["PROPAGATION_SUPPORTS"])
def balance(self, act):
    return self.dt.queryForObject("""
        SELECT BALANCE
        FROM ACCOUNT
        WHERE ACCOUNT_NUM = ?""",
        (act,), types.FloatType)
```

@transactional scans the list of transaction definitions. Currently, Spring Python supports the following definitions:

| Property | Description |
|---|---|
| PROPAGATION_REQUIRED | A transaction is required. If a current one exists, join it. Otherwise, start a new one. This is the default for @transactional. |
| PROPAGATION_SUPPORTS | A transaction is not required. This code can run inside or outside a transaction. |
| PROPAGATION_MANDATORY | A transaction is required. If a current one exists, join it. Otherwise, raise an exception. |
| PROPAGATION_NEVER | A transaction is not allowed. If a current one exists, raise an exception. Otherwise, run the code. |