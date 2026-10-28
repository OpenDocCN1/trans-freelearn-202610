

## 探索 Azure 消息队列

Microsoft Azure Service Bus 是一个完全托管的企业集成消息代理。Service Bus 中包含了一些常见的消息场景，例如在传输数据时进行消息传递或解耦应用程序以提高其可靠性和可扩展性，以及通过实现需要消息排序的工作流来提供消息会话，例如主题和订阅。

Azure 支持以下两种类型的队列机制：

- Queue Storage 是一个用于存储大量消息的简单消息队列服务
- Service Bus 队列是更广泛的消息服务集的一部分，支持排队、发布/订阅和高级集成模式

Azure Queue Storage 是一个用于发送和接收消息的服务，但也用于存储大量消息；一个队列消息的大小可达 64 KB。

## 练习 2 – 创建 Azure Service Bus 命名空间和队列

在本练习中，我们将执行以下操作：

- 创建一个 Azure Service Bus 命名空间
- 创建一个 Azure Service Bus 队列

队列可以通过 Azure 门户的交互式界面、使用 PowerShell 或 Azure CLI 的命令行，或使用资源管理器模板来创建。我们在这里只介绍前两种方法：Azure 门户和 Azure CLI。

### 使用 Azure 门户

让我们打开 Azure 门户并选择 **All services**。在 **Integration** 类别中，我们可以找到 **Service Bus** 资源：

![](img/66aceec30d3e7dead2f71bb68a887688_77_0.png)

图 3.16 – 使用 Azure 门户创建 Service Bus

在填写完创建命名空间所需的配置后，该命名空间将包含队列之后，我们点击 **+ Create** 或选择 **Create service bus namespace**：

![](img/66aceec30d3e7dead2f71bb68a887688_77_1.png)

图 3.17 – 使用 Azure 门户创建命名空间

在 **Basics** 选项卡中，我们有以下界面：

![](img/66aceec30d3e7dead2f71bb68a887688_78_0.png)

图 3.18 – 创建 Service Bus 命名空间

如果在 **Pricing tier** 字段中选择 **Basic**，请注意您将无法创建 **Topics**，只能创建 **Queues**，如下图所示：

![](img/66aceec30d3e7dead2f71bb68a887688_79_0.png)

在 **Networking** 选项卡中，您可以保留公共访问的连接方法的默认值，或者如果您已配置登陆区，则可以选择您的私有访问。在登陆区中，您配置您的账户、安全治理、网络和身份。

我们将选择 **Review + create** 然后选择 **Create**，Service Bus 命名空间将被创建。

完成后，我们将在门户中创建一个队列。让我们打开 Service Bus 命名空间并从左侧菜单中选择 **Overview**。接下来，在屏幕顶部选择 **+ Queue** 并填写创建队列所需的所有信息，包括名称、队列最大大小、最大投递次数、消息生存时间（天、小时、分钟和秒）、锁定持续时间（天、小时、分钟和秒）。此外，我们需要启用以下选项：

- Auto-delete on idle queue
- Duplicate detection
- Dead lettering on message expiration
- Partitioning
- Sessions

我们可以启用将消息转发到队列和主题：

![](img/66aceec30d3e7dead2f71bb68a887688_80_0.png)

图 3.20 – 创建队列

我们使用 Azure 门户创建了 Azure Service Bus 命名空间和队列。在下一节中，我们将了解如何使用 Azure CLI 来完成此操作。

### 使用 Azure CLI

首先，如果尚未存在，我们需要创建一个资源组：

```
Az group create --name packrg --location eastus
```

消息队列使用命名空间，因此我们需要创建一个命名空间：

```
Az servicebus namespace create --resource-group packrg --name appointmentQueue --location eastus
```

接下来，使用以下命令创建队列：

```
az servicebus queue create --resource-group packrg --namespace-name appointmentQueue --name myqueue
```

要检查消息队列是否已成功创建，您可以在门户中检查或运行以下命令行：

```
az servicebus queue list -resource-group packrg -namespace-name appointmentQueue
```

在下面的屏幕截图中，我们可以看到不同的 Service Bus 队列：

![](img/66aceec30d3e7dead2f71bb68a887688_81_0.png)

图 3.21 – Service Bus 队列

