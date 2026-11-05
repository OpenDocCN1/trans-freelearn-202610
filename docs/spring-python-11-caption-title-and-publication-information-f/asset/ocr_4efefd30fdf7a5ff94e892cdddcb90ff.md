This policy indicates that a user can only access /edit* URLs if he has ROLE_EDIT. For any other web path, the user must have ROLE_READ. Note: don't forget that /login* is excluded from this filter.

+   - Let's add a login page to our view layer
+   - Let's add the code to create and process a login page in our controller

```
```python
@cherrypy.expose
def login(self, fromPage="/", login="", password="",
errorMsg=""):
    return self.controller.getLoginPage(
        fromPage, login, password, errorMgr)
```
```

```
```python
def getLoginPage(self,fromPage="/"
",login="",password="",errorMsg=""):
    if login != "" and password != "":
        try:
            self.authenticate(login, password)
            return [self.redirector.redirect(fromPage)]
        except AuthenticationException, e:
            return [self.redirector.redirect(
                "?login=%s&errorMsg=Username/password failure" %
                login)]

    results = """
<html>
    <head>
        <title>Spring Python book demo</title>
    </head>
    <body>
        <form method="POST" action="">
            Login: <input type="text" name="login" value="%s"
size="10"/><br/>
            Password: <input type="password" name="password" size="10"/>
><br/>
            <input type="hidden" name="fromPage" value="%s"/><br/>
            <input type="submit"/>
        </form>
        <a href="http://springpythonbook.com">Spring Python book</a>
    </body>
</html>
    """ % (login, fromPage)
```
```