

abstract Integer getVersion();
abstract Date getDepartureDate();
abstract Date getArrivalDate();
abstract FlightDescriptionProxy getFlightDescription();
abstract void setId(Long id);
...

Roo 的 @RooGwtMirroredFrom 注释指定了创建 FlightProxy 实体代理所对应的 JPA 实体的完全限定类名。

> 正是 @RooGwtMirroredFrom 注释使实体代理与相应的 JPA 实体保持同步。

GWT 的 @ProxyForName 注释指定实体代理所代表的服务器端实体。请注意，在上面的代码中，getFlightDescription 方法被定义为返回 FlightDescriptionProxy，因为它代表与 FlightDescription JPA 实体对应的实体代理。

要调用服务器端服务，GWT 应用程序的客户端代码需要为远程服务提供客户端存根。客户端代码使用服务存根来调用远程服务。服务存根由扩展 GWT 的 RequestContext 接口并定义与相应远程服务方法签名相似的方法的接口来定义。我们将稍后看到远程服务类中定义的方法与客户端服务存根之间的区别。

默认情况下，Roo 生成富实体，生成的应用程序没有服务层；因此，对于 Roo 生成的 GWT 应用程序，服务存根定义了与 JPA 实体类对应的方法。以下代码显示了 flightapp-gwt 应用程序中与 Flight JPA 实体对应的 Roo 生成的 FlightRequest 服务存根：

```
FlightRequest.java
package sample.roo.flightapp.client.managed.request;
import com.google.gwt.requestfactory.shared.InstanceRequest;
import com.google.gwt.requestfactory.shared.Request;
import com.google.gwt.requestfactory.shared.RequestContext;
import com.google.gwt.requestfactory.shared.ServiceName;
import org.springframework.roo.addon.gwt.RooGwtMirroredFrom;
@RooGwtMirroredFrom("sample.roo.flightapp.domain.Flight")
@ServiceName("sample.roo.flightapp.domain.Flight")
public interface FlightRequest extends RequestContext {
    abstract Request<java.lang.Long> countFlights();
    abstract Request<java.util.List<? extends FlightProxy>>
    findAllFlights();
    ....
    abstract InstanceRequest<? extends FlightProxy, java.lang.Void>
    remove();
    abstract InstanceRequest<? extends FlightProxy, java.lang.Void>
    persist();
}
```

正如此代码所示，FlightRequest 服务存根扩展了 GWT 的 RequestContext 接口。GWT 的 @ServiceName 注释指定与客户端服务存根对应的服务器端服务类的完全限定名称，在 FlightRequest 存根的情况下即为 Flight JPA 实体。上述代码显示 FlightRequest 服务存根定义了返回以下内容的方法：

- Request<T> - 其中 T 表示服务器端类中相应方法的实际返回类型。例如，countFlights 方法在 Flight JPA 实体中返回 java.lang.Long，FlightRequest 存根中的 countFlights 方法也是如此。如果服务器端服务类中的方法返回实体，则客户端服务存根返回相应的实体代理。例如，Flight JPA 实体中的 findAllFlights 方法返回 java.util.List<Flight>，因此 FlightRequest 存根中的 findAllFlights 方法返回 java.util.List<FlightProxy>。需要注意的是，与服务器端服务的静态方法对应的方法在客户端服务存根中返回 Request<T> 类型。

- InstanceRequest<P, T> - 其中 P 表示相应服务器端服务方法作用的实体类型，T 表示方法的实际返回类型。例如，Flight JPA 实体的 persist 方法作用于 Flight 实体实例并返回 void；因此，FlightRequest 存根中相应方法的返回类型是 InstanceRequest<FlightProxy, java.lang.Void>。请注意，只有与服务器端服务上的实例方法对应的存根方法才返回 InstanceRequest。

> 正是 Roo 的 @RooGwtMirroredFrom 注释使客户端存根与相应的 JPA 实体保持同步。

现在，让我们看看 RequestFactory 如何帮助客户端代码与服务器端代码之间的通信。

