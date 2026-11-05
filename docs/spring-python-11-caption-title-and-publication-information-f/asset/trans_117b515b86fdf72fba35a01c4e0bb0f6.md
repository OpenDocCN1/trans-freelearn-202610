
```
@cherrypy.expose
def withdraw(self, id="", amount=""):
    if id != "" and amount != "":
        self.controller.withdraw(id, float(amount))
    raise cherrypy.HTTPRedirect("/?message=Successfully withdrew %s" % amount)

    results = """
        <h2>Withdraw Money</h2>
        <form method="POST" action="">
        <table border="1">
            <tr>
                <td>Amount</td>
                <td><input type="text" name="amount"/></td>
            </tr>
            <tr><td colspan="2">
    """
    for account in self.controller.get_accounts(SCH.getContext().authentication.username):
        results += """
            <input type="radio" name="id" value="%s">%s - %s</input><br/>
            """ % (account["id"], account["id"], account["description"])

        results += """
            </td></tr>
            <tr><td colspan="2"><input type="submit"/></td></tr>
        </form>
    """

    return results
```

当用户点击提交时，withdraw 会委托给控制器。

```
def withdraw(self, id, amount):
    self.dt.execute("""
        UPDATE account
        SET balance = balance - ?
        WHERE id = ?""", (amount, id))
    self.log("TX", id, "Withdrew %s" % amount)
```

## 添加存款功能

我们在 SpringBankView 中添加了 `deposit`，以便客户向现有账户存款。

![](img/bcf886d8b665372ee8afbb620d4273bb_207_0.png)

```
@cherrypy.expose
def deposit(self, id="", amount=""):
    if id != "" and amount != "":
        self.controller.deposit(id, float(amount))
    raise cherrypy.HTTPRedirect("/?message=Successfully deposited %s" % amount)

    results = """
        <h2>Deposit Money</h2>
        <form method="POST" action="">
        <table border="1">
            <tr>
                <td>Amount</td>
                <td><input type="text" name="amount"/></td>
            </tr>
            <tr><td colspan="2">
    """

    for account in self.controller.get_accounts(SCH.getContext().authentication.username):
        results += """
            <input type="radio" name="id" value="%s">%s - %s</input><br/>
            """ % (account["id"], account["id"], account["description"])

        results += """
            </td></tr>
            <tr><td colspan="2"><input type="submit"/></td></tr>
        </form>
    """

    return results
```

当用户点击提交时，deposit 会委托给控制器。

```
def deposit(self, id, amount):
    self.dt.execute("""
        UPDATE account
        SET balance = balance + ?
        WHERE id = ?""", (amount, id))
    self.log("TX", id, "Deposited %s" % amount)
```

## 添加转账功能

我们在 SpringBankView 中添加了 transfer，以便客户在账户之间转账。

![](img/bcf886d8b665372ee8afbb620d4273bb_208_0.png)

```
@cherrypy.expose
def transfer(self, amount="", source="", target=""):
    if amount != "" and source != "" and target != "":
        self.controller.transfer(amount, source, target)
        raise cherrypy.HTTPRedirect("/?message=Successful transfer")

    results = """
        <h2>Transfer Money</h2>
        <form method="POST" action="">
        <table border="1">
            <tr>
                <td>Amount</td>
                <td><input type="text" name="amount"/></td>
            </tr>
    """

    accounts = self.controller.get_accounts(SCH.getContext().authentication.username)

    for param in ["source", "target"]:
        results += '<tr><td>%s</td><td>' % (param[0].upper() + param[1:])
        for account in accounts:
            results += """
                <input type="radio" name="%s" value="%s">%s - %s</input><br/>
            """ % (param, account["id"], account["id"], account["description"])
            results += "</td></tr>"

        results += """
            <tr><td colspan="2"><input type="submit"/></td></tr>
        </form>
        """

    return results
```

当用户点击提交时，transfer 会委托给控制器。

```
def transfer(self, amount, source, target):
    self.withdraw(source, amount)
    self.deposit(target, amount)
```

transfer 方便地复用了 withdraw 和 deposit。

