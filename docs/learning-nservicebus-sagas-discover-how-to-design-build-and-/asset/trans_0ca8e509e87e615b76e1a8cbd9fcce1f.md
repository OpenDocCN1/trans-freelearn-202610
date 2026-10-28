

![](img/8188651cea6a881a16242b395a81399a_55_0.png)

Saga 数据是通过使用 IContainSagaData 接口来存储的。即使这三个字段没有显式定义，IContainSagaData 接口也会在存储中定义它们。不要修改这些字段，因为 NServiceBus 使用它们。主键是另一个在所有 saga 中用于从消息查找 saga 数据的字段。在此情况下，我们使用 RequestId，它将始终包含来自 [Unique] 注解的唯一值。我们可以在以下代码中看到这一点：

```
public class CreditCardRequestData : IContainSagaData
{
    /***
    * Gets/sets the Id of the process. Do NOT generate this value in your code.
    The value of the Id will be generated automatically to provide the
    best performance for saving in a database.
    * ***/
    public Guid Id { get; set; } // Required
    /***
    * Contains the return address of the endpoint that caused the process to run.
    * ***/
    public string Originator { get; set; } //Required
    /***
    * Contains the Id of the message which caused the saga to start.
    This is needed so that when we reply to the Originator, any
    registered callbacks will be fired correctly.
    * ***/
    public string OriginalMessageId { get; set; } //Required

    [Unique]
    public Guid RequestId { get; set; } // Unique ID to lookup Request message
    public string Card { get; set; }
    public decimal Cost { get; set; }
    public bool RequiresApproval { get; set; }
    public bool Approved { get; set; }
}
```

超时也可以用于为 saga 安排每日任务以完成其清理和维护工作。例如，我们可以每天向运营发送电子邮件，报告 saga 当天处理的邮件数量以及错误队列中是否有报告的邮件。在这里，我们给信用卡审批人 60 秒的时间进行审批。以下屏幕截图显示了此代码：

```
public void Handle(SubmitRequestCommand message)
{
    logger.Info("--------CreditCardSaga Handle-------" + message);
    
    // We give the Approver 60 seconds to respond
    RequestTimeout<TimeoutMessage>(TimeSpan.FromSeconds(60));
}
```

Saga 的目的是通过 saga 数据的状态来管理消息的工作流。Saga 将由 `IAmStartedByMessages<>` 中定义的消息或多个消息启动。后续消息在 `IHandleMessages<>` 中定义。超时有一个特殊的消息处理器，称为 `IHandleTimeouts<>`。我们可以在以下类图中观察到这一点：

![](img/8188651cea6a881a16242b395a81399a_57_0.png)

为了保存 saga 数据，数据需要映射到消息。NSB 会完成大部分映射工作，但它需要定义将用于将数据映射到消息的消息 ID。在我们的 saga 数据中，我们使用 RequestId 实例作为消息的数据主键。在 4.0 版本中，我们将此功能映射为使用 RequestId 实例来映射消息 ApproveRequestCommand 和 DenyRequestCommand，如以下代码所示，使用 ConfigureHowToFindSaga()：

```
public override void ConfigureHowToFindSaga()
{
    ConfigureMapping<ApproveRequestCommand>(x => x.RequestId).ToSaga(x => x.RequestId);
    ConfigureMapping<DenyRequestCommand>(x => x.RequestId).ToSaga(x => x.RequestId);
}
```

在 5.0 版本中，我们将此功能映射为使用 RequestId 实例来映射消息 ApproveRequestCommand 和 DenyRequestCommand，如以下代码所示，使用 `ConfigureHowToFindSaga(SagaPropertyMapper<> mapper)`：

```
protected override void ConfigureHowToFindSaga(SagaPropertyMapper<CreditCardRequestData> mapper)
{
    mapper.ConfigureMapping<ApproveRequestCommand>(x => x.RequestId).ToSaga(x => x.RequestId);
    mapper.ConfigureMapping<DenyRequestCommand>(x => x.RequestId).ToSaga(x => x.RequestId);
}
```

