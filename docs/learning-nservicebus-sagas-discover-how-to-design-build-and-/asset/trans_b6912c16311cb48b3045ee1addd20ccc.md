

正如我们所见，NServiceBus 测试 API 使用的是 Fluent API 规范风格的测试模式，而非大多数人通常会使用的、属于 nunit 或其他 xunit 类型框架的传统断言风格。

我们现在将开始测试来自 UnitTestHandlers 项目和 UnitTestSaga2.cs 文件的 saga 代码。

需要注意的一点是，如果 saga 实体对象在不同的函数调用中通过 MarkAsComplete() 方法被删除，这些应该分别进行测试。这是因为一旦我们删除了对象，就无法再次删除它。例如，在我们的测试中，我们将使用以下内容：

```
[TestMethod]
public void Run()
{
    Test.Initialize();

    /**
     * State sent to Saga
     * ***
    */
    SendCommand command = new SendCommand();
    command.RequestId = new Guid("8b265223-dc9e-4789-a6df-69d19f644ad7");
    command.state = MyMessages.MessageParts.StateCodes.SentMyWCFClient;

    /**
     * Response from WCF and to MVCApp
     * ***
    */
    ResponseCommand resp = new ResponseCommand();
    resp.RequestId = new Guid("8b265223-dc9e-4789-a6df-69d19f644ad7");
    resp.state = MyMessages.MessageParts.StateCodes.CompleteMyWCFClient;

    Test.Saga<MyTestSaga>()
        .ExpectReplyToOriginator<ResponseCommand>()
        .ExpectSend<SendCommand>()
        .When(s => s.Handle(command))
        .ExpectReplyToOriginator<ResponseCommand>()
        .When(s => s.Handle(resp))
        .AssertSagaCompletionIs(true);

    Test.Saga<MyTestSaga>()
        .ExpectReplyToOriginator<ResponseCommand>()
        .ExpectSend<SendCommand>()
        .ExpectTimeoutToBeSetIn<SendCommand>((state, span) => span == TimeSpan.FromHours(3))
        .When(s => s.Handle(command))
        .ExpectReplyToOriginator<ResponseCommand>()
        .WhenSagaTimesOut()
        .AssertSagaCompletionIs(true);
}
```

现在数据库部分已经创建，我们需要在 MSMQ 中创建 mysaga 端点。这只需按如下方式执行 MySaga 解决方案即可：

![](img/8188651cea6a881a16242b395a81399a_143_0.png)

![](img/8188651cea6a881a16242b395a81399a_143_1.png)

我们可以在 MSMQ Commander 窗口中看到端点已创建，如下所示：

![](img/8188651cea6a881a16242b395a81399a_144_0.png)

如果测试运行成功，则 mysaga 端点存在并且正在工作。我们可以看到测试程序的工作情况如下：

![](img/8188651cea6a881a16242b395a81399a_144_1.png)

数据库表，&lt;PayMessage&gt;，具有以下属性，如下所示：

|  | Column Name | Data Type | Allow Nulls |
|---|---|---|---|
| 🔑 | id | int | ☐ |
|  | EventId | uniqueidentifier | ☐ |
|  | error | varchar(50) | ☑ |
|  | state | varchar(50) | ☑ |

## RabbitMQ for NSB

RabbitMQ 是一个跨平台的消息框架，像 MSMQ 一样，可以在 Linux 和 Windows 操作系统上运行。

RabbitMQ 比 MSMQ 具有更多功能，例如路由、虚拟主机和强大的管理工具集。它可以在 Linux 上运行并且扩展性非常好。对于大型企业系统（尤其是在异构网络环境中），你应该认真考虑 RabbitMQ。RabbitMQ 可以在 NSB 配置中轻松替换 MSMQ 队列系统。

对于 RabbitMQ 的管理，可以轻松安装 Web 管理界面。然而，如果需要命令行界面，则需要安装 Python 语言。

为了重温前面章节中的一些参考资料，请查看以下内容：

- **Local host management site**: 这可以在 http://localhost:15672/#/. 找到。
- **Documentation**: 这可以在 http://www.rabbitmq.com/documentation.html. 找到。
- **Windows installation**: 这可以在 https://www.rabbitmq.com/install-windows.html. 找到。
- **NServiceBus samples**: 这些可以在 https://github.com/Particular/NServiceBus.RabbitMQ.Samples. 找到。
- **Development tools site for RabbitMQ**: 这可以在 http://www.rabbitmq.com/devtools.html. 找到。
- **NServiceBus.RabbitMQ source code**: 这可以在 https://github.com/Particular/NServiceBus.RabbitMQ. 找到。
- **NServiceBus RabbitMQ hands-on lab**: 这可以在 http://particular.net/HandsOnLabs. 找到。

