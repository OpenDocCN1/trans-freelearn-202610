

为了管理一个 SOAP 会话，客户端需要在 SOAP 头部发送一个额外的引用参数，命名为 serviceGroupId（当客户端第一次调用部署在 SOAP 会话中的服务时，该参数会发送给客户端）。与此同时，SOAP 会话必须提供一种方式来跨不仅单个服务调用，而且服务组中的多个服务来管理会话。只要我们处于同一个 SOAP 会话中，我们就可以在 ServiceContext 中管理与服务相关的数据，如果我们想在组中的其他服务之间共享数据，那么我们就可以使用 ServiceGroupContext 来存储会话相关的数据。

当我们在 SOAP 会话中部署一个服务并且客户端第一次尝试访问该服务时，Axis2 会生成 serviceGroupId 并将其作为引用参数发送给客户端，位于 wsa:ReplyTo 中，如下所示。然而，我们在这里应该提到的是，要使 SOAP 会话得到支持，客户端和服务器都必须具有 WS-addressing 支持。

```xml
<wsa:ReplyTo>
    <wsa:Address>http://www.w3.org/2005/08/addressing/anonymous</wsa:Address>
    <wsa:ReferenceParameters>
<axis2:ServiceGroupId xmlns:axis2="http://ws.apache.org/namespaces/axis2">urn:uuid:65E9C56F702A398A8B11513011677354</axis2:ServiceGroupId>
    </wsa:ReferenceParameters>
</wsa:ReplyTo>
```

因此，如果客户端想保持在同一个会话中，那么他/她必须复制该引用参数，并在第二次调用服务时将其发送回服务器。只要客户端发送有效的 ServiceGroupId，他/她就可以使用同一个会话，并且服务可以维护会话相关的数据。与请求会话作用域不同，SOAP 会话有一个默认的超时时间，如果客户端在 30 秒的时间段内未访问服务，则会话将过期，如果客户端发送旧的 serviceGroupId，他/她将收到一个 AxisFault。我们可以通过按如下方式更改服务器的 axis2.xml 文件来更改默认超时时间：

```xml
<parameter name="ConfigContextTimeoutInterval">30000</parameter>
```

通过更改参数值，我们可以获得我们想要的超时间隔。

如前所述，将服务部署在 SOAP 会话中需要我们按如下方式更改 services.xml：

```xml
<service name="MyService" scope="soapsession">
</service>
```

createPayLoad 方法如下所示：

```java
public static OMElement createPayLoad(int intValue) {
    OMFactory fac = OMAbstractFactory.getOMFactory();
    OMNamespace omNs = fac.createOMNamespace(
        "http://ws.apache.org/axis2"
```

一旦我们运行代码，我们将在客户端看到以下输出：

```text
10
10
```

这意味着尽管我们调用了服务两次，但我们得到了相同的输出。这仅仅意味着没有会话发生，所以现在让我们稍微更改一下客户端代码，看看我们得到什么。只需添加以下代码行并再次运行客户端：

```java
opts.setManageSession(true);
sc.setOptions(opts);
```

现在你应该在客户端看到以下输出：

```text
10
20
```

这就简单地告诉我们，我们已以会话感知的方式调用了服务。

### Option 2: Using the service client

当我们使用服务客户端时，一旦我们将会话管理标志设置为 true，它也会发送回传输 cookie。因此，如果我们运行以下代码，我们将看到以下结果：

```java
ServiceClient sc = new ServiceClient();
Options opts = new Options();
opts.setTo(new EndpointReference(
"http://127.0.0.1:8000/axis2/services/MyService"));
opts.setAction("urn:add");
opts.setManageSession(true);
sc.setOptions(opts);
OMElement ele = sc.sendReceive(createPayLoad(10));
System.out.println(ele.getFirstElement().getText());
ele = sc.sendReceive(createPayLoad(10));
System.out.println(ele.getFirstElement().getText());
```

![](img/e2fb05de168053e6227dab034eff9353_179_0.png)

它简单地告诉我们，我们必须以会话感知的方式调用部署在传输会话中的服务。在这种情况下，我们不需要地址模块。

### Application scope

