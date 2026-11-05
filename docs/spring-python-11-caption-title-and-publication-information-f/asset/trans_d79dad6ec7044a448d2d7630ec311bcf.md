
```
class Bank(object):
    def __init__(self, connectionFactory):
        self.factory = connectionFactory
        self.dt = DatabaseTemplate(self.factory)

    def transfer(self, transfer_amt, source_act, target_act):
        self.dt.execute("""
            update ACCOUNT
            set BALANCE = BALANCE - %s
            where ACCOUNT_NUM = %s""",
            (transfer_amt, source_act))
        self.dt.execute("""
            update ACCOUNT
            set BALANCE = BALANCE + %s
            where ACCOUNT_NUM = %s""",
            (transfer_amt, target_act))
```

在这种情况下，我们已剔除所有手工编写的事务代码。取而代之的是定义一个 `transfer` 操作的简单、简洁的业务逻辑。

请注意，此版本的 Bank 应用尚未 *NOT* 安全。

步骤可通过以下时序图轻松展示：

![](img/bcf886d8b665372ee8afbb620d4273bb_115_0.png)

2. 现在让我们使用 Spring Python 的 @transactional 装饰器为 transfer 方法添加事务保护。

```python
from springpython.database.core import *
from springpython.database.factory import *
from springpython.database.transaction import *

class Bank(object):
    def __init__(self, connectionFactory):
        self.factory = connectionFactory
        self.dt = DatabaseTemplate(self.factory)

    @transactional
    def transfer(self, transfer_amt, source_act, target_act):
        self.dt.execute("""
            update ACCOUNT
            set BALANCE = BALANCE - %s
            where ACCOUNT_NUM = %s""",
            (transfer_amt, source_act))
        self.dt.execute("""
            update ACCOUNT
            set BALANCE = BALANCE + %s
            where ACCOUNT_NUM = %s""",
            (transfer_amt, target_act))
```

@transactional 是一个装饰器，它使用一个隐藏的 TransactionTemplate 实例在非常健壮的事务模式版本中执行我们的 transfer 函数。我们的交互在以下图表中展示得更好。

![](img/bcf886d8b665372ee8afbb620d4273bb_116_0.png)

- 因为 `@transactional` 包裹了我们的 `transfer` 函数，当用户调用 `transfer` 时，他们首先命中的是装饰器
- `@transactional` 将请求的方法调用的所有上下文传递给其私有的 `TransactionTemplate` 实例
- `TransactionTemplate` 开始一个事务
- `TransactionTemplate` 然后调用原始的 `Bank transfer` 函数
- `Bank` 执行其业务，完全不知道它在事务内部
- 完成后，`Bank` 将控制权交还给 `TransactionTemplate`，后者发出一个 `commit`
- `TransactionTemplate` 将控制权交还给 `@transactional`，最终交还给调用者

> 此配置完全将事务逻辑与我们的 transfer 操作解耦。

- 由于 `TransactionTemplate` 是 `transfer` 的调用者，它可以轻松处理任意数量的 return 语句。如果在 `transfer` 内部任何位置引发异常，`TransactionTemplate` 将执行 `rollback` 而不是 `commit`。

有了这种清晰的关注点分离，我们可以在业务代码上工作，而无需担心事务的正确性。

## 关于 TransactionTemplate 的更多内容

我们第一个事务模式版本简单且天真。随后我们检查了该模式的问题列表。`TransactionTemplate` 具有更复杂的模式，可处理这些额外情况：

- 它通过捕获任何抛出的异常来处理简单情况，发出回滚，然后重新抛出异常
- 它通过捕获方法的返回值来处理所有 return 语句，发出必要的提交，然后最终返回结果
- `@transactional` 默认编码为：如果当前没有进行中的事务则启动新事务，如果已存在事务则加入该事务。我们将在本章后面查看其他事务选项

在我们的 `transfer` 代码中还存在另一个完整性缺口。它位于事务模式和业务逻辑之间。你能发现它吗？

没有检查来确保我们甚至有 $10,000 可转账！此外，也没有安全检查来确保我们拥有这两个账户中的任一个。我们将在填补事务细节时稍后解决这一缺陷。

