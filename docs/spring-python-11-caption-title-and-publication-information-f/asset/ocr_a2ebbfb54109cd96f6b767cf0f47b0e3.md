# Creating a banking application

Until now, the various chapters have shown sample code that involved designing a wiki engine. For this chapter, a banking application will be used as the basis for code examples. Banks must be able to move money around and not lose a single penny, maintaining a high integrity. This provides a simple problem space to demonstrate writing transactional code.

+   1. First, let's write a simple transfer operation that transfers a certain amount of money from one account to another without any concept of a transaction.
+   
+   2. Let's combine this operation with the transaction pattern we looked at earlier, to give our bank some integrity.

```
def transfer(transfer_amt, source_act, target_act):
    cursor = conn.cursor()
    cursor.execute("""
        update ACCOUNT
        set BALANCE = BALANCE - %s
        where ACCOUNT_NUM = %s""",
        (transfer_amt, source_act))
    cursor.execute("""
        update ACCOUNT
        set BALANCE = BALANCE + %s
        where ACCOUNT_NUM = %s""",
        (transfer_amt, target_act))
    cursor.close()
```

Let's assume we are transferring $10,000 from the SAVINGS account to the CHECKING account. In our business logic, we accomplish this by first withdrawing $10,000 from the SAVINGS account and then depositing $10,000 into the CHECKING account.

Imagine if there was some system error that happened after we had withdrawn from the SAVINGS account that caused the system to restart. Having never deposited the transfer amount into the CHECKING account, the bank would have leaked $10,000 to nowhere. How long would you keep your money in a bank like that? Probably not for long!

```
def transfer(transfer_amt, source_act, target_act):
    conn.commit()
    try:
        cursor = conn.cursor()
        cursor.execute("""
            update ACCOUNT
            set BALANCE = BALANCE - %s
            where ACCOUNT_NUM = %s"""
```