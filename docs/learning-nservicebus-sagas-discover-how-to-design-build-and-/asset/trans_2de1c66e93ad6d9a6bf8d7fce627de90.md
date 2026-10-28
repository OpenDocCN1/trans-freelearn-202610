
```
public class PaymentRequestSaga : NServiceBus.Saga.Saga<PaymentRequestData>,
    IAmStartedByMessages<SendCommand>,
    IHandleMessages<ResponseCommand>,
    IHandleTimeouts<SendCommand>
{
    public void Handle(SendCommand message)
    {
        ResponseCommand resp = new ResponseCommand();
        resp.RequestId = message.RequestId;
        resp.state = PaymentMessages.MessageParts.StateCodes.SentMyWCFClient;
        ReplyToOriginator(resp);
        Bus.Send(message);
        RequestTimeout(TimeSpan.FromHours(3), message); // 3 hours
        Data.RequestId = message.RequestId;
    }
}
```

- `Saga<IContainSagaData>`：saga 数据是被持久化的数据。这些数据包含 NSB 为 ID、发起方和 OriginalMessageId 实例设置的默认值。此外，应使用唯一 ID 在消息和数据之间进行映射，反之亦然。如下图所示：

```
public class PaymentRequestData : IContainSagaData
{
    /***
     * Gets/sets the Id of the process. Do NOT generate this value in your code.
     The value of the Id will be generated automatically to provide the
     best performance for saving in a database.
     ***/
    public virtual Guid Id { get; set; } // Required
    /***
     * Contains the return address of the endpoint that caused the process to run.
     ***/
    public virtual string Originator { get; set; } //Required
    /***
     * Contains the Id of the message which caused the saga to start.
     This is needed so that when we reply to the Originator, any
     registered callbacks will be fired correctly.
     ***/
    public virtual string OriginalMessageId { get; set; } //Required

    [Unique]
    public virtual Guid RequestId { get; set; } // Unique ID to lookup Request message
}
```

- `IHandleMessages<IMessage>`：除非正在处理消息，否则 saga 不会做太多事情。它可以检索通过配置映射的消息的已保存数据。NSB 通过唯一 ID（此处为 `RequestId`，定义在 `ConfigureHowToFindSaga()` 中）来处理数据到消息的映射。
- `ConfigureHowToFindSaga()`：saga 模式的这个函数用于将数据映射到消息。通常，映射可以通过 GUID 或 ID 执行，但它必须是唯一的数据类型，可以作为数据库中的主键存储。被映射的消息包含相同的键，当它传入 saga 时，会找到与该键匹配的数据。当 saga 对象被实例化时，会调用此函数。

```
public override void ConfigureHowToFindSaga()
{
    ConfigureMapping<ResponseCommand>(x => x.RequestId).ToSaga(x => x.RequestId);
}
```

## Saga 有什么用处？

Saga 在工作流管理中扮演着关键角色。如果无法将数据映射到持久化数据，消息之间的工作流就无法实现。Saga 的目的是持久化 saga 数据对象，这些对象由消息处理程序中的消息指示，从所选内容中填充数据。以下是 saga 的一些用途：

- **保存消息状态/会话信息**：当消息启动时，消息处理程序将创建 saga 数据实例，该实例将在消息处理程序结束时保存。由消息处理程序数据负责将数据设置到对象的实例中。NSB 处理映射。以下截图显示了处理消息的代码：

```
public void Handle(SendCommand message)
{
    ResponseCommand resp = new ResponseCommand();
    resp.RequestId = message.RequestId;
    resp.state = PaymentMessages.MessageParts.StateCodes.SentMyWCFClient;
    ReplyToOriginator(resp);
    Bus.Send(message);
    RequestTimeout(TimeSpan.FromHours(3), message); // 3 hours
    Data.RequestId = message.RequestId;
}
```

在此场景中，我们给消息三个小时的时间来执行其操作，然后使其超时。以下截图显示了提供超时的代码：

```
public class PaymentRequestSaga : NServiceBus.Saga.Saga<PaymentRequestData>,
    IAmStartedByMessages<SendCommand>,
    IHandleMessages<ResponseCommand>,
    IHandleTimeouts<SendCommand>
{
    public override void ConfigureHowToFindSaga()...

    public void Handle(SendCommand message)
    {
        // ...
        RequestTimeout(TimeSpan.FromHours(3), message); // 3 hours
        // ...
    }

    public void Handle(ResponseCommand message)...

    public void Timeout(SendCommand state)
    {
        ResponseCommand resp = new ResponseCommand();
        resp.RequestId = state.RequestId;
        resp.state = PaymentMessages.MessageParts.StateCodes.Timeout;
        ReplyToOriginator(resp);
        MarkAsComplete();
    }
}
```

由于 saga 模式能够持久化消息的状态数据，消息变成了各自独立、无连接的片段，并提供了一种将多个消息相互连接以执行端到端流程的方法。因此，saga 成为交互的焦点。消息现在具有行为模式，因为它们比单一实例更有意义。它们现在具有很长的生存时间，因为即使单个消息不再可用，代表该消息数据的 saga 数据仍然会继续存在，直到被标记为销毁。

这些示例表明，在 5.0 版本中，总线配置是作为一个对象创建的，配置值被添加到该对象的实例中。总线配置可以作为配置传递给函数，以便反复利用，实现总线配置的重用。

较新版本中 NServiceBus 扩展的其他功能还包括新增的 NSB 包。其中一个包是 `NServiceBus.NLog`，它专门用于在 NSB 5.0 及以上版本中使用 NLog。它用于扩展 NSB 的日志记录能力，以记录适用于端点、消息和服务的更多信息。有关 `NServiceBus.NLog` 包的详细使用，请参见 `http://docs.particular.net/nservicebus/logging-in-nservicebus`。我们将在本章后面演示该代码的用法。

## Saga 工作流

Saga 的目的是提供工作流。它提供 saga 数据的持久化，以便消息可以返回 saga，并有一个指向原始消息的参考点。

本节中的源代码位于 *CreditCardApproval – v5* 部分。它使用 NSB 5.0 版本在 Visual Studio 2012 中编译。项目将如下所示：

![](img/8188651cea6a881a16242b395a81399a_53_0.png)

这里我们有以下项目：

- `AppCommon`：这包含 Windows 窗体的 ViewModel 和上下文。
- `AppForAccountingDept`：这接收与信用卡审批相关的采购订单号。
- `CreditCardApproval`：这审批信用卡。
- `CreditCardSubmit`：这提交信用卡以供审批。

## NServiceBus Saga 架构

- `CreditCardMessages`：这包含项目的公共消息。
- `CreditCardSaga`：这是信用卡 saga，通过使用消息处理和 saga 数据来指导工作流和消息路由。

消息流（它完成了所有工作，即工作流）将从 `CreditCardSubmit` 项目开始。此窗体将向 saga 提交信用卡，saga 将保存其状态并将其转发给 `CreditCardApproval` 项目。`CreditCardApproval` 窗体将批准或拒绝该卡，并将响应发送回 saga。saga 将检索先前的消息并将其传递回 `CreditCardSubmit` 窗体，如果获得批准，则会将其发送到 `AppForAccountingDept` 项目。消息流如下所示：

![](img/8188651cea6a881a16242b395a81399a_54_0.png)

这是一个非常简单的工作流，因为它跟踪信用卡是否获批。通过使 saga 数据持久化，它允许持久化的 saga 数据状态在单一点路由消息并管理消息信息。saga 数据将包含必需的 ID、发起方和 `OriginalMessageId`，以确保能够响应原始客户端。这些字段确保返回始终可用。我们在下图中看到这些字段：
