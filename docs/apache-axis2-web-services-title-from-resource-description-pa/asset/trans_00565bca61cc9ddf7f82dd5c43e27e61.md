
## 在服务级别为整个服务指定消息接收器

想象这样一个场景：你需要在 services.xml 文件中发布大量操作。在这种情况下，为每个操作都添加消息接收器似乎很令人头疼。然而，如果你可以为整个服务指定一个消息接收器，那么服务编写者的工作就会更轻松，同时也会简化 services.xml。

Axis2 内置支持 W.S.D.L. 2.0 定义的全部八种消息交换模式（MEP）。在 services.xml 中，你可以指定一种 MEP 及其对应的消息接收器，然后根据操作所属的 MEP，Axis2 会自动选取消息接收器并将其设置到该操作上。

在 operation 标签内，你可以添加一个属性来指定操作的 MEP，如下所示：

```
<operation name="sayHello" mep="http://www.w3.org/2004/08/wsdl/in-out" />
```

以下示例展示了为给定服务定义服务级消息接收器的方法：

```
<service>
    <Description>
        This is my first service, which will say Hello.
    </Description>
    <messageReceivers>
        <messageReceiver mep="http://www.w3.org/2006/01/wsdl/in-only" class="org.apache.axis2.rpc.receivers.RPCInOnlyMessageReceiver"/>
        <messageReceiver mep="http://www.w3.org/2006/01/wsdl/in-out" class="org.apache.axis2.rpc.receivers.RPCMessageReceiver"/>
    </messageReceivers>
    <parameter name="ServiceClass" locked="false">HelloWorld</parameter>
    <operation name="sayHello" mep="http://www.w3.org/2004/08/wsdl/in-out" />
</service>
```

根据 `services.xml`，`org.apache.axis2.rpc.RPCMessageReceiver` 是服务中所有 in-out 操作（任何属于 in-out MEP 的操作都会将该消息接收器分配为其消息接收器）的消息接收器。而对于 in-only MEP，服务级消息接收器是 `RPCInOnlyMessageReceiver`。如果你使用新的 `services.xml` 重新部署 `HelloWorld` 服务，当你再次调用该服务时，肯定会得到相同的结果。

## 指定服务级消息接收器并通过操作覆盖它们

有时，服务作者可能希望为某些操作使用不同的消息接收器，即使他已经定义了服务级消息接收器。要让某个操作覆盖服务级消息接收器，只需向要分配不同消息接收器的操作添加一个消息接收器元素即可。下面展示了一个采用这种技术的 `services.xml` 示例：

```
<service>
    <Description>
        This is my first service, which will say Hello.
    </Description>
    <messageReceivers>
        <messageReceiver mep="http://www.w3.org/2006/01/wsdl/in-only" class="org.apache.axis2.rpc.receivers.RPCInOnlyMessageReceiver"/>
        <messageReceiver mep="http://www.w3.org/2006/01/wsdl/in-out" class="org.apache.axis2.rpc.receivers.RPCMessageReceiver"/>
    </messageReceivers>
    <parameter name="ServiceClass" locked="false">HelloWorld</parameter>
    <operation name="sayHello">
        <messageReceiver class="org.apache.axis2.receivers.RawXMLINOutMessageReceiver"/>
    </operation>
</service>
```

操作 `sayHello` 使用了与其服务级消息接收器不同的消息接收器。

无论你是否在 `services.xml` 中指定它们，服务实现类中的所有公共方法都会暴露。Axis2 通过检查操作对应的 Java 方法来计算其 MEP。如果方法的返回类型是 void，那么 MEP 将为 in-only，否则将为 in-out。根据 MEP，将设置正确的消息接收器。

## 添加第三方资源

在很多情况下，你希望在 Web 服务中使用第三方库。此外，到目前为止，你应该知道 Axis2 中的每个服务和模块都是隔离的。因此，每个服务都有自己独立的类加载器。当你需要将第三方库包含到 Web 服务中时，Axis2 提供了一种机制来实现这一点。只需在服务归档文件中创建一个名为 lib 的文件夹，并将库或资源放入其中即可。

