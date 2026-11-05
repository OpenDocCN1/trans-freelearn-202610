Spring Python provides incredibly useful transaction context management, transaction API handling, and allows us clean demarcation of transactions.

As better definitions are added to Python's database specification for things like isolation, Spring Python will add more options to support it. This will increase our ability to cleanly declare the exact type of transaction needed to wrap our code.

# Applying transactions to non-transactional code

An important aspect of Spring Python is its non-invasive nature. This was demonstrated in great detail in the chapter that introduced aspect oriented programming. Spring Python provides a convenient, non-intrusive method interceptor that allows the demarcation of existing code.

This solves the problem mentioned earlier, where neither editing existing source code nor tangling our business logic with transaction management are acceptable.

+   1. Let's start with an alternative version of Bank class that has no transaction demarcation.

```
```python
class Bank(object):
    def __init__(self, connectionFactory):
        self.factory = connectionFactory:
        self.dt = DatabaseTemplate(self.factory)

    def balance(self, act):
        return self.dt.queryForObject("""
            SELECT BALANCE
            FROM ACCOUNT
            WHERE ACCOUNT_NUM = ?""",
            (act,), types.FloatType)

    def withdraw(self, amt, act):
        if (self.balance(act) > amt):
            rows = self.dt.execute("""
                update ACCOUNT
                set BALANCE = BALANCE - %s
                where ACCOUNT_NUM = %s""",
                (amt, act))
            if (rows == 0):
                raise Exception("Account %s does not exist." % act)
        else:
```
```