我们还没完成。为了让 @transactional 发挥作用，我们需要通过一个 AutoTransactionalObject 将其与 Transaction Manager 链接。Transaction Manager 为 @transactional 提供进入数据库的句柄以发出必要的提交和回滚。它还跟踪现有事务的上下文并在何时启动新事务时做出适当决策。

1. 为了注入所有内容，让我们定义一个纯 Python IoC 容器，将 ConnectionFactoryTransactionManager 通过 AutoTransactionalObject 链接到 @transaction。

```python
from springpython.database.transaction import *
class BankAppConfig(PythonConfig):
    def __init__(self, factory):
        PythonConfig.__init__(self)
        self.factory = factory

    @Object
    def transactionalObject(self):
        return AutoTransactionalObject(self.tx_mgr())

    @Object
    def tx_mgr(self):
        return ConnectionFactoryTransactionManager(self.factory)

    @Object
    def bank(self):
        return Bank(self.factory)
```

- tx_mgr 定义了我们的 Transaction Manager，它使用注入的工厂来执行 SQL 事务 API。这与 DatabaseTemplate 使用的工厂类型相同。tx_mgr 跟踪事务开始和结束的时间，为 TransactionTemplate 和 @transactional 提供必要的服务。
- transactionalObject 定义了 AutoTransactionalObject 的一个实例，这是一个 IoC 后处理器。它的工作是找到所有 @transactional 的实例并将其与 tx_mgr 链接。这赋予了 @transactional 通过 SQL 事务确保数据完整性的能力。
- bank 类是我们的业务类。

2. 现在，让我们编写一些启动代码来执行转账。

```python
if __name__ == "__main__":
    from springpython.context import ApplicationContext
    ctx = ApplicationContext(BankAppConfig(
        Sqlite3ConnectionFactory("/path/to/sqlite3db")))
    service = ctx.get_object("bank")
    service.transfer(10000.0, "SAVINGS", "CHECKING")
```

## Spring 三角形——可移植服务抽象

我们在书中前面看到过此图，作为 Spring Python 背后关键原则的说明。

![](img/bcf886d8b665372ee8afbb620d4273bb_119_0.png)

TransactionTemplate 代表一个可移植服务抽象。

- 它是可移植的，因为它使用 Python 的标准化 API 进行 SQL 事务，不将我们绑定到任何数据库厂商或自定义数据库连接库
- 它提供了有用的服务，让我们可以轻松地用复杂而强大的事务模式包装方法
- 它提供了一个编写事务代码的良好抽象，而无需直接处理 SQL 事务 API

## 编程式事务

我们的银行示例展示了如何使用 Spring Python 的 `@transactional` 装饰某些业务逻辑以使操作具有事务性。在整个示例中，我们反复提到了 `TransactionTemplate`。

在本节中，我们将直接使用 `TransactionTemplate` 而不是 `@transactional`。我们还将探讨如何有无 IoC 配置地执行此操作。

### 使用 IoC 容器配置

1. 首先，让我们重写 Bank，用 `TransactionTemplate` 替换 `@transactional`。

```python
class Bank(object):
    def __init__(self, connectionFactory):
        self.factory = connectionFactory
        self.dt = DatabaseTemplate(self.factory)
        self.tx_mgr = ConnectionFactoryTransactionManager(self.factory)
        self.tx_template = TransactionTemplate(self.tx_mgr)

    def transfer(self, transfer_amt, source_act, target_act):
        class TxDefinition(TransactionCallbackWithoutResult):
            def doInTransactionWithoutResult(self, status):
                self.dt.execute("""
                update ACCOUNT
                set BALANCE = BALANCE - %s
                where ACCOUNT_NUM = %s""",
                (transfer_amt, source_act))
                self.dt.execute("""
                update ACCOUNT
                set BALANCE = BALANCE + %s
                where ACCOUNT_NUM = %s""",
                (transfer_amt, target_act))
        self.tx_template.execute(TxDefinition())
```

此版本的 Bank 显示了另外两个属性：`tx_mgr` 和 `tx_template`。这些可以注入到我们的类中，但我们选择只注入连接工厂。
