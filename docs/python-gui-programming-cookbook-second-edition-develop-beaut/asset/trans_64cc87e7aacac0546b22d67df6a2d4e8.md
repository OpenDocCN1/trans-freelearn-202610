
```python
quote = self.quote.get(1.0, tk.END)
print(title)
print(quote)
self.mySQL.insertBooks(title, page, quote)
```

当我们现在运行代码时，我们可以将来自 Python GUI 的数据插入到 MySQL 数据库中：

GUI_MySQL.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_264_0.png)

在输入图书标题和图书页码以及书籍或电影中的引文后，我们通过点击 Insert Quote 按钮将数据插入到数据库中。

我们当前的方案允许标题、页码和引文。我们也可以插入我们最喜欢的电影引文。虽然电影没有页码，但我们可以使用 page 列来插入引文在电影中发生的近似时间。

接下来，我们可以通过发出之前使用过的相同命令来验证所有这些数据是否已进入我们的数据库表：

```text
GUI_MySQL.py
```

![](img/986b0dbf3f539f4f3baa82bd3003f04e_265_0.png)

插入数据后，我们可以通过点击 **Get Quotes** 按钮来验证数据是否已进入我们的两个 MySQL 表，该按钮随后会显示我们插入到两个 MySQL 数据库表中的数据，如前面的屏幕截图所示。

点击 **Get Quotes** 按钮会调用我们与按钮点击事件关联的回调方法。这会给我们提供在 `ScrolledText` 部件中显示的数据：

```python
# Adding a Button
self.action1 = ttk.Button(self.mySQL, text="Get Quotes",
                          command=self.getQuote)
self.action1.grid(column=2, row=2)

# Button callback
```

```python
def getQuote(self):
    allBooks = self.mySQL.showBooks()
    print(allBooks)
    self.quote.insert(tk.INSERT, allBooks)
```

我们使用 `self.mySQL` 类实例变量来调用 `showBooks()` 方法，该方法是我们导入的 MySQL 类的一部分：

```python
from Ch07_Code.GUI_MySQL_class import MySQL

class OOP():
    def __init__(self):
        # create MySQL instance
        self.mySQL = MySQL()

class MySQL():
    #------------------------------------------------------------
    def showBooks(self):
        # connect to MySQL
        conn, cursor = self.connect()

        self.useGuiDB(cursor)

        # print results
        cursor.execute("SELECT * FROM Books")
        allBooks = cursor.fetchall()
        print(allBooks)

        # close cursor and connection
        self.close(cursor, conn)

        return allBooks
```

## How it works...

在本教程中，我们导入了我们编写的 Python 模块，其中包含连接到 MySQL 数据库的所有编码逻辑，并知道如何插入、更新和删除数据。

我们现在已将 Python GUI 连接到此 SQL 逻辑。

## Using the MySQL Workbench

MySQL 有一个非常不错的 GUI，我们可以免费下载。它名为 MySQL Workbench。

在本教程中，我们将成功安装此工作台，然后使用它对我们在之前的教程 Configuring the MySQL database connection、Designing the Python GUI database 等中创建的 GuiDB 运行 SQL 查询。

## Getting ready

为了使用本教程，您需要我们在之前的教程中开发的 MySQL 数据库。您还需要一个正在运行的 MySQL 服务器。

## How to do it...

我们可以从官方 MySQL 网站下载 MySQL Workbench：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_267_0.png)

MySQL Workbench 本身就是一个 GUI，与我们在之前的教程中开发的非常相似。它还附带了一些专门用于与 MySQL 协作的附加功能。

## Storing Data in Our MySQL Database via Our GUI

当您安装 MySQL 时，如果您的 PC 上已安装了所需的组件，您可能已经安装了 MySQL Workbench。如果您尚未安装 Workbench，以下是安装 MySQL Workbench 的步骤：

1. 在 `http://www.mysql.com/products/workbench/` 网页上，您可以点击 **Download** 按钮，这会将您带到 `http://dev.mysql.com/downloads/workbench/` 网页：

   ![](img/986b0dbf3f539f4f3baa82bd3003f04e_268_0.png)

   下载与您的操作系统匹配的 MSI 安装程序，例如，对于典型的运行 64 位操作系统的 Windows 10，*mysql-workbench-community-6.3.8-winx64.msi*。

   安装可能会要求您使用 Oracle 账户登录，因此如果您还没有账户，您需要创建自己的 Oracle 开发者账户。不要担心，这是免费的。

   ![](img/986b0dbf3f539f4f3baa82bd3003f04e_268_1.png)

   > 几年前，MySQL 被 Oracle 收购。这就是为什么您需要 Oracle 账户来下载和安装 MySQL Server 和 MySQL Workbench 的原因。

2. 您将看到以下屏幕，及其他屏幕，直到您成功安装 MySQL Workbench：

   ![](img/986b0dbf3f539f4f3baa82bd3003f04e_269_0.png)

   安装程序窗口中的 6.3 CE 是 6.3 Community Edition 的缩写。

## Storing Data in Our MySQL Database via Our GUI

当您启动 MySQL Workbench 时，它会提示您连接。使用 root 用户和您为其创建的密码。MySQL Workbench 足够智能，可以识别您正在运行的 MySQL Server 及其监听的端口：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_270_0.png)

一旦您成功登录到 MySQL Server 实例，我们就可以选择我们的 `guidb`：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_271_0.png)

在 Workbench 左下角，我们可以在 **SCHEMAS** 标签下找到我们的 **guidb**。

在某些文献和产品中，数据库通常被称为 **SCHEMAS**。

我们可以将 SQL 命令输入到 Query Editor 中，并通过点击闪电图标执行我们的命令：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_272_0.png)

以下结果是 Query Editor 中的 Result Grid。我们可以点击不同的标签来查看不同的结果：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_272_1.png)

## How it works…

我们现在可以通过 *MySQL Workbench* GUI 连接到我们的 MySQL 数据库。我们可以执行之前发出的相同 SQL 命令，并在我们的 Python GUI 中执行时获得相同的结果。

## There's more…

凭借我们在本章和前几章的教程中获得的知识，我们现在完全有能力创建自己的 Python 编写的 GUI，这些 GUI 可以连接到 MySQL 数据库并与其通信。

# 8

## Internationalization and Testing

在本章中，我们将对 Python GUI 进行国际化和测试，涵盖以下教程：

- 在不同语言中显示部件文本
- 一次性更改整个 GUI 语言
- 本地化 GUI
- 为国际化准备 GUI
- 如何以敏捷方式设计 GUI
- 我们需要测试 GUI 代码吗？
- 设置调试监视
- 配置不同的调试输出级别
- 使用 Python 的 `__main__` 部分创建自测试代码
- 使用单元测试创建健壮的 GUI
- 如何使用 Eclipse PyDev IDE 编写单元测试

## Introduction

在本章中，我们将通过在标签、按钮、选项卡和其他部件上以不同语言显示文本来对 GUI 进行国际化。我们将从简单开始，然后探索如何在设计层面为 GUI 准备国际化。

## Internationalization and Testing

我们还将本地化 GUI，这与国际化略有不同。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_276_0.png)

由于这些词很长，它们已被缩写为使用该词的第一个字符，后跟第一个和最后一个字符之间的字符总数，最后是该词的最后一个字符。因此，internationalization 缩写为 I18N，localization 缩写为 L10N。

我们还将测试我们的 GUI 代码，编写单元测试，并探索单元测试在我们的开发工作中可以提供的价值，这将引领我们走向 *refactoring* 代码的最佳实践。

以下是本章的 Python 模块概述：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_276_1.png)

