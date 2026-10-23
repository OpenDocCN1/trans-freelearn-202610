In the meantime, in another session, execute the following statements:

```
-- session 2
forumdb=> BEGIN;
BEGIN
forumdb=> SELECT txid_current(), txid_current_snapshot();
txid_current | txid_current_snapshot
---------------+------------------------
  4931 | 4930:4930:
(1 row)
forumdb=> UPDATE tags SET tag = lower( tag );
-- LOCKED!!!
```

Transaction 4931 is locked because PostgreSQL cannot decide which data manipulation to apply. On one hand, transaction 4930 applies uppercase to all the tags, but at the same time, transaction 4931 applies lowercase to the very same data.

Since the two changes conflict, and the final result (that is, the result that will be consolidated in the database) depends on the exact order in which changes will be applied (and in particular on the last one applied), PostgreSQL cannot allow both transactions to proceed. Therefore, since 4930 applied the changes before 4931, the latter is suspended, waiting for transaction 4930 to complete either with success or failure. As soon as you end the first transaction, the second one will be unblocked (showing the message status for the UPDATE statement):

```
-- session 1
forumdb=> COMMIT;
COMMIT

-- session 2
UPDATE 6
-- unblocked, can proceed further ...
forumdb=>
```

Therefore, MVCC is not a silver bullet against lock usage but allows better concurrency in the overall usage of the database.