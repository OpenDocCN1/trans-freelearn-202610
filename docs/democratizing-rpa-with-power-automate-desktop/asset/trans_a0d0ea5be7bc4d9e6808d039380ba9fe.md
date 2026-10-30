

通常，工作站是 AD 的一部分，这意味着它们已加入该域，并且该域的用户可以登录。运行 PAD 的交互用户也可能是这样的用户。在这种情况下创建与 AD 的连接不应成为问题。如前所述，可以通过域控制器的 IP 地址和适当的用户凭据创建与 AD 的连接。在这种情况下，运行该 UI 流程的计算机不需要是该域的成员，并且可以通过 IP 地址建立连接。但是，要使用操作，例如创建用户，需要对域有一定的了解，以便提供可分辨名称的信息。

在 *第 8 章* 中，我们讨论了新入职员工进入公司因此需要 SAP 登录的场景。我们创建了一个 UI 流程，读取 Excel 列表并为每个条目创建一个 SAP 用户。作为附加任务，我们也可以以同样的方式在 AD 中创建用户。我鼓励你采用此示例并将此附加任务添加为子流程。如果没有 SAP 系统可用，也可以采用 *第 8 章* 中的 UI 流程并将 SAP 部分替换为 AD 部分。

尽管仍然存在许多本地 Windows 网络，但 PAD 包含对本地 AD 的管理操作这一事实表明了该软件的来源以及它在市场上存在的时间。除 AD 外，没有提供其他可以管理本地 Windows 网络服务的动作。尽管如此，使用以下服务也为 PAD 整体带来了巨大的价值。

## 与数据库协作

**Database** 操作组仅包含三个操作（**Open SQL connection**、**Execute SQL statement** 和 **Close SQL connection**），但这些操作打开了全方位的可能性，因为可以访问所有数据库以发出任何类型的 SQL 命令。迄今为止，重点一直放在没有任何 API 的系统上。然而，PAD 也能够集成具有 API 的系统，以便在 UI 流程中也可以完成需要调用 API 的任务。数据宇宙的关键在于将 **Open SQL connection** 操作拖到设计器上，连接字符串参数的右侧（见以下屏幕截图）。

### Open SQL connection

Open a new connection to a database More info

Select parameters

Connection string:

Variables produced SQLConnection2

On error

Save

Cancel

Figure 11.3 – Open SQL connection

要使用数据库，我们需要提供连接字符串，单击上一张屏幕截图中显示的红色边框图标将打开一个向导来完成此操作。

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_317_0.png)

Figure 11.4 – Create a connection string

在我的示例中，我使用的是远程服务器上的 SQL Server 数据库，但如前面的屏幕截图所示，**Provider** 选项卡包含许多条目，可用于连接其他系统。在这里按下 **Next** 按钮会带我们进入 **Connection** 选项卡，其中需要根据上一屏幕选择的提供程序进行额外的输入。我输入了远程服务器名称和提供的凭据，这使我能够在该服务器上选择一个数据库。如果我现在按下 **OK** 按钮，连接字符串将为我生成并输入到相应的文本字段中，我也可以复制该条目并将其输入到敏感变量中。

通过这项简短的准备工作，我现在可以针对数据库执行任何 SQL 语句。请注意，如果相应用户没有必要的权限，语句可能无法成功执行。以下屏幕截图显示了 **Execute SQL statement** 操作。查询结果将存储在变量中以供进一步处理。

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_318_0.png)

Figure 11.5 – Execute SQL statement

关于 SQL 语句可以做出以下评论：

- 非常频繁地，语句的质量对数据库的性能以及整个系统的性能至关重要。因此，如果可能，应避免使用诸如 `SELECT * FROM ...` 之类的查询。
- 当然，现在也可以将存储在变量中的先前流程步骤中的其他内容插入到语句中。

在下一个主要部分中，我们将看一个数据库如何集成到更大上下文中的示例。在此之前，让我们先看看此类别中的其他服务。

## 使用本地 Exchange 和其他邮件服务

我们已在 *第 2 章* 和 *第 8 章* 中学习了如何使用本地安装的 Outlook 应用程序以及相应的 PAD 操作来接收、处理和发送电子邮件。除了这种可能性之外，PAD 还提供了其他操作组来处理电子邮件。这些操作组的行为与我们之前学到的非常相似，只是连接设置不同。这些操作组是 **Exchange Server** 和 **Email**。为了提供概述并了解这些操作组之间的差异，包括 **Outlook** 的操作组，让我们看一下下面的表格。

| Feature/Action group | Outlook | Exchange Server | Email |
|---|---|---|---|
| Connection through | Local Outlook client | Local/on-premises Exchange Server deployment | Any **Internet Message Access Protocol (IMAP)** mailbox |
| Feature/Action group | Outlook | Exchange Server | Email |
| Connection capability | Any mailbox that is connected through Outlook, including Outlook 365 | One personal or shared mailbox | Any IMAP server |
| Retrieve email messages | Yes | Yes | Yes |
| Send email messages | Yes | Yes | Yes |
| Process email messages | Yes | Yes | Yes |
| Save messages | Yes | No | No |
| Respond to messages | Yes | No | No |

Table 11.1 – A comparison between different email options in PAD

正如我们在此看到的，不同操作组之间有很多相似之处，也有一个主要区别。虽然 **Exchange Server** 和 **Email** 操作组通过网络请求执行操作，但 Outlook 操作针对的是本地安装的 Outlook 客户端的功能。这导致了以下后果：

- 使用 Outlook 时，会启动本地程序；例如，该程序也必须可用、已安装并已设置。
- 优点是操作可以用 Outlook 设置的所有邮箱使用，包括 Outlook 365、本地 Exchange Server 或任何其他邮箱。
- 出于同样的原因，使用 Outlook 操作也提供了更高级的选项，例如 **Save messages** 或 **Reply to a message**。
- 如果运行 UI 流程的计算机上未安装 Outlook，则必须使用其他操作组之一。
- 然而，这也有利于节省本地计算机资源，从而为其他任务提供资源。

对于电子邮件来说，最轻量的选项是使用 **Email** 操作组中的 **Retrieve email messages**、**Process email messages** 和 **Send email** 操作。连接到邮件服务器的创建必须在三个操作中分别定义，因此没有单独用于建立或关闭连接的操作。然而，这只需要一个操作，例如接收电子邮件。需要邮件服务器的地址，以及有效用户名和密码来使用操作。

如果你想使用访问本地 Exchange Server 的操作，请注意 Exchange Server 直到版本 **2013 Service Pack 1** 才受支持。

除了这里介绍的差异外，用于处理电子邮件的操作都以相同的方式工作。唯一需要决定的就是是否可以使用 Outlook 客户端以及是否应该使用。这一决定必须单独做出，我希望你能自行尝试进一步的电子邮件操作。

我们将在下一节学习 PAD 还可以使用哪些其他基于 Web 的协议和格式。

## 与 Web 协议和格式协作

在上一节中，我们讨论了 LDAP 和 IMAP 等各种 Web 协议，并看到了 PAD 如何支持它们。接下来，我们将继续
