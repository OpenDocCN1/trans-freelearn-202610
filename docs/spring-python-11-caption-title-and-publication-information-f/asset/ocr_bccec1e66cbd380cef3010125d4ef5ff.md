```
DictionaryRowMapper() [0] ["balance"]
if float(balance) < amount:
    raise BankException("Insufficient balance in acct %s" % id)
self.dt.execute("""
    UPDATE account
    SET balance = balance - ?
    WHERE id = ?""", (amount, id))
self.log("TX", id, "Withdrew %s" % amount)
```

An important point here is that there is no redirect operation happening. This method isn't aware of the web layer above it. While it would be easy to raise a cherrypy.HTTPRedirect message, it is best left to the view layer. This also impacts transfer operations by causing an exception to be thrown if there are insufficient funds.

2. Define a simple BankException class.

```
class BankException(Exception):
    pass
```

3. Modify the beginning of the withdraw web method by adding a try-except block that either redirects with a success message or an error message.

```
@cherrypy.expose
def withdraw(self, id="", amount=""):
    if id != "" and amount != "":
        try:
            self.controller.withdraw(id, float(amount))
            raise cherrypy.HTTPRedirect(\
                "/?message=Successfully withdrew %s" % amount)
        except BankException, e:
            raise cherrypy.HTTPRedirect("/?message=%s" % e)
```

The rest of this view-level method is unchanged. This provides a clear behavior pattern based on any exception thrown down in the controller's withdraw operation.

4. Modify the beginning of the transfer web method by adding a try-except block that either redirects with a success message or an error message.

```
@cherrypy.expose
def transfer(self, amount="", source="", target=""):
    if amount != "" and source != "" and target != "":
        try:
            self.controller.transfer(amount, source, target)
            raise cherrypy.HTTPRedirect(\
                "/?message=Successful transfer")
        except BankException, e:
            raise cherrypy.HTTPRedirect("/?message=%s" % e)
```