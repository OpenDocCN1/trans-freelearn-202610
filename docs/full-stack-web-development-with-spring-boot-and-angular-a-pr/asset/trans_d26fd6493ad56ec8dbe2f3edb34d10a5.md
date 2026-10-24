

## Spring 与 Spring Boot 的区别

那么，Spring 和 Spring Boot 有什么区别？在使用 Spring Boot 之前是否需要先学习 Spring Framework？让我们从第一个问题开始。

下表显示了这两个框架的区别：

| spring | spring boot |
|---|---|
| 开发者为项目配置依赖项。 | 使用 Spring Starters，Spring Boot 将配置运行应用程序所需的所有依赖项。 |
| Spring 是一个用于构建应用程序的 Java EE 框架。 | Spring Boot 通常用于构建 REST API。 |
| Spring 简化了 Java EE 应用程序的开发，因为 Spring JDBC、Spring MVC 和 Spring Security 等模块已提供。 | Spring Boot 提供依赖项的配置，减少了模块布局的样板代码。这使得运行应用程序变得更容易。 |
| 依赖注入（DI）和控制反转（IOC）是 Spring 构建应用程序的主要特性。 | Spring Boot Actuator 是一个功能，它公开了有关应用程序的运行信息，例如指标和流量。 |

我们可以确定 Spring Boot 是构建在 Spring 之上的，主要区别在于 Spring Boot 会自动配置运行 Spring 应用程序所需的依赖项。因此，关于在使用 Spring Boot 之前是否需要学习 Spring Framework 的问题，答案是否定的——Spring Boot 只是 Spring 的扩展，它使配置更快，因为它采用了有主见的方法。

现在，让我们看看在 Spring 和 Spring Boot 中配置 Web 应用程序所需的依赖项。

## Spring 和 Spring Boot 的依赖示例

在 Spring 中，我们的应用程序运行所需的最小依赖项是 Spring Web 和 Spring Web MVC：

```
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.3.5</version>
</dependency>
```

```
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
<version>5.3.5</version>
</dependency>
```

Spring Boot 只需要 spring-boot-starter-web，它是应用程序运行的 Spring Starter。必要的依赖项会在构建时自动添加，因为 starter 将负责配置：

```
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
<version>2.4.4</version>
</dependency>
```

在 Spring 中需要考虑的另一件事是，我们需要定义一些配置，例如调度 servlet 和映射，以便我们的应用程序在服务器上运行：

```
public class SpringInitializer implements WebApplicationInitializer {
@ Override
public void onStartup(ServletContext container) {
AnnotationConfigWebApplicationContext context =
    new AnnotationConfigWebApplicationContext();
context.setConfigLocation("com.springexample");
container.
addListener(new          ContextLoaderListener(context));
ServletRegistration.Dynamic dispatcher =
    container. addServlet("dispatcher",
        new  DispatcherServlet(context));
dispatcher.setLoadOnStartup(1);
dispatcher.addMapping("/");
    }
}
```

初始化调度 servlet 之后，我们还需要使用 @EnableWebMvc 并拥有一个带有 @Configuration 注解的 Configuration 类，在其中我们将为应用程序实例化一个视图解析器。

## 密封类

**密封类**在 Java 16 中处于第二个预览阶段。假设，例如，我们在代码中创建了一个类。我们可以假设该类可以在我们的应用程序中被任何子类扩展。因此，通过使我们的类密封，我们可以确保我们的类不能被子类扩展。如果我们想允许某些子类扩展我们的类，我们可以使用 `permits` 关键字来标识我们想要授予权限的特定类，如以下示例所示：

```
public sealed class Animal permits Cat, Dog, Horse
```

## 外部函数和内存 API

引入了一个新的 API 用于访问和使用 Java 运行时之外的代码，它通过应用外部函数（JVM 之外的代码）并安全地访问外部内存（JVM 不处理的内存）来实现。API 允许 Java 应用程序在没有 **Java Native Interface (JNI)** 的情况下调用本地库。

该 API 旨在用纯 Java 开发模型和更好的性能来取代 JNI，同时访问堆外数据并省略不安全的操作。

## *外部内存*

Java 今天的一个常见问题是访问堆外数据。**堆外数据**是存储在 Java 运行时外部内存中的数据。我们可以说这是一个第三方库。访问此数据对性能至关重要，因为 Java 垃圾收集器仅在堆上数据上工作，这使它们避免了垃圾收集的不可预测性。以下 API 用于处理堆外数据：

+   - **The ByteBuffer API**: 此 API 允许您在堆外数据中创建直接 ByteBuffers，以便数据可以在 Java 运行时之外管理。然而，ByteBuffer 的主要缺点是其最大大小为 2 GB，并且不会及时释放，导致应用程序的运行时性能变慢。
+   - **The Sun.misc.Unsafe API**: Unsafe API 暴露了在堆外数据上工作的访问操作。此 API 使此过程高效，因为 **Just-in-Time (JIT)** 编译器优化了访问操作。然而，不建议使用 Unsafe API，因为我们允许访问任何内存位置。
+   - **The Foreign Function and Memory API**: 此 API 解决了访问内存位置的困境并牺牲运行时性能，因为它提供了类和接口，应用程序可以执行以下操作：
+     - Allocate Foreign Memory
+     - Manipulate and access foreign memory
+     - Call Foreign Functions

## 使用 switch 语句的模式匹配

模式匹配是在 switch 语句中测试模式和复杂表达式的想法。此新功能允许 switch 语句更具扩展性和灵活性，以接受复杂表达式。

## Applet API

**Applet API** 在 Java 中很少使用，因为所有浏览器都已删除对 Java 浏览器插件的支持。

## 实验性 AOT 和 JIT 编译器

实验性的基于 Java 的 **ahead-of-time (AOT)** 和 **JIT** 编译器已被移除，因为其功能使用有限。

这些只是应用于 Java 17 的一些更改。现在，让我们学习 Angular，这是当今顶级的 JavaScript 框架之一，以及使用 Angular 框架开发前端的优势。

## 介绍 Angular

**Angular** 是一个由 Google 维护的免费开源 JavaScript 框架。它主要用于开发 Web 应用程序，并已扩展其功能，可使用插件创建移动和桌面应用程序。Angular 使用基于组件的代码，是渐进式的，并提供许多库和扩展，可缩短开发大型应用程序的时间。

截至撰写本文时，Angular 在构建前端应用程序方面非常流行。它是为三星、Upwork、PayPal 和 Google 等大型知名公司开发应用程序的主要框架。它还拥有非常活跃的社区，在 GitHub 上有 76,000 个星标，约有 1,500 人为该框架做出贡献。此外，它还有数千个功能性 NPM 库，您可以使用它们来加快开发速度。

## Angular 历史

在它成为 Angular 之前，Google 开发的第一个框架是 **AngularJS** 或 Angular 版本 1。尽管开发人员通常对此感到困惑，因为他们认为 AngularJS 和 Angular 相似，但 AngularJS 是由 Google 员工 *Miško Hevery* 作为开源框架发布的，他开发 AngularJS 是为了更快地开发 Web 应用程序。

AngularJS，您可以使用 JavaScript 或 Dart，随着其社区变得更广泛而变得流行。与此同时，Ionic 框架发布了，它允许开发人员使用 AngularJS 构建原生移动应用程序。

