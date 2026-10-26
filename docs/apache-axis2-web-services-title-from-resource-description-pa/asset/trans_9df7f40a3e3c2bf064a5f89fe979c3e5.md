

## 使用 Axis2 的异步 Web 服务

无论使用哪种应用服务器，都可以通过编辑 Axis2.war 分发版的 web.xml 文件，让 Axis2 从自定义仓库工作。完成此操作后，就无需担心应用服务器是否会解压 WAR，以及解压到何处等问题。甚至可以将基于应用服务器的 Axis2 配置为使用远程仓库启动。首先，让我们看看如何配置 Axis2.war 以使用本地文件系统。在此，你需要在 web.xml 文件的 servlet 部分添加以下初始化参数：

```
<servlet>
    <servlet-name>AxisServlet</servlet-name>
    <display-name>Apache-Axis Servlet</display-name>
    <servlet-class>org.apache.axis2.transport.http.AxisServlet</servlet-class>
    <init-param>
        <param-name>axis2.xml.path</param-name>
        <param-value>path to custom axis2.xml (you need this only if you want to override the default axis2.xml)</param-value>
    </init-param>
    <init-param>
        <param-name>axis2.repository.path</param-name>
        <param-value>full path to the custom repository</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
```

如前所述，你可以将 web.xml 文件配置为从 URL 仓库启动 Axis2。在这种情况下，你需要添加以下两个参数：

```
<servlet>
    <servlet-name>AxisServlet</servlet-name>
    <display-name>Apache-Axis Servlet</display-name>
    <servlet-class>org.apache.axis2.transport.http.AxisServlet</servlet-class>
    <init-param>
        <param-name>axis2.xml.url</param-name>
        <param-value>http://localhost/myrepo/axis2.xml</param-value>
    </init-param>
    <init-param>
        <param-name>axis2.repository.url</param-name>
        <param-value>http://localhost/myrepo</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
```

如图所示，我们添加了两个名为 axis2.xml.url 和 axis2.repository.url 的参数。你可以根据环境设置修改这些值。

异步 Web 服务的概念是一个非常重要的特性，用户可以调用一个 Web 服务并继续其工作，直到收到回复。在传统的同步模型中，客户端必须等待（或阻塞）直到收到响应。更重要的是，在 Axis2 中，你在客户端和服务器端都获得了异步支持。在客户端，我们可以以阻塞或非阻塞方式调用服务。选择调用方式非常简单。在服务器端，我们可以拥有处理时间很长的服务。在这种情况下，服务可以以非阻塞方式处理，一旦收到消息，就会发送确认，并在稍后发送响应。Axis2 在客户端支持两种不同的异步 Web 服务调用方式；其中一种需要 WS 地址支持，另一种不需要。我们将分别用代码示例讨论它们。

### 什么是同步 Web 服务？

如前所述，同步 Web 服务是阻塞式服务调用，应用程序在收到响应之前会被阻塞。通过这种类型的调用，用户必须等待直到收到想要的响应。如果这是一个 GUI 应用程序，那么在收到回复之前应用程序将变得无响应。

### 什么是异步 Web 服务？

在上一节中，我们讨论了同步 Web 服务。异步 Web 服务与之相反。异步 Web 服务是非阻塞调用，用户可以在不阻塞用户应用程序的情况下调用服务。

### 客户端异步

Axis2 的主要设计目标之一是提供非常好的异步 Web 服务支持，而异步支持对客户端更有用。因此，Axis2 对客户端的异步服务调用提供了非常好的支持。Axis2 在客户端支持两种类型的异步调用：

- **应用级异步支持**：在这种情况下，我们只有应用级异步支持。然而，在传输层（传输发送器），我们有阻塞行为。换句话说，当我们调用服务时，我们提供一个回调对象，然后 Axis2 将其注册到传输层。一旦传输层收到响应，它就会调用回调。为了实现这种类型的异步支持，它不需要 WS-Addressing 支持，因为我们在传输层没有异步行为。

```
public class MyCallBack implements AxisCallback {
    public void onMessage(MessageContext messageContext) {
        SOAPBody msg = messageContext.getEnvelope().getBody();
        System.out.println(msg);
    }

    public void onFault(MessageContext messageContext) {
        messageContext.getFailureReason().printStackTrace();
    }

    public void onError(Exception e) {
        System.err.println(e.getMessage());
    }

    public void onComplete() {
        System.out.println("Invocation is complete");
        //In real application you do not need to terminate the program
        System.exit(0);
    }
}
```

如我们所见，接口中有四个方法。现在让我们逐一查看：

- **onMessage**：如前所述，WSDL 2.0 定义了 MEP 或消息交换模式。其背后的思想是客户端和服务器之间的消息调用模式。客户端可以发送两条消息并得到一个响应（in-in-out），或客户端发送请求并得到响应（in-out），等等。onMessage 背后的思想是在收到消息时调用用户操作。在我们的实现中，我们需要打印消息，但根据需求，我们可以在方法内部实现我们的代码。
- **onFault**：有时我们可能会在服务调用中收到故障，用户可能对此感兴趣。因此此方法有助于实现它。
- **onError**：在调用服务时，如果客户端出现问题，它将使用此方法通知用户。
- **onComplete**：当 MEP 完成时，用户将通过此方法收到通知。在此示例中，当 Axis2 调用 onComplete 方法时，我们终止应用程序。然而，在实际应用程序中，我们可以在那里实现我们的用户操作。

```
("http://localhost:8080/axis2/services/EchoService");
opts.setAction("urn:echo");
client.engageModule("addressing");
opts.setUseSeparateListener(true);
client.setOptions(opts);
client.sendReceiveNonBlocking(createPayLoad(), new MyCallBack());
System.out.println("send the message");
while (true) {
    Thread.sleep(100);
}
}
```

第一个高亮行显示了我们如何启用 addressing 模块，第二个高亮行显示了如何通过在 Option 对象中设置 Option 值在客户端启用单独的监听器。

### 总结

在本章中，我们讨论了 Axis2 的许多高级特性，并附带示例。特别是 REST 和 MTOM 在 Web 服务的上下文中非常有用。你可以使用 REST 作为优化消息线格式的一种方式，你可以使用 MTOM 以高效的方式发送二进制数据。然后我们讨论了使用自定义仓库创建 Axis2，该仓库可能位于本地或远程。接下来，我们研究了在各种应用服务器中部署 Axis2。最后，我们讨论了如何使用 Axis2 以异步方式调用服务。

现在我们已经涵盖了 Axis2 的大部分必要概念和特性。我们讨论了如何将 Axis2 用作服务器和客户端。

在下一章（本书的最后一章）中，我们将讨论如何为你的 Web 服务添加服务质量。特别是，我们将讨论安全性和可靠性以及如何通过代码示例使用它们。

