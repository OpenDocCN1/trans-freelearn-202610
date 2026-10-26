
现在，在 `classes` 目录下创建一个名为 `META-INF` 的目录，并将 `module.xml` 文件复制到其中。然后，我们的 `classes` 目录应该如下面的截图所示：

![](img/e2fb05de168053e6227dab034eff9353_146_0.png)

现在，从 `classes` 目录创建一个 ZIP 文件，并将该 ZIP 文件重命名为 `counter-module.mar`。

部署模块非常简单，只需将文件放入 `TOMCAT_HOME/webapps/axis2/WEB-INF/modules` 或 `repository/modules` 目录即可。在这里，我们将重点讨论在 Tomcat 或你喜欢的 Web 应用服务器中部署模块。众所周知，Axis2 不支持模块的热部署，因此仅仅放入模块不会对运行时系统产生任何更改。要部署我们的模块并将必要的更改应用到运行时，我们需要重启 Axis2；换句话说，我们需要重启应用服务器。

需要注意的是，仅仅部署模块并不会将其处理器（handler）添加到处理器链中；要将处理器添加到系统中，必须启用（engage）该模块。现在，让我们看看如何在 Axis2 中启用模块。为此，我们可以使用 Axis2 Web 管理控制台。此外，为了简单起见，让我们尝试使用 Web 管理控制台将该模块启用到系统中的所有服务（我们也可以使用管理控制台仅启用单个服务）。我们可以通过以下步骤将模块启用到系统中：

- 1. 访问 http://localhost:8080/axis2/。
- 2. 点击“Administration”选项卡。这将打开一个新页面，并要求输入用户名和密码。

> 还有其他启用模块的方式，例如，要全局启用模块，用户可以编辑 axis2.xml 并添加 `<module ref="modulename" />` 元素。此外，如果用户想要将其启用到特定服务，可以通过编辑 services.xml 并添加上面提到的相同标签来实现。

## 高级 module.xml

这里，我们看到了一个非常简单的 module.xml，但对于非常复杂的应用程序，我们需要在 module.xml 文件中进行更多配置。我们可能需要参数、WS-Policy 和端点。现在让我们逐一简要了解它们。

### 参数

在此处添加参数与在 services.xml 或 axis2.xml 中添加参数相同。我们只需要将以下标签添加到 module.xml 中，Axis2 就会为我们处理正确的事情：

```
<parameter name="foo">bar</parameter>
```

我们可以在 module.xml 中拥有任意数量的参数，当我们需要访问该参数时，可以按照以下步骤操作：

- - 首先，我们需要获取 AxisModule。我们可以通过使用 init 方法（Axis2 传递 AxisModule）来做到这一点，或者我们需要从 ConfigurationContext（在 Module 实现类中）或从 messageContext（在处理器中）获取相应的 AxisModule。
- - 之后，我们可以从 AxisModule 中请求参数。

### WS-Policy

在 module.xml 中指定 WS-Policy 元素是配置模块的一种方式。我们可以在 module.xml 中添加 WS-Policy 元素。如果我们考虑 Sandesha2（可靠消息实现）模块，我们可以找到以下策略元素：

```
<wsp:Policy xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy" xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd" xmlns:sandesha2="http://ws.apache.org/sandesha2/policy" wsu:Id="RMPolicy">
    <sandesha2:RMAssertion>
        [REST OF THE FILE]
    </sandesha2:RMAssertion>
</wsp:Policy>
```

### 端点

