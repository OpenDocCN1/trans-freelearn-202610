
下图显示了一系列针对特定类的单元测试，这些测试使用依赖类方法的模拟替代品，以避免需要关心它们的需求：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_426_0.png)

依赖类的模拟版本应模仿调用方法（即你正在测试的方法）的预期，例如通过返回值或抛出异常。配置模拟是你需要掌握的单元测试关键设置方面。它可以让你编写非常复杂且多样的测试。我们将在本章后面详细介绍模拟。

同时，如果在开发测试代码时你不熟悉模拟方法，这个 http://en.wikipedia.org/wiki/Mock_object 链接是一个有用的介绍。当然，互联网上有许多其他网站描述其优势。以下是维基百科页面的摘录：

> '在面向对象编程中，模拟对象是以受控方式模仿真实对象行为的模拟对象。程序员通常创建模拟对象来测试其他对象的行为，这很大程度上类似于汽车设计师使用碰撞测试假人来模拟人类在车辆撞击中的动态行为。
> 
> 在单元测试中，模拟对象可以模拟复杂真实对象的行为，因此当真实对象不切实际或不可能纳入单元测试时，它们非常有用。如果实际对象具有以下任何特征，那么使用模拟对象来代替它可能很有用。

开发者在单元测试上投入越多，集成测试和最终用户发现缺陷的可能性就越小。但是请记住，证明方法在隔离状态下工作并不能证明你的应用程序按设计运行。集成测试仍然是测试应用程序的关键工具。

## 依赖注入、模拟和单元测试

到这个阶段，你可能在想，使用代码在测试执行期间如何在技术上替换或 *注入*（使用正确的术语）不同的已编译代码。为了说明依赖注入的各种选项，让我们从一个简单的代码示例开始。我们将在本章后面探讨如何将单元测试应用于 Apex Enterprise 模式。

下图显示了 `Car` 类模型的 **统一建模语言 (UML)**，该模型在设计时考虑了 SOC（关注点分离）。引擎、仪表板和数字读数显示等职责已被分离。这是一个纯 Apex 代码示例，用于说明如何使用依赖注入 (DI) 来管理类之间的依赖关系：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_427_0.png)

以下是 `Car` 类的代码。它依赖于 `Dashboard` 和 `Engine` 类中的方法。调用者必须设置并提供这些类的实例，方法才能正确运行：

```
public class Car {
    private Engine engine;
    private Dashboard dashboard;
    private Boolean isRunning;

    public Car(Engine engine, Dashboard dashboard) {
        this.dashboard = dashboard;
        this.engine = engine;
    }

    public void start() {
        dashboard.initialise();
        engine.start();
    }

    public void stop() {
        engine.stop();
        dashboard.off();
    }

    public Boolean isRunning() {
        return engine.isRunning();
    }
}
```

start 和 stop 方法包含确保仪表板和引擎相应初始化的功能。以下是 Dashboard 类的代码，它需要 Display 类的实例，该类处理信息的显示：

```
public class Dashboard {

    private Display display;

    public Dashboard(Display display) {
        this.display = display;
    }

    public void initialise() {
        display.backlight(true);
        display.showMessage(10, 20, 'Hello Driver!');
    }

    public void updateRPMs(Integer rpms) {
        display.showMessage(10, 10, 'RPM: ' + rpms);
    }

    public void off() {
        display.backlight(false);
    }

}
```

`Dashboard` 类上的方法与 `Display` 类集成，以在汽车数字显示屏的各个位置显示消息。`Display` 类没有依赖项，因此为简单起见此处未显示。（如果你想查看其代码，可以参考本章的示例代码）。最后，`Engine` 类显示如下：

```
public class Engine {

private Dashboard dashboard;
private Boolean isRunning;

public Engine(Dashboard dashboard) {
    this.dashboard = dashboard;
}

public void start() {
    dashboard.updateRPMs(1000);
    isRunning = true;
}

public void stop() {
    dashboard.updateRPMs(0);
    isRunning = false;
}

public Boolean isRunning() {
    return isRunning;

}
}
```

`Engine` 类中的方法调用 `Dashboard` 类上的方法，以向驱动程序更新引擎的 **每分钟转数 (RPM)**。

> 这是示例代码。实际代码可能包括表示各种硬件传感器的类。

首先让我们看看 Car 的集成测试是什么样子：

```
@IsTest
private class CarTest {

@IsTest
private static void integrationTestStartCar() {
    // Given
    Display display = new Display();
    Dashboard dashboard = new Dashboard(display);
    Engine engine = new Engine(dashboard);

    // When
    Car car = new Car(engine, dashboard);
    car.start();

    // Then
    System.assertEquals(true, car.isRunning());
    System.assertEquals(true, engine.isRunning());
    System.assertEquals(true, display.isVisible());
    System.assertEquals('Hello Driver!',
        display.getMessageShowAt(10, 20));
    System.assertEquals('RPM:1000',
        display.getMessageShowAt(10, 10));
}
}
```

> 此测试的设置看起来很简单，但想象一下，如果每个构建的类都需要创建其他配置和数据，设置逻辑可能会迅速增长。这个集成测试本身是完全有效且有用的。但是，正如我之前提到的，你不能仅仅因为彻底单元测试了方法，就假设当把所有东西放在一起时一切仍然可以工作！
> 
> 前面的测试代码分为三个主要部分：Given、When 和 Then。这种方法和思维来源于另一种称为行为驱动开发 (BDD) 的实践。你可以阅读 Martin Fowler 的网站上的更多内容。https://martinfowler.com/bliki/GivenWhenThen.html

## 决定在单元测试中测试什么和不测试什么

在本节中，让我们回顾我们想要编写哪种类型的单元测试，以及这些测试应该针对前面示例中的某些方法断言什么。

让我们考虑一下，在编写仅测试 Car.start 方法中以下逻辑的单元测试时，我们会断言什么。（请记住，我们目前不感兴趣测试 dashboard.initialise 或 engine.start 方法）：

```
public void start() {
    dashboard.initialise();
    engine.start();
}
```

对于此方法，我们会断言 dashboard.initialise 和 engine.start 方法实际上已被调用。因此，在此方法的单元测试中，我们只测试 start 方法的行为。在单独的单元测试中，我们也会测试 Dashboard.initialise 和 Engine.start 方法本身。

> 仅测试方法的行为或实现（例如，它调用了哪些方法），与断言值相比可能看起来很奇怪。但是，有时断言某些方法已被调用（以及被调用的次数）以及任何可能存在的参数已传递给它们，是验证代码按预期工作的关键。断言方法返回的值，作为测试正确行为的一种手段，在单元测试中也是有效的。

对于再次显示在以下代码中的 Engine.start 方法，我们希望断言 dashboard.updateRPMs 方法已使用正确的空转 RPM 值被调用，并且引擎状态已正确设置为运行中：

```
public void start() {
    dashboard.updateRPMs(1000); // Idle speed
    isRunning = true;
}
```

我们不关心该方法是否更新了显示器；那是直接测试 Dashboard.updateRPMs 方法的另一个单元测试的关注点。

以下是 Dashboard.updateRPMs 方法的代码。在为该方法编写单元测试时，我们应该验证传递给 rpms 参数的任何值是否已正确传递给 Display.showMessage 方法：

```
public void updateRPMs(Integer rpms) {
    display.showMessage(10,10, 'RPM: ' + rpms);
}
```
