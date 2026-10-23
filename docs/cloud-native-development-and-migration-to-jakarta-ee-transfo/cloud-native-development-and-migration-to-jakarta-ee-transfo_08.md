## 让应用程序更易测试

Cargo Tracker 应用程序已经有单元测试和良好的测试覆盖率。但这并不意味着你当前的项目或下一个项目也是如此。有可能你当前的项目没有单元测试，或者测试覆盖率较低。

测试覆盖率低或单元测试不足可能会带来风险。如果在迁移过程中某些东西发生了变化而无法正常工作，该怎么办？你将如何知道哪里出了问题？这时，各种不同类型的测试就派上了用场。在本章中，你将学习如何创建不同类型的测试，来测试应用程序的不同方面。

我们将涵盖以下主要主题：

- 测试对迁移的影响
- 衡量项目的代码覆盖率
- 关于**测试驱动开发**（TDD）的一点说明
- 如何创建单元测试
- 如何创建集成测试

到本章结束时，你应该了解有哪些类型的测试、何时使用它们，以及如何创建这些测试。




### 技术要求

对于本章，你需要在系统上安装 Docker。有关详细说明，请参阅第 6 章，其中涵盖了 Docker 的完整安装。我们将使用 Docker 来启动集成测试的实例。本章的第一部分可以在没有 Docker 的情况下完成；然而，为了从本章中获得最大价值，安装 Docker 很重要。

你可以在 GitHub 上找到本章的所有代码示例：https://github.com/PacktPublishing/Cloud-Native-Development-and-Migration-to-Jakarta-EE/tree/chapter-5。该仓库包含本章的所有示例。

### 测试对迁移的影响

测试是衡量你的迁移是否成功或项目中仍需修复某些部分的绝佳工具。当所有测试都能正常工作时，意味着这些测试覆盖的功能与迁移前一样正常工作。也有可能在迁移过程中某些测试开始失败，这意味着迁移的某些部分破坏了功能。

如果你要迁移的项目已有足够的测试来覆盖功能，你可以快速浏览本章，看看是否有新的内容想要纳入你的测试策略，或看到测试 Java 应用的替代方法。迁移应用时，不必彻底改造当前的测试策略。

如果要迁移的项目几乎没有测试，添加测试可以极大地帮助你，因为测试会指示项目是否仍在正常工作。本章介绍的是编写测试并为你要迁移的项目提供测试覆盖率的众多方法之一。还有很多其他值得考虑的用于测试应用的库，例如 Arquillian、Jakarta REST client 和 MicroProfile REST client。

### 衡量项目的代码覆盖率

拥有测试是很好的，但你知道项目中有多少部分被项目内的测试覆盖了吗？这就是测试覆盖率的作用。测试覆盖率告诉你有多少生产代码被测试覆盖。这告诉你哪些功能被测试覆盖，哪些功能必须手动测试。

当你使用 IntelliJ 时，衡量测试覆盖率非常容易。你只需导航到一个测试类。在测试类中，你可以右键单击该类以带覆盖率运行测试，或者也可以从菜单选项中带覆盖率运行测试，如以下屏幕截图所示：

Figure 5.1 – Running tests with coverage

运行带覆盖率的测试后，你会看到一份报告，显示有多少生产代码被测试覆盖。它看起来是这样的：

Figure 5.2 – Test coverage result

在之前的屏幕截图中，你可以看到各个类的测试覆盖率以及类被覆盖的百分比。RouteSpecification 类仅覆盖了 55%。这意味着并非每个方法都被覆盖。当你查看行覆盖率时，你会看到只有略多于一半的行被覆盖。

你应该追求多少百分比的测试覆盖率？这取决于许多因素。它应该是你和你的团队感到舒适的百分比，并且能让你很有信心迁移后的功能继续正常工作。这可能是 60%、90% 甚至 100%。重要的是，它应该帮助你在迁移中前进，而不是阻碍你。

### 关于 TDD 的一句话

一章关于测试的内容不可能缺少关于 TDD 的部分。顾名思义，测试代码驱动你生产代码的开发。当你使用 TDD 时，你首先通过例如 JUnit 创建一个测试用例。在 Java 中，它看起来像这样：