## 显示账户历史

最后添加的面向客户的功能是查看账户历史的能力。

![](img/bcf886d8b665372ee8afbb620d4273bb_210_0.png)

```
@cherrypy.expose
def history(self, id):
    results = """
        <table border="1">
    """

    for entry in self.controller.history(id):
        results += """
            <tr><td>%s</td><td>%s</td></tr>
        """ % (entry["date"], entry["message"])

    results += """
        </table>
    """

    return results
```

当用户点击提交时，history 会委托给控制器。

```
def history(self, id):
    return self.dt.query("""
        SELECT type, date, message
        FROM log
        WHERE log.account = ?
        AND log.type = 'TX'""", (id,),
        DictionaryRowMapper())
```

## 客户功能的问题

在此版本的功能中，我们已经初步实现了一些有用的银行功能。然而，仍有一些问题需要处理。

- 没有足够的保护措施防止其他客户访问 Alice 的账户并转移资金。
- 从账户取款没有透支保护。这也会影响转账，因为它们复用了 withdraw 函数。一个简单的解决方案是，如果请求金额超过余额则失败。
- 转账应该是事务性的，以避免资金泄漏。

## 保护 Alice 的账户

如果我们以 Alice 的身份登录，可以查看 Checking 账户的历史记录。

![](img/bcf886d8b665372ee8afbb620d4273bb_211_0.png)

这是可以的。然而，如果有另一个客户，安全性可能不如你想象的那样。让我们调整上下文，使另一个客户 "Dave" 存在。

```
@Object
def user_details_service(self):
    user_details_service = InMemoryUserDetailsService()
    user_details_service.user_dict = {
        "alice": ("alicespassword", ["ROLE_CUSTOMER"], True),
        "bob": ("bobspassword", ["ROLE_MGR"], True),
        "carol": ("carolspassword", ["ROLE_SUPERVISOR"], True),
        "dave": ("davesspassword", ["ROLE_CUSTOMER"], True)
    }
    return user_details_service
```

## 案例研究 I – 将 Spring Python 与您的 Web 应用程序集成

查看 Alice 的账户历史后，我们可以轻松地将 URL 从浏览器复制到剪贴板，注销，然后以 Dave 的身份重新登录。

![](img/bcf886d8b665372ee8afbb620d4273bb_212_0.png)

Dave 目前还没有账户。然而，Dave 可以粘贴该 URL 并轻松查看 Alice 的账户历史。

![](img/bcf886d8b665372ee8afbb620d4273bb_212_1.png)

这是因为这两个用户都拥有 ROLE_CUSTOMER。这意味着我们需要编写一个自定义的 AccessDecisionVoter 来决定某个记录是否可以被当前用户查看。

安全章节向我们展示了如何编写自定义身份验证器。在这种情况下，用户已经通过身份验证。我们需要的是对授权的适当处理。我们的任务要求确认当前登录的用户是否有权限查看当前账户。

- 1. 让我们编写自己的 AccessDecisionVoter。

```
class OwnerVoter(AccessDecisionVoter):
    def __init__(self, controller=None):
        self.controller = controller

    def supports(self, attr):
        """This voter will support a list."""
        if isinstance(attr, list) or \
            (attr is not None and attr == "OWNER"):
            return True
        else:
            return False

    def vote(self, authentication, invocation, config):
        """Grant access if any of the granted
        authorities matches any of the required roles.
        """
        results = self.ACCESS_ABSTAIN
        for attribute in config:
            if self.supports(attribute):
                results = self.ACCESS_DENIED
                id = cgi.parse_qs(
                    invocation.environ["QUERY_STRING"])["id"][0]
                if self.controller.get_username(id) == \
                    authentication.username:
                    return self.ACCESS_GRANTED

        return results
```

- OwnerVoter 需要一个 supports 方法来确定是否进行投票。在这种情况下，如果以列表形式给出安全配置，并且其中一个列表值是 OWNER，则会投票。
- 为了投票，投票器会提供用户身份验证凭据的副本，以及对调用对象的句柄，其中包含所有 Web 请求参数和安全角色配置。
