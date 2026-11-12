

要了解 Ginjector 在 flightapp-gwt 中的使用方式，让我们来看一下以下类图：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_243_0.png)

在此类图中，Ginjector 接口是 GIN API 的一部分。ScaffoldInjector 和 DesktopInjector 由 Roo 生成。以下是在上述类图中需要注意的重要点：

- ScaffoldInjector 扩展 Ginjector 接口，定义一个名为 getScaffoldApp 的方法，该方法返回类型为 ScaffoldApp 的实例。

- ScaffoldApp 是由 Roo 生成的类，用于定义桌面和移动浏览器初始化 GWT 应用程序的契约。

- DesktopInjector 扩展 ScaffoldInjector，并重写 getScaffoldApp 方法以返回 ScaffoldDesktopApp。这种返回类型的更改完全合法，因为 ScaffoldDesktopApp 是 ScaffoldApp 类的子类。

- ScaffoldDesktopApp 负责创建面向桌面 Web 浏览器的 Web UI，并在应用程序投入使用前执行所有初始化工作。同样，ScaffoldMobileApp 负责创建移动浏览器的 Web UI。

GIN 的 Ginjector 接口是 GIN 框架的核心，负责执行依赖注入。要使用 Ginjector，GWT 应用程序必须执行以下操作：

- 定义一个扩展 Ginjector 的接口——在 flightapp-gwt 中，这就是 DesktopInjector 接口。GWT 编译器负责提供该接口的实现。

- 在接口中定义一个或多个方法，用于返回应用程序其他部分将使用的顶级对象。Ginjector 根据 Ginjector 配置的绑定，通过注入低级对象的依赖项来创建顶级对象。在 flightapp-gwt 应用程序中，ScaffoldDesktopApp 和 ScaffoldMobileApp 表示顶级对象。

- 定义一个 GinModule 或 AbstractGinModule 类（两者均属于 GIN API），用于定义依赖项的绑定。flightapp-gwt 中的 ScaffoldModule 类为 EventBus、PlaceController 和 ApplicationRequestFactory 定义绑定。

- 在希望 Ginjector 执行依赖注入的类的方法、构造函数或字段上使用 @Inject 注释（属于 Guice API）进行标注。在 flightapp-gwt 中，ApplicationDetailsActivities、ApplicationMasterActivities 等使用 @Inject 注释来让 Ginjector 注入依赖项。

现在，让我们来看看 Roo 为之前描述的每个活动创建的代码。

以下代码显示 DesktopInjector 接口：

```
DesktopInjector.java
package sample.roo.flightapp.client.scaffold.ioc;
..
import sample.roo.flightapp.client.scaffold.ScaffoldDesktopApp;
import com.google.gwt.inject.client.GinModules;

@GinModules(value = {ScaffoldModule.class})
public interface DesktopInjector extends ScaffoldInjector {

    ScaffoldDesktopApp getScaffoldApp();
}
```

DesktopInjector 扩展 ScaffoldInjector 接口（后者又扩展 GIN 的 Ginjector 接口），并定义一个名为 getScaffoldApp 的方法，该方法返回 ScaffoldDesktopApp 对象。因此，由 GWT 编译器生成的 Ginjector 实现的责任是返回一个 ScaffoldDesktopApp 实例，其所有依赖项均已注入适当的实现。

@GinModules 注释指定负责定义依赖项及其提供程序的类（该类实现 `GinModule` 接口或扩展 `AbstractGinModule` 抽象类）。以下代码显示 `ScaffoldModule` 类，该类将 `EventBus`、`PlaceController` 和 `ApplicationRequestFactory` 依赖项绑定到各自的提供程序：