```
```java
@Test
void goesToNetherlands_success(){
}
```
```

最后一个示例显示了一个空的测试方法。TDD 的冒险从这里开始。从现在开始，在测试通过之前，你不应该开发任何生产代码。这意味着如果我们想开发新功能，我们首先需要一个失败的或无法编译的测试。

第一步可以是创建你需要的实例。这允许你创建一个类来保存业务逻辑。它可以简单到以下内容：

```
```java
@Test
void goesToNetherlands_success(){

    RoutingHelper routingHelper = new RoutingHelper();
}
```
```

如果你尝试运行/编译项目，你会得到一个异常，说类似类或引用丢失或不存在。那是完全正确的，也是有效的，因为它在项目中还不存在。当你进行 TDD 时，你首先想要一个可以修复的失败测试。这是 TDD 的红绿循环的一部分。你从一个失败的测试开始（红），然后使其通过（绿）。为了让测试工作，你只需要创建一个 RoutingHelper 类并将其作为导入添加到测试类中。

以下是会使测试再次编译的 RoutingHelper 类的示例：

```
```java
public class RoutingHelper {
}
```
```

当你添加了之前的示例类后，你就有一个可工作的（绿）测试。下一步将是再次使测试失败（红）。你可以通过添加一个你希望 `RoutingHelper` 类具有的方法调用来做到这一点。下一个示例展示了如何在测试方法内添加方法调用来实现这一点：

```
```java
@Test
void goesToNetherlands_success() {
    RoutingHelper routingHelper = new RoutingHelper();
    routingHelper.goesToNetherlands();

}
```
```

当你添加方法调用时，测试再次失败，因为该方法还不存在。创建此方法将是下一步。我们刚刚经历了 TDD 的红绿循环。我们从一个失败的测试开始，修复它，使测试通过，然后再次使其失败。这是 TDD 的基础，当你开发新功能时，你希望从一个失败的测试开始并尝试使其通过。这将是新的红绿循环的开始。TDD 可以应用于你编写的任何代码或测试；这可以是单元测试或集成测试。当你对集成测试使用 TDD 时，真的要尽量保持测试执行的速度很快，因为你将多次执行测试。

### 如何创建单元测试

在 Java 中，测试通常通过单元测试完成。当人们谈论测试覆盖率或你的代码是否有测试时，他们最常指的是单元测试。单元测试是一种测试代码最小单元的方法。对于 Java 来说，代码的最小单元通常是一个单一方法。通过单元测试，我们一次只测试一个单一方法。听起来很简单，对吧？如果它不依赖于另一个类、包、数据库，甚至框架或整个应用程序，测试单个方法很容易。那时单元测试往往变得困难，测试代码变得一团糟，没人再想碰它。我们想要防止这种情况，所以我们从基础开始，从一个单一的单元测试开始构建我们的测试策略。

#### 单元测试所需的工具和库

本章中你要编写的单元测试依赖于两个非常流行的测试库，JUnit 和 Mockito。对于单元测试本身，我们将使用 JUnit，并使用 Mockito 来模拟代码所拥有的任何依赖项。这两个库非常有帮助，并且几乎是每个项目都必不可少的。

JUnit 帮助你编写实际的单元测试。借助其内置的注解，它使你能够标记方法并将其连接到测试生命周期的不同部分。测试生命周期帮助你为测试准备对象实例。另一个必不可少的库是 Mockito。Mockito 是一个库，可以以简单直接的方式帮助你创建现有类的模拟。正如你将在后面的示例中看到的，模拟的创建是为了让我们不必与类的真实实例和数据库交互。





#### 创建单元测试

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

### 如何创建集成测试

在本节中，我们将向你展示如何创建集成测试。我们今天构建的应用不再是孤立的。相反，我们在日常生活中开发的应用对内部和/或外部系统都有很多依赖。这些依赖都有各自的版本、接口等，当我们只替换应用的一小部分甚至很大一部分时，这些都需要继续正常工作。重要的是要知道，例如在升级软件依赖、服务器或操作系统后，你的应用是否仍然正常工作。

