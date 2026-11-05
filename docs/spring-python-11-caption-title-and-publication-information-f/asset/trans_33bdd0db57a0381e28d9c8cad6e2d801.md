
在 `transfer` 函数内部，我们定义了类 `TxDefinition`。然后在调用 `tx_template.execute()` 时实例化它。

- 此内部类定义了一个方法：
  `doInTransactionWithoutResult`，其中包含我们的业务逻辑。
- 为避免 `transfer` 中传入的 `self` 与 `TxDefinition` 中传入的 `self` 混淆，`doInTransactionWithoutResult` 将其第一个参数命名为 `s`。
- 因为 `transfer` 不期望返回任何内容，所以它使用 `TransactionCallbackWithoutResult` 作为基类。对于返回值，请改用 `TransactionCallback`/`doInTransaction`。

2. 接下来，让我们调整 IoC 配置以处理这些更改。

```
from springpython.database.transaction import *
class BankAppConfig(PythonConfig):
    def __init__(self, factory):
        PythonConfig.__init__(self)
        self.factory = factory

    @Object
    def bank(self):
        return Bank(self.factory)
```

3. 这不应影响用于运行我们的应用程序并执行相同转账的代码。

```
if __name__ == "__main__":
    from springpython.context import ApplicationContext
    ctx = ApplicationContext(BankAppConfig(
        Sqlite3ConnectionFactory("/path/to/sqlite3db")))
    service = ctx.get_object("bank")
    service.transfer(10000.0, "SAVINGS", "CHECKING")
```

## 不使用 IoC 容器进行配置

在这种情况下，我们的 IoC 配置相当简单。我们可以在没有它的情况下编写应用程序。只需记住：IoC 在测试、模拟以及能够替换关键对象等方面提供了有用的帮助。

我们首先重写启动脚本，使其不需要任何 IoC 容器。

```
if __name__ == "__main__":
    service = Bank(Sqlite3ConnectionFactory("/path/to/sqlite3db"))
    service.transfer(10000.0, "SAVINGS", "CHECKING")
```

因为我们的 Bank 是使用简单的构造函数注入编写的，所以为了在没有容器的情况下运行它，无需对其进行更改。由于我们以编程式的方式使用 TransactionTemplate，因此没有使用 IoC 容器的要求。这为开发人员提供了一个机会，可以纯粹为了事务功能而评估 Spring Python，而无需同时尝试 IoC 容器。

但重要的是要记住，本书中已经展示了 IoC 价值的多个示例，还有更多即将到来。

## @transactional 与 编程式

Spring 的方式包括为开发人员提供选择。为了选择合适的方法，以下是一些优缺点：

- @transactional
  - 优点：
    - 使用单行定义事务。
    - 清晰、简洁、易读。
  - 缺点：
    - 需要 IoC 来连接 AutoTransactionalObject。
    - 需要编辑现有代码。

- programmatic
  - 优点：
    - 明确显示事务行为发生的位置。
    - 不需要 IoC。
  - 缺点：
    - 重新引入代码纠缠。
    - 需要编辑现有代码。

如果这两种解决方案中的任何一种的缺点都是不可接受的，还有第三种选择：从 IoC 容器内部声明事务。这允许在不产生代码纠缠且无需编辑已有代码的情况下轻松地用事务包装业务代码。我们将在本章后面演示这一点。但首先让我们来看一下添加新函数。

## 使新函数与现有事务良好协作

到目前为止，我们已经成功构建了一个银行，它只做一件事：转账。如同任何软件项目一样，我们通常需要扩展功能。随着我们对 Bank 进行修改，我们希望在不危及现有事务的情况下添加新事务。Spring Python 的事务管理使这变得非常简单。

在本节中，我们将回到我们的 @transactional Bank 并添加一些新功能。

1. 让我们从 transfer 函数中提取 withdraw 函数和 deposit 函数。

```
class Bank(object):
    def __init__(self, connectionFactory):
        self.factory = connectionFactory
        self.dt = DatabaseTemplate(self.factory)

    def withdraw(self, amt, act):
        self.dt.execute("""
            update ACCOUNT
            set BALANCE = BALANCE - %s
            where ACCOUNT_NUM = %s""",
            (amt, act))

    def deposit(self, amt, act):
        self.dt.execute("""
            update ACCOUNT
            set BALANCE = BALANCE + %s
            where ACCOUNT_NUM = %s""",
            (amt, act))

    @transactional
    def transfer(self, transfer_amt, source_act, target_act):
        self.withdraw(transfer_amt, source_act)
        self.deposit(transfer_amt, target_act)
```

通过将两个 SQL 语句移入单独的函数中，我们已将 transfer 很好地定义为先 withdraw 后 deposit。我们现在准备将这两个新函数提供给我们的客户。你注意到这里有什么问题吗？这些函数本身在事务方面安全吗？如果另一个银行操作尝试重用这些原语会怎样？

2. 使用 @transactional 保护 withdraw 和 deposit 方法。

```
class Bank(object):
    def __init__(self, connectionFactory):
        self.factory = connectionFactory
        self.dt = DatabaseTemplate(self.factory)

    @transactional
    def withdraw(self, amt, act):
        self.dt.execute("""
            update ACCOUNT
            set BALANCE = BALANCE - %s
            where ACCOUNT_NUM = %s""",
            (amt, act))

    @transactional
    def deposit(self, amt, act):
        self.dt.execute("""
            update ACCOUNT
            set BALANCE = BALANCE + %s
            where ACCOUNT_NUM = %s""",
            (amt, act))

    @transactional
    def transfer(self, transfer_amt, source_act, target_act):
        self.withdraw(transfer_amt, source_act)
        self.deposit(transfer_amt, target_act)
```

我们代码中唯一的差异就是用 @transactional 标记 withdraw 和 deposit。

3. 现在让我们通过在执行 SQL 之前和之后进行一些检查，为我们的 Bank 增加一层完整性。

```
class Bank(object):
    def __init__(self, connectionFactory):
        self.factory = connectionFactory
        self.dt = DatabaseTemplate(self.factory)

    @transactional(["PROPAGATION_SUPPORTS"])
    def balance(self, act):
        return self.dt.queryForObject("""
            SELECT BALANCE
            FROM ACCOUNT
            WHERE ACCOUNT_NUM = ?""",
            (act,), types.FloatType)

    @transactional
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
                raise Exception("Account %s has insufficient funds." % act)

    @transactional
    def deposit(self, amt, act):
        rows = self.dt.execute("""
            update ACCOUNT
            set BALANCE = BALANCE + %s
            where ACCOUNT_NUM = %s""",
            (amt, act))
        if (rows == 0):
            raise Exception("Account %s does not exist." % act)

    @transactional
    def transfer(self, transfer_amt, source_act, target_act):
        self.withdraw(transfer_amt, source_act)
        self.deposit(transfer_amt, target_act)
```

我们做了一些更改，使我们的应用程序开始看起来像一个真正的银行。如下所示：

- 我们创建了一个 `balance` 函数，允许我们查询账户的余额
- `withdraw` 函数现在会检查余额，以确保有足够的金额可供提取
- `withdraw` 函数验证是否更新了一行数据，以确认被提取的账户是真实的
- `deposit` 函数验证是否更新了一行数据，以确认被存款的账户是真实的

## Spring Python 如何让我们定义事务的 ACID 属性

如前所述，事务的 ACID 属性如下：

- **Atomicity** – 我们需要保证所有步骤都完成，或者没有任何步骤完成。
- **Consistent** – 这是确保我们的数据保持一致状态的概念。