## Saga Development

安装 RabbitMQ 后，我们可以使用 rabbitmq plugins 设置管理插件以启用 rabbitmq_management。这是为了使用 Web 管理 http://localhost:15672/#/queues 查看队列，如以下屏幕截图所示：

![](img/8188651cea6a881a16242b395a81399a_148_0.png)

我们可以通过在 RabbitMQ 命令提示符中输入 rabbitmqctl list_queues 来获取队列列表，如下所示：

![](img/8188651cea6a881a16242b395a81399a_148_1.png)

## ActiveMQ in NSB

Apache Active Message Queue（ActiveMQ）是 Apache 基金会基于 Java 消息服务（JMS）的 JAVA 开源框架。访问 http://en.wikipedia.org/wiki/Apache_ActiveMQ 和 https://activemq.apache.org 获取更多信息。它可以在机器上运行，无论是 Windows 还是 Linux，都在 Java 运行时环境（JRE）中。机器上必须运行 JAVA，并且 JAVA_HOME 环境路径必须配置为指向 JRE 的根文件夹。ActiveMQ 的安装说明可以在 https://activemq.apache.org/getting-started#GettingStarted20-Download. 找到。

### The source code

在本节中，我们将使用 ActiveMQ 解决方案。此解决方案与 RabbitMQ 类似，只是使用 ActiveMQ 而不是 RabbitMQ。

将有三个基本步骤：

- 1. 添加 NServiceBus.ActiveMQ 引用。
- 2. 将 NServiceBus 传输机制从 `<MSMQ>` 改为 `<ActiveMQ>`。
- 3. 在 App.config 文件中设置 ActiveMQ 传输配置。

在 Windows 操作系统上下载后，我们将 Windows 二进制文件解压到 c:\activemq\ 目录。从命令提示符在 c:\activemq\bin\ 下运行 activemq.bat 批处理文件，将显示一系列命令以表明 ActiveMQ 正在运行。这在以下屏幕截图中显示：

![](img/8188651cea6a881a16242b395a81399a_151_0.png)

另一种选择是将 ActiveMQ 安装为 Windows 服务，其中存在适用于 Win32 和 Win64 机器的安装脚本。对于 64 位 Windows Server，我们可以使用 c:\activemq\bin\win64\ 中的 InstallService.bat。

确保 RabbitMQ 未在后台作为 Windows 服务运行，因为它们使用相同的网络端口。此外，Microsoft ServiceBus for Windows Servers 也会共享相同的 AMQP 端口，即端口 5672。ActiveMQ 的默认端口是 61616。可以使用 Microsoft 的 TcpView 检查端口，可以在 http://technet.microsoft.com/en-us/sysinternals/bb897437. 找到。

为了确保 ActiveMQ 正在运行，你可以通过浏览器使用 http://localhost:8161/admin 访问管理控制台。默认用户 ID 和密码均为 admin。请访问 http://activemq.apache.org/getting-started.html 查看文档。访问管理控制台时，你应该会看到如下屏幕截图：

![](img/8188651cea6a881a16242b395a81399a_152_0.png)

要在 Visual Studio 项目中使用 ActiveMQ for NServiceBus，必须安装 NServiceBus.ActiveMQ 的 NuGet 版本。前往 https://www.nuget.org/packages/NServiceBus.ActiveMQ/1.0.5 查看如何将 PM> Install-Package NServiceBus.ActiveMQ 安装到项目中。确保 IBus 的配置设置为 ActiveMQ，如下所示：

```
protected void Application_Start()
{
    Configure.ScaleOut(s => s.UseSingleBrokerQueue());
    _startableBus = Configure.With()
        .DefaultBuilder()
        .RijndaelEncryptionService()
        .UseTransport<ActiveMQ>()
        .UnicastBus()
        .CreateBus();
    Configure.Instance.ForInstallationOn<Windows>().Install();

    _bus = _startableBus.Start();
    AreaRegistration.RegisterAllAreas();

    RegisterGlobalFilters(GlobalFilters.Filters);
    RegisterRoutes(RouteTable.Routes);
}
```

确保 App.Config 或 Web.Config 文件具有适当的连接字符串，以便 NServiceBus/Transport 字符串指向正确的 ActiveMQ 队列实例：

```
<connectionStrings>
<add name="NServiceBus/Transport" connectionString="ServerUrl=activemq:tcp://localhost:61616"/>
</connectionStrings>
```

