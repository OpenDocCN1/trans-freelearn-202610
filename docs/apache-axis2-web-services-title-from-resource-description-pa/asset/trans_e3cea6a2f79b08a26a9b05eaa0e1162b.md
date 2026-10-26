

![](img/e2fb05de168053e6227dab034eff9353_113_0.png)

如你在左侧所见，层级结构中最顶层的组件是 ConfigurationContext。ConfigurationContext 与其他上下文相比，唯一的区别在于，它是系统中在接收消息之前就已存在的唯一上下文。ConfigurationContext 持有对 AxisConfiguration 的引用，要创建 ConfigurationContext，必须有一个可用的 AxisConfiguration。

### ConfigurationContext

ConfigurationContext 是整个系统的运行时表示。要启动 Axis2，你需要有一个配置上下文。配置上下文的生命周期就是系统的生命周期，因此如果我们存储一个状态（一个属性），它将永远保留在那里（也就是说，直到系统关闭）。ConfigurationContext 不仅是所有其他上下文的父级，还可以被视为整个 Axis2 系统的发起者元素。

### ServiceGroupContext

当 Axis2 收到消息时，会创建一个 ServiceGroupContext 来存储和共享数据。要创建 ServiceGroupContext，需要有一个可用的 AxisServiceGroup 对象。一个 AxisServiceGroup 可能有一个或多个 ServiceGroupContext。不过，与该上下文关联的 AxisServiceGroup 只有一个。ServiceGroupContext 的生命周期取决于服务作用域（例如，应用、传输、SOAP 会话和请求）。如果服务作用域是应用，那么其生命周期将与系统生命周期相同。然而，如果服务作用域是“请求”，那么每次调用都会创建一个 ServiceGroupContext。如果我们希望在运行时在同一个服务组中的多个服务之间共享数据，那么 ServiceGroupContext 提供了实现这一目标的手段。

### ServiceContext

ServiceContext 表示给定服务的运行时数据。要创建 ServiceContext，我们需要有可用的 ServiceGroupContext 对象和 AxisService 对象。ServiceContext 的生命周期取决于服务作用域。如果我们希望在同一个服务的多次调用之间共享数据，那么可以使用 ServiceContext 作为存储这些数据的占位符。

例如，假设我们有一个包含三个操作的服务——login、doSomething 和 logout。同时，假设我们需要在这三个操作之间共享数据。这里，可以使用 ServiceContext 来实现这些目标。一个 ServiceGroupContext 中 ServiceContext 的数量取决于对应 AxisServiceGroup 中 AxisService 的数量。

### OperationContext

OperationContext 表示一个消息交换模式（MEP）的生命周期。通常情况下，操作上下文的生命周期比服务上下文的生命周期短。我们可以使用 OperationContext 在 MEP 的消息之间共享数据。例如，如果我们希望在请求和响应之间共享数据，那么可以使用 OperationContext 来实现。ServiceContext 中 OperationContext 上下文的数目与 AxisService 中 AxisOperation 操作的数目无关；它只取决于调用次数，例如，每次调用对应一个 OperationContext。

## 7 编写 Axis2 服务

在前几章中，我们介绍并讨论了各种 Axis2 概念，并强调了它们的重要性。从本章开始，我们将讨论如何在现实世界中使用这些概念。每一章都为你提供了一套全面的示例，帮助你清楚地理解这些概念。在你自己的环境中重复这些示例，将有助于你从每一章中获得最大收获。Axis2 为其 Web 服务引入了若干新特性。例如，注解支持、会话支持，以及在信息层次结构、POJO 和基于 Spring 的 Web 服务中存储会话感知数据的方法。

正如我们在前面章节中已经讨论过的，任何 Web 服务框架都有两个主要方面：首先，为 Web 服务提供托管环境；其次，为访问远程服务提供调用框架。托管服务包括实现服务、部署服务和管理服务生命周期。在第 5 章中，我们简要介绍了若干不同的服务部署方式。在本章中，我们将讨论如何实现服务并将其部署到 Axis2 Web 服务托管环境中。此外，本章将涵盖业界创建 Web 服务常用的两种基本方法。学完本章后，你将深入理解如何创建 Web 服务并在 Axis2 中部署它。在第 9 章“客户端 API”中，我们将讨论如何调用部署在 Axis2 或其他地方的 Web 服务。

- 现在打开浏览器，输入以下 URL，查看结果。这里需要注意的是，根据你在 Tomcat 中指定的端口，你可能需要修改 8080。

```
http://localhost:8080/axis2/services/HelloWorld/sayHello?name=Axis2
```

现在，你应该能在浏览器中看到 Hello Axis2。你可以通过更改 name 参数来进行测试，你会发现该服务会对输入的任何名称回复 Hello。

![](img/e2fb05de168053e6227dab034eff9353_121_0.png)

这一切意味着你已成功部署了服务；更具体地说，你已经以 REST（表述性状态转移）方式调用了该服务。

如前所述，前面的方法可以被视为在 Axis2 中编写和部署服务最简单的方法之一。虽然我们选择的示例非常简单，但这并不是说你就不能以这种方式编写非常复杂的服务。然而，唯一的限制是，你的服务类不能放在默认包之外的任何包中。如果你希望服务类位于自己的包结构中，则必须遵循一条略有不同的路径，我们将在本章稍后部分讨论。

考虑到 Axis2 提供的 POJO 支持，你应该知道，你可以部署带注解的 Java 类和不带注解（普通）的 Java 类。这里需要提及的是，Axis2 支持 JSR 181 注解规范。如果你不熟悉 Java 注解，那么可以这样说：注解是在使用 POJO 时提供元数据的一种机制。一个带有基本 JSR 181 注解的简单 Java 类可以写成如下形式。需要注意的是，使用这一特性可以提供大量数据并创建复杂的应用程序。

```
import javax.jws.WebService;
import javax.jws.WebMethod;
import javax.jws.WebParam;

@WebService (targetNamespace = "http://sample.org/helloWorld", name = "HelloWorld")
public class HelloWorld {
@WebMethod (action = "urn:sayHello" ,operationName = "sayHello")
public String sayHello(@WebParam (partName = "name") String name)
{
    return "Hello " + name;
}
}
```

注解仅在 JDK 1.5 或更高版本中受支持。此外，此时你无需担心缺乏注解使用经验。你这里的主要目标应该是学习 Axis2 概念，之后，使用注解就会非常简单。

### 带包的 POJO

正如我们在上一节中讨论的，当以单个 .class 文件部署 POJO 时，你不能将 Java 类声明在自定义包中。但如果你确实需要这样做，或者如果你的 POJO 的运行需要多个类，而你又不能再使用单个类的 POJO 方法，那么你需要采用我们接下来要讨论的方法。

现在让我们考虑如下类：

```
package book.sample;
import javax.jws.WebService;
@WebService
public class AddressService {
    public Address getAddress(String name) {
        Address address = new Address();
        address.setStreet("Street");
        address.setNumber("Number 15");
        return address;
    }
}
```

你可以看到，我们为 AddressService 类添加了注解。对应的 Address 类如下所示：

```
package book.sample;
public class Address {
    private String street;
    private String number;
    public String getStreet() {
        return street;
    }
}
```