在下一节中，我们将带你了解集成测试是什么以及如何使用它们来测试你的应用。

#### 什么是集成测试？

要验证你的系统是否仍然正常工作，你需要测试！使用单元测试，我们只测试一小段代码，而不是包、模块，甚至整个应用。它们并不是为此而设计的。要验证更大的部分或整个应用是否正常工作，你需要集成测试。

集成测试顾名思义，是关于测试你的应用与其他应用的集成，以查看被测试的应用在与其他应用交互时是否按预期行为。你也可以用它们来测试应用内部模块之间的集成，因为理想情况下，你希望在真实运行的应用上运行集成测试。在下一节中，我们将向你展示如何针对真实运行的 Cargo Tracker 应用运行测试。为此，我们将使用 Testcontainers。

#### 什么是 Testcontainers？

Testcontainers 是由 AtomicJar 开发的一款软件。它是一个开源框架，为为测试目的启动和拆除 Docker 容器提供抽象。它为你提供了一个简单且轻量级的接口来管理你的容器。还有一个库可以让与 JUnit 5 的集成变得非常简单。使用 Testcontainers，你可以做到以下几点：

- 更轻松地更真实地测试我们的应用。
- 测试你的系统如何与真实依赖集成，而不必使用模拟或存根。例如，你可以启动一个真实的数据库。
- 确保你的测试环境可移植。每个用户都会启动相同的 Docker 容器，使得每个人都很容易开始使用和扩展集成测试。
- 防止开发后期出现错误。你现在可以在部署到开发、测试或验收环境之前测试集成。

#### 设置 Testcontainers

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




下一步是使用 Payara 的 Docker 镜像创建一个容器，我们也可以使用它来部署 .war 文件：

```java
@Testcontainers
public class IntegrationTests {

    static MountableFile warFile = MountableFile.forHostPath(
    Paths.get("target/cargo-tracker.war").toAbsolutePath(), 0777);
    @Container
    static GenericContainer CargoContainer =
    new GenericContainer("payara/server-full:6.2023.2-jdk17")
    .withCopyFileToContainer(warFile,
    "/opt/payara/deployments/cargo-tracker.war")
    .withExposedPorts(8080)
    .waitingFor(Wait.forHttp("/cargo-tracker"));

}
```

使用这个 GenericContainer 类会增加不少代码。所以，让我们一步一步分析发生了什么。GenericContainer 类标有 @Container 注解。这使得 JUnit 负责容器的生命周期管理。在幕后，JUnit 会使用 Testcontainers 来完成这项工作。JUnit 会在每个测试用例的基础上指示 Testcontainers 依赖需要做什么。

将 GenericContainer 声明为 static（静态）是因为我们希望每个测试都使用同一个容器实例。如果不使用 static，那么每个测试都会启动一个新容器，并停止旧容器。这对某些测试可能很方便，但也会耗费一些时间。如果你想让测试跑得更快，最好复用容器。

创建实际实例是代码中有趣的部分。看下面的示例，你会发现这里发生了四件事：

```java
new GenericContainer("payara/server-full:6.2023.2-jdk17")
.withCopyFileToContainer(warFile,
    "/opt/payara/deployments/cargo-tracker.war")
.withExposedPorts(8080)
.waitingFor(Wait.forHttp("/cargo-tracker"));
```

第一行，我们告诉 Testcontainers 我们希望容器基于哪个镜像。有了容器之后，我们希望把 .war 文件复制到容器中，以便它可以被部署。我们不需要调用部署命令，因为 Payara 会自动部署容器 /opt/payara/deployments/ 目录中的任何内容。第 3 行，我们指定要暴露的端口——本例中是 8080。这是默认端口，使我们能够从外部访问容器。

代码的最后一部分非常重要。它告诉我们容器何时可以开始使用。如果没有包含这一行，我们将无法判断容器是否已经就绪。这可能会导致测试运行过早，无法访问 Cargo Tracker 应用程序，从而使测试失败。为了防止测试过早运行，我们可以根据某个条件进行等待。对于 Cargo Tracker 应用程序，我们希望等到 Payara 服务器运行起来，并且 Cargo Tracker 应用程序已经部署完成。要测试 Payara 服务器是否在运行，我们可以向 Cargo Tracker 应用程序的起始页面发起一个 HTTP 请求。如果这个请求成功，我们就知道应用程序已启动并运行，可以准备进行集成测试了。