这些消息由消息处理器处理，例如 `Handle(DenyRequestCommand message)`。Saga 数据将从 NSB 中提取与消息的 RequestId 匹配的记录。NSB 只需要映射定义，它就会处理维护持久性和映射的记录，如以下代码所示：

```
public void Handle(DenyRequestCommand message)
{
    logger.Info("----------CreditCardSaga Handle----------" + message);
    var reply = new SubmitRequestReplyMessage
    {
        RequestId = Data.RequestId,
        Approved = false
    };

    ReplyToOriginator(reply);
    MarkAsComplete();
}
```

## 消息流

启动流程的源客户端将是 CreditCardSubmit。此客户端可以很容易地是一个网站或启动消息流程的另一种方式。SubmitRequestCommand 消息是 saga 的起始消息，该消息从 CreditCardSubmit 项目发送。它在 CreditCardSubmit 项目的 App.config 文件中定义为发送到 saga，如以下代码所示：

```
<UnicastBusConfig ForwardReceivedMessagesTo="MyAudits">
  <MessageEndpointMappings>
    <add Endpoint="CreditCardSaga" Messages="CreditCardMessages.SubmitRequestCommand, CreditCardMessages" />
    <add Endpoint="CreditCardSaga" Messages="CreditCardMessages.IRequestExpiredEvent, CreditCardMessages" />
  </MessageEndpointMappings>
</UnicastBusConfig>
```

我们可以在 MSMQ 中查看正在发送的消息，但如果 saga 正在运行，消息会处理得太快而无法观察。我们可以在不运行所有服务的情况下，通过在某个时间点拍摄快照来查看 saga 中的消息，如以下屏幕截图所示：

![](img/8188651cea6a881a16242b395a81399a_60_0.png)

一旦 saga 启动，它就会处理 SubmitRequestCommand 消息。Saga 将使用默认值和消息信息填充数据。请注意，RequestId 在 saga 数据中设置为与消息的 RequestId 实例匹配。这是为了将 saga 数据映射到 saga 处理的其他消息，但为了使此 RequestId 实例匹配，saga 需要映射的每个消息中都必须设置此键。此代码如下所示：

```
public class SolicitApprovalFromLevel1CommandHandler : IHandleMessages<SolicitApprovalFromLevel1Command>
{
    public Context<ItemViewModel> Context { get; set; }

    public void Handle(SolicitApprovalFromLevel1Command message)
    {
        Context.MarshalToUiThread(() => HandleOnUiThread(message));
    }

    private void HandleOnUiThread(SolicitApprovalFromLevel1Command message)
    {
        var item = new ItemViewModel
        {
            RequestId = message.RequestId,
            Card = message.Card,
            Cost = message.Cost
        };

        Context.Items.Add(item);
    }
}
```

另请注意，在生产系统中，信用卡会被加密。如我们在上一章中所讨论的，NSB 很容易处理数据的加密和消息的加密。这将按如下方式加载审批或拒绝的选择：

![](img/8188651cea6a881a16242b395a81399a_63_0.png)

Saga 将读取并处理来自会计部门的类型为 RecordEncumbranceReplyMessage 的消息。Saga 将响应原始客户端 CreditCardSubmit，以便其可以使用采购订单号和状态进行更新。我们可以在 saga 消息处理器代码的以下屏幕截图中看到这一点：

```
public void Handle(RecordEncumbranceReplyMessage message)
{
    logger.Info("--------CreditCardSaga Handle------" + message);
    var reply = new SubmitRequestReplyMessage
    {
        RequestId = Data.RequestId,
        Approved = true,
        PurchaseOrderNumber = message.PurchaseOrderNumber
    };

    ReplyToOriginator(reply);
    MarkAsComplete();
}
```

源客户端将通过 saga 从会计部门应用程序接收采购订单，如下所示：

![](img/8188651cea6a881a16242b395a81399a_66_0.png)

在原始请求者收到采购订单后，消息即完成。对于拒绝和错误，消息流将类似。

有人可能会问为什么消息流中有这么多工作。NSB 几乎完成了消息流和映射的所有工作。许多组织保存数据是为了处理错误。Saga 具有处理消息、端点和服务中错误的大量基本错误处理机制。我们已经编写了超时代码，以便消息在特定时间内发生。