想象你有一个服务，需要使用名为 foo.jar 和 bar.jar 的库，以及名为 xyz.properties 的资源。那么你的服务归档文件应类似于以下截图：

![](img/e2fb05de168053e6227dab034eff9353_130_0.png)

## 服务 WSDL 和 Schema

当你开始编写相对更复杂的应用程序时，你会觉得需要在服务归档文件中包含 WSDL 文档和 Schema 文件。对于企业级应用程序，你不能让客户端依赖自动生成的 WSDL 文档。为了拥有一个可靠且定义明确的 WSDL 文档，有必要将服务与 WSDL 文档文件以及相应的 XML Schema 文件一起部署。因此，当你想要添加 WSDL 文档文件时，应该将其放在服务归档的 META-INF 目录中。这样做时你唯一需要记住的是，services.xml 中给出的服务名称与 WSDL 文档中定义的服务名称应该相同。包含 WSDL 文件的服务归档文件将具有以下结构：

## 填充服务骨架

Axis2 会生成一个服务骨架类，其中的方法可以抛出 UnsupportedOperation。我们所需要做的就是根据你想要提供的业务逻辑来实现这个服务骨架类。

## 运行 Ant 构建文件

完成服务骨架后，下一步是使用修改后的生成代码创建服务归档文件。为了让这项任务更简单，Axis2 会为你生成一个 Ant 构建文件来创建服务归档文件。你只需打开命令行控制台，进入你输出生成代码的文件夹，然后在控制台中输入 ant build 来运行 Ant 构建文件。服务归档文件将随即为你创建。

在本章中，我们只是粗略地了解了契约优先方法。在本书后面的内容中，你将更详细地学习代码生成和数据绑定。我们还将介绍一些示例以及可用的工具。

## 小结

在 Axis2 中，将 Java 类转换为 Web 服务非常简单。一旦你知道如何正确编写 `services.xml` 文件，你就可以创建更复杂的应用程序，而不仅仅是 POJO。部署服务只需创建服务归档文件并将其放入 Axis2 仓库中的 services 目录即可。WSDL 优先方法是创建服务最简单的方法，因为 Axis2 内置了对代码生成的支持，并且它提供了一套工具来让这项工作更加轻松。

在本章中，我们讨论了如何将简单的类文件转换为 Web 服务，以及如何以 REST 方式访问该服务。我们讨论了当服务因添加包名而变得稍复杂时会发生什么。我们还讨论了如何使用基于归档的部署机制。最后，我们简要讨论了契约优先方法。

在下一章中，我们将讨论如何扩展 Axis2 的核心功能，并使用 Axis2 模块提供额外的服务质量。

## 8

## 编写 Axis2 模块

Web 服务在行业中越来越受欢迎，并已成为应用程序集成的主要推动力之一。此外，由于使用 Web 服务的灵活性和优势，每个人都在尝试为其应用程序提供 Web 服务支持。因此，Web 服务框架需要支持新的、更加定制化的需求。正如我们在前面章节中已经讨论过的，Web 服务框架的主要目标之一是将传入的消息交付给目标服务。然而，仅仅将消息传递给服务是不够的；当今的应用程序还需要可靠性、安全性、事务性以及其他服务质量。

由于 Web 服务的普及，标准机构正在制定新的 Web 服务标准，而如果 Web 服务框架不够灵活，就很难支持这些新标准。从 Axis2 诞生之初，灵活性和可扩展性就是两个主要的设计考虑因素。Axis2 模块的理念是在不修改核心系统的情况下扩展系统的核心功能。例如，Axis2 将可靠性和安全性作为两个独立的模块来支持，而核心引擎完全独立于这两个服务质量模块。

在本章中，我们将讨论 Axis2 模块的强大功能，以及如何使用它们扩展 Axis2 以支持你自己的需求。具体来说，我们将讨论以下内容：

- Axis2 模块的简要历史
- 介绍模块概念
- 模块的结构
- 模块配置文件（`module.xml`）
- 可选的模块实现类
- 编写 `module.xml` 文件的步骤
- 部署和启用模块
- WS-Policy 及其在模块中使用的简要概述