要检查创建容器和部署 Cargo Tracker 应用程序是否都能正常工作，你可以在类中添加以下测试：

```java
@Test
public void isDeployed() {
    Assertions.assertTrue(CargoContainer.isRunning());
}
```

这个测试会检查容器是否在运行，如果容器在运行，则测试通过。这是一个很好的测试，但它不能告诉你任何关于 Cargo Tracker 应用程序运行情况的有用信息。因此，让我们创建一个真正会测试 Cargo Tracker 应用程序的集成测试。

#### 创建集成测试

在上一节中，我们设置了运行 Cargo Tracker 应用程序的 Docker 容器。这个容器由 JUnit 使用 Testcontainers 进行管理。在上一节中，你只设置了容器，并没有编写真正的集成测试。在本节中，你将构建一个集成测试。我们将向测试容器发起一个 HTTP 请求。

要向容器发起请求，我们将使用 rest-assured 库。rest-assured 是一个提供工具让我们在测试用例中测试和验证 REST 服务的库。要将 rest-assured 依赖添加到项目中，你需要在 Cargo Tracker 应用程序的 pom.xml 文件中加入以下代码片段：

```xml
<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>rest-assured</artifactId>
    <version>5.3.1</version>
    <scope>test</scope>
</dependency>
```

加入这个依赖后，我们就可以从测试中发起 REST 调用了。下一步是构建测试用例。让我们从一个空用例开始，如下所示：

```java
@Test
public void verify() {
}
```

这个测试用例只是一个用 `@Test` 注解标记的方法，该注解告诉 JUnit 5 这个方法包含一个它可以运行的测试。现在，让我们使用 `rest-assured` 向测试容器发起调用。将以下代码添加到测试方法中：

```java
List<LinkedHashMap<String, Object>> cargoResult = given().get(String.format("http://localhost:%d/cargo-tracker/rest/cargo", CargoContainer.getMappedPort(8080)))
    .then()
    .assertThat().statusCode(200)
    .and()
    .contentType(ContentType.JSON)
    .extract()
    .as(List.class);
```

这个方法做了很多事情。让我们逐行看看它做了什么：

-   示例以对 `cargoResult` 的赋值开始。这将是请求正文的引用，我们稍后想用它做进一步测试。
-   赋值之后，你会看到为 Cargo Tracker 应用程序的 `cargo` 端点创建了一个 GET 请求。
-   仔细看，你会发现我们必须使用 `String.format` 创建一个 URL，以调用运行在容器内的 Cargo Tracker 应用程序。这是因为我们不知道哪个外部端口被映射到了 Cargo Tracker 应用程序所监听的内部端口 `8080`。

要找出哪个端口被映射到内部端口 8080，我们必须从上一节中创建的 CargoContainer 实例获取该端口，如下所示：

```java
String.format("http://localhost:%d/cargo-tracker/rest/cargo",
    CargoContainer.getMappedPort(8080))
```

这与上一个示例中的 `String.format` 用法相同。如你所见，我们通过调用 `CargoContainer.getMappedPort(8080)` 方法来获取正确的端口。该方法返回映射到 8080 的端口号。我们必须这样做，因为 Testcontainers 会自动将内部端口 8080 映射到宿主机的一个可用端口。经过 `String.format` 之后，我们就得到了一个可用的 URL，可以用来调用测试容器。

发起调用之后，我们想验证结果是否符合预期。为此，我们可以使用 `.then()` 和 `.assertThat()`，如下例所示：

```java
.then()
    .assertThat().statusCode(200)
    .and()
    .contentType(ContentType.JSON)
```

在上面的代码片段中，我们做了两个断言，如下所示：

-   我们做的第一个断言是检查响应状态码是否为 200，这意味着请求已成功处理。
-   我们做的第二个断言是检查响应内容是否为 JSON。