```java
ScaffoldModule.java
package sample.roo.flightapp.client.scaffold.ioc;

import com.google.gwt.event.shared.EventBus;
import com.google.gwt.event.shared.SimpleEventBus;
import com.google.gwt.inject.client.AbstractGinModule;
import com.google.gwt.place.shared.PlaceController;
import com.google.inject.Inject;
import com.google.inject.Provider;
import com.google.inject.Singleton;

public class ScaffoldModule extends AbstractGinModule {

    @Override
    protected void configure() {
        bind(EventBus.class).
        to(SimpleEventBus.class).in(Singleton.class);
        bind(ApplicationRequestFactory.class).
        toProvider(RequestFactoryProvider.class).
        in(Singleton.class);
        bind(PlaceController.class).
        toProvider(PlaceControllerProvider.class).
        in(Singleton.class);
    }

    static class PlaceControllerProvider implements Provider<PlaceController> {

        private final PlaceController placeController;

        @Inject
        public PlaceControllerProvider(EventBus eventBus) {
            this.placeController = new PlaceController(eventBus);
        }

        public PlaceController get() {
            return placeController;
        }
    }

    static class RequestFactoryProvider implements
    Provider<ApplicationRequestFactory> {

    }
}
```

由 Roo 生成的 ScaffoldModule 类扩展 AbstractGinModule 并重写 configure 方法，以将依赖项与其提供程序关联。AbstractGinModule 的 bind 方法将依赖项绑定到其提供程序。in(Singleton.class) 指示为应用程序仅创建 EventBus、PlaceController 和 ApplicationRequestFactory 的单个实例。PlaceControllerProvider 和 RequestFactoryProvider 静态内部类分别表示 PlaceController 和 RequestFactory 实例的提供程序。SimpleEventBus（属于 GWT API）是 EventBus 的一个实现。

这段代码中需要注意的一个重要点是，PlaceControllerProvider 使用 @Inject 注释来指示 Ginjector 注入 EventBus 的实现。因此，如果您正在创建对象层次结构，那么请在 Ginjector 中定义返回顶级对象的方法，因为层次结构中的低级对象可以使用 @Inject 注释利用 Ginjector 的依赖注入功能。

以下图表总结了 Scaffold 类如何利用 GIN 框架来引导应用程序：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_246_0.png)

Scaffold 类的 onModuleLoad 方法调用 DesktopInjectWrapper 的 getInjector 方法来访问 Ginjector 实现，随后调用 getScaffoldApp 以获取 ScaffoldDesktopApp 的实例。以下序列图进一步澄清了类之间方法调用的顺序：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_247_0.png)

在上述序列图中，对 DesktopScaffoldApp 的 run 方法的调用会导致 Web UI 的初始化。

既然我们了解了 flightapp-gwt 应用程序如何使用 GIN 并自行初始化，我们将研究 GWT 的 EntityProxy 和 RequestFactory 接口，以及脚手架代码如何使用它们。

## EntityProxy、RequestContext 和 RequestFactory 接口

实体代理表示客户端（JavaScript 端）对象，用于模拟服务器端上的实体，在 flightapp-gwt GWT 应用程序中，该实体是 JPA 实体。实体代理充当客户端与服务器之间传输数据的手段。在 GWT 中，充当实体代理的客户端对象扩展 GWT 的 EntityProxy 接口，并为相应服务器端实体中定义的字段定义抽象 getter 和 setter 方法。在 flightapp-gwt 中，我们有 Flight 和 FlightDescription JPA 实体；因此，Roo 分别生成相应的实体代理 FlightProxy 和 FlightDescriptionProxy。默认情况下，Roo 在实体代理中为相应 JPA 实体类中定义的所有属性创建抽象 getter 和 setter 方法。以下代码显示 FlightProxy 实体代理：

```
FlightProxy.java
package sample.roo.flightapp.client.managed.request;

import com.google.gwt.requestfactory.shared.EntityProxy;
import com.google.gwt.requestfactory.shared.ProxyForName;
import org.springframework.roo.addon.gwt.RooGwtMirroredFrom;

@RooGwtMirroredFrom("sample.roo.flightapp.domain.Flight")
@ProxyForName("sample.roo.flightapp.domain.Flight")
public interface FlightProxy extends EntityProxy {
    abstract Long getId();
```

