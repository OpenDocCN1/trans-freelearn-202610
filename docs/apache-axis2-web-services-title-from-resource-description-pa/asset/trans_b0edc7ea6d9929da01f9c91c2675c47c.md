

# 客户端 API

+   4. 创建 SOAPEnvelope，并将其添加到消息上下文中（不是我们在前面案例中添加的方式）。这里，你需要创建一个完整的 SOAP 信封。
+   
+   `outMsgCtx.setEnvelope(createSOAPEnvelop());`

createSOAPEnvelop 方法的代码片段如下：

```
public SOAPEnvelope createSOAPEnvelop() {
    SOAPFactory fac = OMAbstractFactory.getSOAP11Factory();
    SOAPEnvelope envelope = fac.getDefaultEnvelope();
    OMNamespace omNs = fac.createOMNamespace(
            "http://ws.apache.org/axis2", "ns1");
    OMElement method = fac.createOMElement("echo", omNs);
    OMElement value = fac.createOMElement("echo", omNs);
    value.setText("Hello");
    method.addChild(value);
    envelope.getBody().addChild(method);

    return envelope;
}
```

在这个示例中，我们使用的是带有示例负载的默认 SOAPEnvelope，但我们可以根据需求创建复杂的 SOAPEnvelope。

+   5. 将一个消息上下文添加到操作客户端：
+   
+   `opClient.addMessageContext(outMsgCtx);`

+   6. 要发送消息，我们需要调用操作客户端中的 execute 方法：
+   
+   `opClient.execute(true);`

该布尔方法参数用于指明我们是希望以阻塞方式还是非阻塞方式调用它。如果值为 true，则调用将是阻塞式的。

+   7. 要访问响应消息上下文和响应 SOAPEnvelope：
+   
   ```java
   //pass message label as method argument
   MessageContext inMsgCtx = opClient.getMessageContext("In");
   
   SOAPEnvelope response = inMsgCtx.getEnvelope();
   System.out.println(response);
   ```

在本示例中，当我们调用 in-out MEP（消息交换模式）时，请求的消息标签是 Out，响应的值是 In；这就是为什么我们必须传入 In 作为消息标签值，以获取响应消息上下文。

一旦我们有了消息上下文，就可以使用它来访问 SOAPEnvelope、属性、传输头等。当我们运行这个代码示例时，应该在控制台输出中得到以下内容：

```
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Header />
    <soapenv:Body>
        <ns:echoResponse
            xmlns:ns="http://ws.apache.org/axis2/xsd">
            <return>
                Hello
            </return>
        </ns:echoResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

这里，我们讨论了 OperationClient 作为访问传入和传出 MessageContext 的手段。访问传出 MessageContext 或 SOAPEnvelope 的最有用的用例之一是添加消息头。然而，ServiceClient API 也可以用来添加 SOAP 头，如下所示：

```
sc.addHeader(SOAPHeaderBlock);
```

或者我们也可以添加消息头，如下所示：

```
sc.addStringHeader(new QName("http://sample.org/header","MyHeader"), "headervalue");
```

如果我们使用 TCP 监视器或类似的机制拦截消息，就可以在 SOAP 消息中看到 SOAP 头：

```
<soapenv:Header>
    <axis2ns1:MyHeader xmlns:axis2ns1=
        "http://sample.org/header">headervalue</axis2ns1:MyHeader>
