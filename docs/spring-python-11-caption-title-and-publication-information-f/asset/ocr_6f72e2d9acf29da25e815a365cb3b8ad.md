Each row includes a link to view the history for that account, and at the bottom of the screen are links to the other new functions.

# Refining the ability to open an account

We altered the back end of openAccount by marking new accounts with a state of OPEN.

```
def open_account(self, description):
    self.dt.execute("""
        INSERT INTO account
        (description, balance, owner, status)
        VALUES
        (?, 0.0, ?, 'OPEN')""", (description,\
            SCH.getContext().authentication.username))

    self.log("TX", self.get_latest_account(), "Opened account %s"
% description)
```

We also added the extra step of writing a transaction log entry. To support writing the log entry, the controller must retrieve the latest account created for this customer.

```
def get_latest_account(self):
    return self.dt.query_for_long("""
        SELECT max(id)
        FROM account
        WHERE owner = ?
        AND status = 'OPEN'""", \
        (SCH.getContext().authentication.username,))
```

open_account, along with several of the other controller operations uses the following code to write log entries.

```
def log(self, type, account, message):
    self.dt.execute("""
        INSERT INTO log
        (type, account, message)
        values
        (?, ?, ?)""", \
        (type, account, message))
```