接下来，我们要获取 Cargo Tracker 应用程序返回的实际响应，即它生成的 JSON。为此，我们可以使用 extract() 方法，它将使用 Jackson 将 JSON 转换为对象。在本例中，我们只需要一个对象列表。

下面的示例展示了如何从 rest-assured 中提取 JSON 响应：

```java
.extract()
.as(List.class);
```

我们可以使用 JSON 响应进行进一步测试。我们可以用它来断言大小，并验证内容是否符合预期。下面的示例演示了具体做法：

```java
Assertions.assertEquals(4, cargoResult.size());

cargoResult.forEach(cargo -> {
    Assertions.assertEquals(cargo.size(), 7);
});
```

完整的集成测试将如下所示：

```java
@Test
public void verify() {

    List<LinkedHashMap<String, Object>> cargoResult = given().get(String.format("http://localhost:%d/cargo-tracker/rest/cargo", CargoContainer.getMappedPort(8080)))
        .then()
        .assertThat().statusCode(200)
        .and()
        .contentType(ContentType.JSON)
        .extract()
        .as(List.class);

    Assertions.assertEquals(4, cargoResult.size());

    cargoResult.forEach(cargo -> {
        Assertions.assertEquals(cargo.size(), 7);
    });
}
```




当此测试运行时，JUnit 将为 Cargo Tracker 应用启动一个容器，并在 Cargo Tracker 应用部署完成且可访问后执行测试。容器就绪后，测试将运行，向 /cargo 端点发出调用，并验证来自 Cargo Tracker 应用的响应。

完成本节及上一节的操作后，完整的测试方法如下例所示：

```java
@Testcontainers
public class IntegrationTests {

    static MountableFile warFile = MountableFile.forHostPath(
Paths.get("target/cargo-tracker.war").toAbsolutePath(), 0777);
    @Container
    static GenericContainer CargoContainer =
    new GenericContainer("payara/server-full:6.2023.2-jdk17")
    .withCopyFileToContainer(warFile,
    "/opt/payara/deployments/cargo-tracker.war")
    .withExposedPorts(8080)
    .waitingFor(Wait.forHttp("/cargo-tracker"));

    @Test
    public void isDeployed(){
    Assertions.assertTrue(CargoContainer.isRunning());
    }

    @Test
    public void verify(){
    List<LinkedHashMap<String, Object>> cargoResult = given().get(String.
    format("http://localhost:${}/cargo-tracker/rest/cargo",
    CargoContainer.getMappedPort(8080)))
    .then()
    .assertThat().statusCode(200)
    .and()
    .contentType(ContentType.JSON)
    .extract()
    .as(List.class);

    Assertions.assertEquals(4, cargoResult.size());

    cargoResult.forEach(cargo -> {
    Assertions.assertEquals(cargo.size(), 7);
    });
    }
}
```

当你将此集成测试添加到 Cargo Tracker 应用后，就可以运行该测试了。结果是，Cargo Tracker 应用的端点将被一个集成测试所覆盖。当 Cargo Tracker 应用的依赖发生变化时，你可以运行此测试来验证端点是否仍按预期工作。

### 小结

在本章中，你学习了为 Cargo Tracker 应用和你自己的项目创建单元测试和集成测试所需的多个库。这是一项重要技能，它能让你对自己的代码库充满信心。创建单元测试和集成测试是专业开发人员日常任务的一部分。

在本章的第一节中，你了解到单元测试是仅覆盖最小代码单元的小型测试。对于 Java 来说，最小代码单元就是单个方法。你从零开始创建了第一个测试，并使用 JUnit 使其运行起来。

在本章的第二部分中，你了解到集成测试覆盖了内部和外部依赖的集成。你使用 Testcontainers 创建了一个集成测试，它利用 Docker 和 Cargo Tracker 应用的镜像来进行集成测试。在设置好 Cargo Tracker 应用的测试容器后，我们向你展示了如何使用 rest-assured 对该实例进行 HTTP 调用测试。

在下一章中，我们将更深入地探讨如何将 Docker 和容器与 Cargo Tracker 应用结合使用。