在 Axis2 中，端点是 Web 服务的一个操作。因此，添加端点与添加操作相同。关键问题是——为什么我们需要向模块添加端点？为了理解这一点，假设我们有一个模块，并且该模块具有一组控制操作。最合适的例子是可靠消息传递，因为它有许多控制消息（例如，创建序列、最后消息、终止序列）。假设我们需要以可靠的方式调用一个服务。它首先需要与该服务建立序列。为此，它将向我们需要访问的服务发送名为 `createSequence` 的控制消息。但该服务没有该方法，因此如果我们尝试在不添加该方法的情况下发送 `createSequence` 消息，Axis2 将抛出一个异常，提示**无法分发**。因此，添加端点将解决该问题。因此，当我们启用 Sandesha 时，它会在我们启用模块时（或者如果我们将其启用到整个系统，则为所有服务）向服务添加一个名为 `createSequence` 的方法。当请求到来时，Axis2 将毫无问题地进行分发。此操作或端点具有自己的消息接收器来处理相应的事情。

因此，很明显，当模块需要交换控制操作时，需要添加端点来表示这些操作。添加端点非常简单。我们需要做的是添加一个或多个操作元素，以及一个消息接收器和一组操作映射。为了对此有所了解，让我们以 Sandesha 模块作为参考。它的 `module.xml` 具有以下操作标签来添加控制操作。请记住，当我们在 `module.xml` 中有操作标签时，Axis2 将完成所有处理，包括创建 `AxisOperation` 并添加它。作为模块作者，我们只需要在 `module.xml` 文件中指定它们：

```
<operation name="Sandesha2OperationInOut" mep="http://www.w3.org/2006/01/wsdl/in-out">
    <messageReceiver class="org.apache.sandesha2.msgreceivers.RMMessageReceiver"/>
    <!-- namespaces for the 2005-02 spec -->
    <actionMapping>http://schemas.xmlsoap.org/ws/2005/02/rm/CreateSequence</actionMapping>
    <actionMapping>http://schemas.xmlsoap.org/ws/2005/02/rm/AckRequested</actionMapping>
    ..................
</operation>
```

### 编写 Axis2 模块

如果我们查看 Sandesha 的 module.xml，我们将能够学习和了解更多关于编写 module.xml 文件的内容。

### 总结

在本章中，你了解了 Axis2 模块的重要性。我们还了解到，Axis2 模块提供了扩展 Axis2 核心功能的一种非常灵活的方式，并提供了高质量的服务。此外，我们通过编写一个示例模块并演示大多数常用配置设置来讨论模块及相关概念。在我们的示例应用程序中，我们讨论了如何编写处理器、如何编写模块实现类，以及最后如何将所有内容组合在一起并部署模块。在本章末尾，我们学习了如何将模块启用到 Axis2。

在本章中，我们讨论了如何编写和部署服务，以及如何编写和部署模块。在下一章中，我们将讨论如何调用远程服务。在那里，我们将讨论如何将 Axis2 用作客户端并以多种方式调用服务。

## 客户端 API

Web 服务是应用程序集成（组合）中常用的方法之一；在集成应用程序时，一个应用程序既可以充当消费者（客户端），也可以充当生产者（服务器）。一个 Web 服务框架应该能够部署服务，也能够访问服务。到目前为止，我们已经讨论了它的部署方面。因此，在本章中，我们将重点关注客户端，以及如何使用 Axis2 访问远程服务。在高层次上，Axis2 运行时并不区分客户端和服务器端。此外，它在服务器端和客户端使用相同的执行框架。正如我们已经讨论过的，在服务器端，我们有服务。因此，为了在客户端也保持对称性，Axis2 在我们使用客户端 API 时会创建一个虚拟服务。在本章中，我们将通过示例用例来探讨客户端 API 的各个方面。

在本章中，我们将讨论 Axis2 客户端最常用的 API，并解释如何通过示例代码使用它们。首先，我们将讨论创建 Axis2 客户端实例的各种方法。然后，我们将讨论可用的 API，最后，我们将讨论 Axis2 客户端 API 的更高级用法。具体来说，我们将详细涵盖以下内容：

- - Web 服务客户端的概念
- - 同步和异步服务调用
- - Axis2 客户端 API，即 `ServiceClient` 和 `OperationClient`
- - 通过示例演示 `ServiceClient` API
- - 通过示例演示 `OperationClient` API
