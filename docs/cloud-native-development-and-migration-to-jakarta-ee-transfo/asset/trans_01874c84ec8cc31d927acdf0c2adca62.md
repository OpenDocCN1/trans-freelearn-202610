

## 创建单元测试

在本节中，我们将向你展示如何创建一个单元测试。Cargo Tracker 应用较为复杂，这意味着你需要花费一些时间才能完全理解代码内部发生了什么。为了加快这一过程，我们首先要创建一个可以测试的方法。这样，你就不必了解 Cargo Tracker 应用的任何细节，而可以专注于创建单元测试。

我们用来教你单元测试的方法需要一个类来承载它。在 `org/eclipse/cargotracker/infrastructure/routing` 包内创建一个名为 `RoutingHelper` 的类。确保 `RoutingHelper` 类的内容如下所示：

```java
package org.eclipse.cargotracker.infrastructure.routing;

public class RoutingHelper {
}
```

我们无法为这个类创建单元测试，因为该类内部没有任何方法。让我们现在创建一个：

```java
boolean goesToNetherlands(RouteSpecification routeSpecification) {
    Location destination = routeSpecification.getDestination();
    return "Netherlands".equals(destination.getName());
}
```

这只是一个简单的教学方法，旨在教你单元测试的基础知识。前面示例中的方法只做一件事——检查 `RouteSpecification` 实例的目的地是否等于荷兰。如果位置等于荷兰，则返回 true；如果不是，则返回 `false`。

让我们为 `RoutingHelper` 类创建一个测试类。每个类都位于一个目录结构中。对于 `RoutingHelper` 类，这就是 `src/main/java/org/eclipse/cargotracker/infrastructure/routing/RoutingHelper.java`。通常，测试会位于测试包中，但目录结构相同。

下一步是创建一个将用于存放该类单元测试的测试类。在此目录结构中创建一个名为 `RoutingHelperTest` 的文件：`src/test/java/org/eclipse/cargotracker/infrastructure/routing/RoutingHelperTest.java`。注意两个路径中的第二个目录。生产代码位于 main 包中，而测试代码位于 test 包中。这样很容易区分，并且你不会意外地从生产代码中调用测试代码。

确保 `RoutingHelperTest` 类的内容如下所示：

```java
package org.eclipse.cargotracker.infrastructure.routing;
import static org.junit.jupiter.api.Assertions.*;
class RoutingHelperTest {
}
```

测试类创建完成后，我们就可以构建第一个单元测试了。让我们先从一个名为 goesToNetherlands_success 的空方法开始。我们给测试方法取与所测试方法相同的名称，并加上下划线和预期行为。这样我们就能在不阅读方法实现的情况下知道该方法测试什么以及预期结果是什么。第一个单元测试看起来像这样：

```java
@Test
void goesToNetherlands_success() {
}
```

每个单元测试都用 @Test 注解标记。这告诉 JUnit 哪些方法是测试方法，哪些不是。下一步是测试你的 goesToNetherlands 方法。在测试内部，我们将使用 given、when、then 结构。given 部分告诉读者测试的基础是什么以及测试需要哪些实例。when 部分调用生产代码。在 then 部分，我们验证发生的事情是否正确。

对于我们的第一个单元测试，我们需要在 given 部分创建一个 RouteSpecification 类。在 when 部分，我们使用在 given 部分创建的 RouteSpecification 类调用 goesToNetherlands 方法。在测试方法的最后一部分，我们将验证发生的事情是否正确。

生成的单元测试如下所示：

```java
@Test
void goesToNetherlands_success() {

    // given
    Location locationNetherlands = new Location(new UnLocode("XXXXX"), "Netherlands");
    Location locationGermany = new Location(new UnLocode("00000"), "Germany");
    RouteSpecification routeSpecification = new RouteSpecification(locationGermany, locationNetherlands, Date.from(Instant.now()));

    RoutingHelper routingHelper = new RoutingHelper();

    // when
    boolean goesToNetherlands = routingHelper.goesToNetherlands(routeSpecification);

    // then
    Assertions.assertTrue(goesToNetherlands);
}
```

在这里，我们在 given 部分创建了两个位置、一个路由规范和一个路由助手，并在单元测试的剩余部分中使用它们。

当你将此代码添加到 Cargo Tracker 项目中时，你的第一个单元测试就完成了。现在你需要做的就是运行单元测试。

## 如何创建集成测试

在本节中，我们将向你展示如何创建集成测试。我们今天构建的应用不再是孤立的。相反，我们在日常生活中开发的应用对内部和/或外部系统都有很多依赖。这些依赖都有各自的版本、接口等，当我们只替换应用的一小部分甚至很大一部分时，这些都需要继续正常工作。重要的是要知道，例如在升级软件依赖、服务器或操作系统后，你的应用是否仍然正常工作。

在下一节中，我们将带你了解集成测试是什么以及如何使用它们来测试你的应用。

### 什么是集成测试？

要验证你的系统是否仍然正常工作，你需要测试！使用单元测试，我们只测试一小段代码，而不是包、模块，甚至整个应用。它们并不是为此而设计的。要验证更大的部分或整个应用是否正常工作，你需要集成测试。

集成测试顾名思义，是关于测试你的应用与其他应用的集成，以查看被测试的应用在与其他应用交互时是否按预期行为。你也可以用它们来测试应用内部模块之间的集成，因为理想情况下，你希望在真实运行的应用上运行集成测试。在下一节中，我们将向你展示如何针对真实运行的 Cargo Tracker 应用运行测试。为此，我们将使用 Testcontainers。

### 什么是 Testcontainers？

Testcontainers 是由 AtomicJar 开发的一款软件。它是一个开源框架，为为测试目的启动和拆除 Docker 容器提供抽象。它为你提供了一个简单且轻量级的接口来管理你的容器。还有一个库可以让与 JUnit 5 的集成变得非常简单。使用 Testcontainers，你可以做到以下几点：

- 更轻松地更真实地测试我们的应用。
- 测试你的系统如何与真实依赖集成，而不必使用模拟或存根。例如，你可以启动一个真实的数据库。
- 确保你的测试环境可移植。每个用户都会启动相同的 Docker 容器，使得每个人都很容易开始使用和扩展集成测试。
- 防止开发后期出现错误。你现在可以在部署到开发、测试或验收环境之前测试集成。

### 设置 Testcontainers

要开始使用 Testcontainers，你需要设置一些东西。你需要一个有效的 Docker 环境。如果尚未设置，请查看第 6 章，了解如何在你的机器上设置 Docker。要将 Testcontainers 添加到你的项目中，你需要添加三个依赖项。

我们需要以下依赖项来运行集成测试：

- JUnit 5
- Testcontainers
- Testcontainers JUnit 集成

我们需要 JUnit 5 依赖项来运行你要创建的测试。Testcontainers 依赖项是与 Docker 实例交互的简便方法，而 Testcontainers JUnit 集成用于使前面提到的两个依赖项协同工作。添加这些依赖项非常简单。你只需要在项目的 pom.xml 文件中添加这些依赖项，如下所示：

```xml
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter</artifactId>
  <version>5.9.3</version>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>org.testcontainers</groupId>
  <artifactId>testcontainers</artifactId>
  <version>1.18.3</version>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>org.testcontainers</groupId>
  <artifactId>junit-jupiter</artifactId>
  <version>1.18.3</version>
  <scope>test</scope>
</dependency>
```

添加这些依赖项后，你就可以开始使用 Testcontainers 创建集成测试了。