</soapenv:Header>
```

同时，我们可以使用 ServiceClient 访问最后一个 OperationContext，如下所示：

```
OperationContext operationContext = sc.getLastOperationContext();
```

从 operationContext 中，我们可以获得 In 或 Out 的 MessageContext。

## 总结

在本章中，我们讨论了 Axis2 的关键组件之一，即 Axis2 客户端 API。我们讨论了如何以多种方式创建 Axis2 客户端实例，以及配置它们的各种方式。

接下来，我们讨论了最常用的 API，并使用代码示例更好地解释了这些场景。Axis2 客户端 API 非常方便，它有许多很棒的特性，比如异步 Web 服务调用、多传输选择等等。运行示例后，我们可以很容易地理解 Axis2 客户端 API 的基础；要理解其余 API，则需要我们编写更复杂的示例。

如前所述，按照设计，Web 服务被认为是无状态的。然而，如果没有会话管理支持，很难实现复杂的应用程序。为了理解会话支持的必要性，让我们考虑一个典型的银行应用程序。如果考虑与典型银行应用程序相关的事件序列，我们就会明白为什么需要会话：

- 首先，用户登录其账户（调用 `login` 方法）
- 取款（对其账户调用某个操作）
- 退出登录（完成交易）

我们很容易理解，前面提到的三个操作是相互关联的，并且同一个用户完成了所有这些调用。这意味着，在调用这些方法的过程中，需要有某种机制来跟踪用户和用户数据。这明确表明了以 Web 服务方式实现银行应用程序需要会话管理。当然，实现同一应用程序还有其他替代方法；但这样就需要有某种其他机制来识别和认证用户。

## Axis2 的无状态特性

到目前为止，我们知道 Axis2 架构将逻辑和数据分开。同时，Axis2 有两种类型的数据模型——**静态数据**和**运行时数据**。我们已经详细讨论过这些。在本章中，我们将进一步讨论 Axis2 动态数据层次结构，因为会话管理完全依赖于运行时数据。本章中的示例将帮助您更多地了解运行时数据和静态数据，最重要的是，在什么地方使用什么。

在 Axis2 框架中，每个单独的组件都被认为是**无状态**的。换句话说，服务实现类或处理器不应尝试存储任何局部变量。无状态实例能更好地支持减少并发控制问题，这主要是由竞态条件导致的。在 Axis2 处理器中，`MessageReceivers`、`TransportSenders`、`TransportReceivers`，甚至 `AxisEngine` 都被认为是无状态的，因此它们不会在这些类中保留任何状态。正因为如此，我们不必担心同一个处理器是只有一个实例还是多个实例。由于 Axis2 要求处理器保持无状态，在编写处理器时，你需要确保它内部不保留任何状态。例如，我们不能认为下面的处理器实现是一个好方法。如我们所见，它有一个类变量来存储 `messageContext`。因此，当我们把 Axis2 部署到并发环境中时，肯定会出现问题。所以，作为最佳实践，我们需要记住不要在代码中使用任何类变量。

```
public class InvalidHandler extends AbstractHandler {
    //Class variable to keep current MC
    private MessageContext currentMessageContext;
    public InvocationResponse invoke(MessageContext msgContext) throws AxisFault {
        currentMessageContext = msgContext;
        //We need to write whatever the log we need to have here
        return InvocationResponse.CONTINUE;
    }
}
```

上述代码示例展示了一个保留类变量 currentMessageContext 的处理器。在处理多个并发请求时，这会产生严重问题。因此，正确的方法是不使用类变量。你可以通过使用方法变量，或者将其存储在其他上下文中来解决这个问题；我们将在本章后面讨论这一点。下面的代码示例演示了处理变量的正确方式：

```
public InvocationResponse invoke(MessageContext msgContext) throws AxisFault {
    MessageContext currentMessageContext = msgContext;
    //We need to write whatever the log we need to have here
    return InvocationResponse.CONTINUE;
}
```

这并不意味着我们不能在 Axis2 中维护状态；它只是告诉我们，在实现类中保存状态不是正确的方法。最推荐的方式是使用上下文层次结构来存储必要的数据并管理会话。

## Axis2 中可用的会话类型

如前所述，如果没有适当的会话管理支持，使用 Web 服务实现企业级应用程序是非常困难的。另一方面，在 Web 服务框架中提供会话管理支持并不是强制要求。我们可能会有根本不需要会话管理的服务。