与所有其他作用域相比，Application 作用域具有最长的使用寿命，并且应用程序会话的生命周期等于系统的生命周期。如果我们在应用程序作用域中部署一个服务，那么该服务只有一个实例，显然该服务也只有一个 ServiceContext。在 Axis2 的世界中，如果我们考虑内存占用并且不想管理会话，那么一个好主意是将服务部署在应用程序作用域中。

当我们在应用程序作用域中部署服务时，客户端不需要发送任何额外数据来使用同一个会话。

要将服务部署在应用程序作用域中，我们需要更改 axis2.xml，如下所示：

```xml
<service name="foo" scope="application">
</service>
```

> 部署在应用程序会话作用域中的服务不能保证是线程安全的。

### Managing sessions using ServiceClient

正如我们现在所知，在客户端管理会话涉及一些工作。如前所述，在 SOAP 会话和传输会话中，客户端必须发送会话相关数据，如果他/她想保持在同一个会话中。也许他可以为 SOAP 会话通过复制所需的引用参数来做到这一点。然而，对于传输会话，用户如何访问传输以复制和发送 cookie？

为了让生活更轻松，Axis2 具有内置的能力，通过设置一个标志来在客户端会话中管理会话，我们已经使用了它。然后，根据服务端的会话，只要我们使用同一个服务客户端，它就会发送相应的数据。因此，主要要求是如果你想保持在同一个会话中，使用同一个服务客户端来调用服务。

如果我们想保持在同一个会话中，我们可以创建如下所示的服务客户端，并重用创建的服务客户端对象来调用服务：

```java
Options options = new Options();
options.setManageSession(true);
ServiceClient sender = new ServiceClient();
sender.setOptions(options);
```

一旦我们创建了 ServiceClient，如上所示，如果服务部署在 SOAP 会话中，它将复制 serviceGroupId 并从第二次调用开始发送该 ID。如果服务器发送会话 ID，例如 HTTP cookie，它将将其复制到 ServiceContext（在客户端）并在客户端第二次调用服务时将其发送回服务器。

### Summary

无状态特性是 Web 服务的主要特征之一。然而，这对于高级 Web 服务开发人员来说是一个限制。开发企业级应用程序使用 Web 服务并不容易，除非我们有一个会话管理层。Axis2 具有四级会话来解决企业级 Web 服务开发问题。在本章中，我们讨论了 Axis2 中可用的不同类型的会话。之后，我们使用示例服务和示例客户端来演示不同的会话以及它们的工作方式。

在下一章中，我们将重点关注另一个非常好的 Web 服务规范，称为 JAXWS。在那里，我们将介绍 JAXWS 注释、部署以及一些示例来说明如何使用它。

## 11 Developing JAX-WS Web Services

Java API for XML-based web services 是一个流行的标准，面向编写 Web 服务和 Web 服务客户端的 Java 开发人员。传统的 **contract first** 方法，即在 WSDL 中定义 Web 服务契约，然后从这些 WSDL 生成适当代码，并不是 Java 开发人员最自然的编程方式。JAX-WS 规范允许你编写常规的 Java 代码并适当地注释代码，以便将其制作成 Web 服务。因此，JAX-WS 是在 Java 中开发 Web 服务和客户端的 **code first** 方法。开发的简便性是通过将所有内容作为 Java 类编写来实现的，而无需编写任何特定的部署描述符文件，例如 Axis2 AAR 服务使用的 `services.xml` 文件。旧的 Java Web 服务规范如 JAX-RPC 要求 Web 服务类实现特定的接口。使用 JAX-WS，没有此类限制，常规 POJOs 可以通过最少的更改制作成 Web 服务。JAX-WS 也允许 contract first 开发。你将在本章后面看到如何使用 wsimport 工具从 WSDL 开始生成 JAX-WS 注释的 Web 服务。截至撰写本文时，Axis2 仅支持 JAX-WS 2.0 规范。

在本章中，我们将介绍：

- JAX-WS Annotations
- Server-side JAX-WS
- Client-side JAX-WS

到本章结束时，你将对 JAX-WS 注释有很好的理解，并且了解如何在 Apache Axis2 中开发 JAX-WS 服务和客户端。