RequestFactory 充当实体代理与服务器端相应实体之间的通信桥梁。RequestFactory 管理实体代理，并负责将服务器端实体属性值复制到相应的实体代理，反之亦然。在您的 GWT 应用程序中，需要定义一个扩展 RequestFactory 接口的接口，并提供返回服务器端服务客户端存根的方法。Roo 创建了一个 ApplicationRequestFactory 类（我们之前讨论过，这是使用 GIN 创建的），如下所示：

```
package sample.roo.flightapp.client.managed.request;

import sample.roo.flightapp.shared.scaffold.ScaffoldRequestFactory;

public interface ApplicationRequestFactory extends
ScaffoldRequestFactory {
    FlightRequest flightRequest();
    FlightDescriptionRequest flightDescriptionRequest();
}
```

![](img/b8c379a2ed921ead3aa2b01811dc29fa_250_0.png)

上图显示 ApplicationRequestFactory 扩展了 Roo 生成的 ScaffoldRequestFactory 接口，而该接口又扩展了 GWT 的 RequestFactory 接口。创建这种层次结构的目的是，如果您想要定义自定义客户端服务存根方法，则可以将它们添加到 ScaffoldRequestFactory。

> > ApplicationRequestFactory 由 Roo 管理；因此，您不应手动修改它以添加自定义客户端服务存根方法。相反，请将它们添加到 ScaffoldRequestFactory。

实体代理与服务器端实体之间的交互通过在 GWT Web 应用程序的 web.xml 文件中配置 GWT 的 RequestFactoryServlet 来实现。

> > 您不需要在 flightapp-gwt 项目的 web.xml 文件中配置 RequestFactoryServlet，因为在执行 gwt setup 命令时，Roo 会在 web.xml 文件中配置 GWT 的 RequestFactoryServlet。

以下代码显示了 flightapp-gwt 应用程序 web.xml 文件中 RequestFactoryServlet 的配置：

```
web.xml
<servlet>
  <servlet-name>requestFactory</servlet-name>
  <servlet-class>
    com.google.gwt.requestfactory.server.RequestFactoryServlet
  </servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>requestFactory</servlet-name>
  <url-pattern>/gwtRequest</url-pattern>
</servlet-mapping>
```

此配置显示，默认情况下 Roo 将 RequestFactoryServlet 映射到 /gwtRequest URL，您可以更改该映射。如果更改 RequestFactoryServlet 的 URL 映射，则还需要更改 ScaffoldModule（我们之前讨论过的 Roo 生成的 GIN 模块）创建 ApplicationRequestFactory 的方式。

RequestFactory 要求服务器端实体必须定义无参构造函数、getId、getVersion 和 find<Entity> 方法。getVersion 应该返回版本，getId 应该返回与实体实例关联的唯一 ID，而 find<Entity> 方法接受实体 ID 并返回相应的实体实例。因此，如果您使用 Roo entity 命令创建实体，请确保不要将标识符字段或版本字段的名称设置为除 id 和 version 之外的任何名称。当您执行 Roo gwt setup 命令，并且 Roo 发现 ID 和 版本 字段的名称与 id 和 version 不同时，则它不会为 GWT 应用程序生成脚手架。

现在，让我们看看 Roo 生成的 GWT 中的一些活动和位置。

## 活动和位置

Roo 生成的 GWT 代码由许多基类和接口组成，旨在为对 JPA 实体执行 CRUD 操作提供一致的方法。在本节中，我们将介绍一些重要的类、接口和概念，这些内容将为您理解 Roo 生成的 GWT 代码提供起点。

Roo 生成的 GWT Web UI 由一个 **主显示区域** 和一个 **详情显示区域** 组成。**主显示区域** 显示可从 Web UI 管理的实体列表。以列表形式显示 **Flight** 和 **FlightDescriptions** 的小部件代表 **主显示区域**。**详情显示区域** 显示可以对 **主显示区域** 中显示的每个实体执行的活动。显示系统中 **Flight** 或 **FlightDescription** 实体实例列表、用于创建新实体实例的表单、用于编辑实体实例的表单等的区域代表 **详情显示区域**。

以下图显示了 flightapp-gwt 应用程序中的 **主显示区域** 和 **详情显示区域**：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_252_0.png)

现在，让我们来看看 Roo 生成的 flightapp-gwt 应用程序中活动、位置和视图的一些示例。