我们演示了两种不同的方法来创建 Azure Service Bus 命名空间和队列 – Azure CLI 和 Azure 门户。在下一个练习中，我们将使用应用程序将消息发布到 Service Bus 队列。

## 练习 3 – 使用 .NET Core 应用程序将消息发布到 Service Bus 队列

我们之前创建了 Azure Service Bus 队列。现在，我们将开始使用 .NET Core 控制台应用程序从 Azure Service Bus 队列发送消息。

我们将创建一个包含两个项目的解决方案：

- AzureQueueSample.Domains：一个类库
- AzureQueueSample.Sender：一个 .NET Core 控制台应用程序

我们将 AzureQueueSample.Domains 项目添加为 AzureQueueSample.Sender 项目的项目引用。为此，右键单击 AzureQueueSample.Sender 项目的 Dependencies，然后选择 Add Project Reference：

![](img/66aceec30d3e7dead2f71bb68a887688_82_0.png)

图 3.22 – 向项目添加 Dependencies 引用

我们选择 AzureQueueSample.Domains 项目，项目将被添加为引用：

![](img/66aceec30d3e7dead2f71bb68a887688_82_1.png)

图 3.23 – AzureQueueSample 解决方案

在 AzureQueueSample.Domains 项目中，我们将添加一个名为 Appointment 的新类：

```
public class Appointment
{
    public Guid AppointmentId { get; set; }
}
```

```
public string Status { get; set; }
public string SlotId { get; set; }
public string PatientEmail { get; set; }
public string DoctorId { get; set; }
public string CurrentDate { get; set; }
public string StartTime { get; set; }
public string EndTime { get; set; }
}
```

在下面的屏幕截图中，我们使用 NuGet Package Manager 向 AzureQueueSample.Sender 添加了两个不同的包，即 Azure.Messaging.ServiceBus 和 Newtonsoft.Json。

![](img/66aceec30d3e7dead2f71bb68a887688_83_0.png)

图 3.24 – 从 NuGet 添加 Microsoft.Azure.ServiceBus 包

接下来，我们打开 AzureQueueSample.Sender 的 program.cs 类并添加预约列表。我们还添加了目标 Azure Service Bus 命名空间的连接字符串和队列名称。

在 Azure 门户中，返回 Service Bus 命名空间并选择 Settings | Shared access policies，然后复制 Primary Connection String 值：

![](img/66aceec30d3e7dead2f71bb68a887688_84_0.png)

图 3.25 – Azure Service Bus 命名空间中的共享访问策略

这是允许我们将消息发布到 Service Bus 队列的 Program 类：

```
private const string ServiceBusConnectionString = "Endpoint=sb://appointmentqueue.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=1CqAJhjCv5M284qwRuLtApely83Ju9G48RxGuED2Zs0=";
    private const string QueueName = "myqueue";
    static List<Appointment> appointments = new List<Appointment>
    {
        new Appointment ()
        {
            AppointmentId = Guid.NewGuid(),
            CurrentDate = DateTime.Now.ToString(),
            StartTime = "10:00AM",
            EndTime ="11:00AM",
            DoctorId = "code1",
            SlotId="S1",
            PatientEmail= "patient1@email.com",
            Status = "Pending"
        },
```

```
var messageBody = JsonConvert.
SerializeObject(item);
    ServiceBusMessage message = new
ServiceBusMessage(Encoding.UTF8.GetBytes(messageBody));
    await sender.SendMessageAsync(message);
    Console.WriteLine($"Sending Message : {item.
AppointmentId.ToString()} ");
    }
    Console.Read();
}
```

结果如下：

![](img/66aceec30d3e7dead2f71bb68a887688_86_0.png)

图 3.26 – 向 Azure Service Bus 队列发送消息

在本练习中，我们使用 .NET Core 应用程序将消息发布到 Service Bus 队列。在下一节中，我们将使用同一应用程序从 Service Bus 队列读取消息。

## 练习 4 – 使用 .NET Core 应用程序从 Service Bus 队列读取消息

之前，我们向 Service Bus 队列发送了一些消息。在本练习中，我们将读取它们。为此，我们将添加一个新的 AzureQueueSample.Receiver .NET Core 控制台应用程序。我们需要为接收器应用程序添加与发送器应用程序之前相同的 NuGet 包：Azure.Messaging.ServiceBus 和 Newtonsoft.Json。我们还将添加 AzureQueueSample.Domains 作为引用。

