

最后，在 Jakarta EE 10 中，多部分表单参数开箱即用地得到支持：

```
@POST
@Path("upload/picture")
@Consumes(MediaType.MULTIPART_FORM_DATA)
public Response postImage(@FormParam("userId") String memberId, @FormParam("image") EntityPart image) {
    String fileName = image.getFileName().orElseThrow(NotSupportedException::new);
    InputStream content = image.getContent();
    return Response.ok("image is uploaded").build();
}
```

我们需要定义该端点消费 MULTIPART_FORM_DATA。接下来，我们需要定义要注入到变量中的各个表单参数的内容。在前面的代码中，EntityPart 将保存包含图像数据的流。尽管这里可以使用字符串，但更明智且更具可扩展性的解决方案是使用输入流来读取数据，以防止图像内容一次性加载到方法中。

## 纯 Java Jakarta Faces 视图

没错，你读对了！如果你想以编程方式组合你的 Jakarta Faces 视图，现在你可以了。你可以跳过所有脚本部分，只使用 Java 代码。

这样做有各种优势。在动态构建视图时，你只需遍历 Java 类并为每个字段按需创建组件。如果你想从视图中排除某些字段，可以在 Java 中编写执行此操作的条件。最后，事件处理程序可以是简单的 Java Lambda——无需通过 EL 表达式使用方法引用。

如果你以前使用 Java SE 编写过 Swing 代码，那么下面的示例可能看起来非常熟悉：

```
HtmlForm form = components.create(HtmlForm.COMPONENT_TYPE);
body.getChildren().add(form);

HtmlOutputText message = components.create(HtmlOutputText.COMPONENT_TYPE);
form.getChildren().add(message);

HtmlCommandButton actionButton = components.create(HtmlCommandButton.COMPONENT_TYPE);
actionButton.addActionListener(
    e -> message.setValue("I was pressed"));
actionButton.setValue("Press");
form.getChildren().add(actionButton);
```

在这里，我们在代码中创建了一个简单的 HTML 表单。我们向表单添加了一个 HtmlOutputText 组件。最后，我们创建并添加了一个 HtmlCommandButton 组件。按钮的 action listener 方法接收一个 Lambda，当按钮被按下时会执行该 Lambda。

## 使用 OpenID 进行身份验证

你在应用程序中如何处理用户身份验证？最常见的方式是提供一个专用的登录，使用用户名和密码。一旦输入，你的应用程序就会尝试将用户与本地数据库或 LDAP 中可用的信息进行身份验证。

然而，用户并不总是希望为每个想要访问的新站点创建新账户。这就是 OpenID 的用武之地。想象一下，你有一个 Google 账户。访问站点 A 时，你指定想要使用你的 Google 凭据登录。站点 A 本身没有任何了解或验证你的 Google 凭据的能力。因此，它将身份验证中继到 google.com。一旦 Google 确定你声称自己是谁，此信息就会返回给站点 A。

随着使用 OpenID 进行身份验证变得越来越普遍，Jakarta Security 现在通过一组注解支持 OpenID。让我们看一个示例：

```
@OpenIdAuthenticationDefinition(
providerURI = "https://accounts.google.com",
clientId = "${config.clientId}",
clientSecret = "${config.clientSecret}",
redirectURI = "${baseURL}/callback",
)
@ApplicationScoped
public class GoogleOpenIdSecurityBean {
}
```

@OpenIdAuthenticationDefinition 接受多个可配置参数。providerUri 指向 OpenId 提供者——即执行实际身份验证的参与者。clientId 和 clientSecret 告知提供者它正在为谁进行身份验证。最后，redirectUri 包含提供者在身份验证完成后返回到你的应用程序的回调端点。

## 改进的并发

Jakarta concurrency 在版本 10 中有大量更改。其中两个最值得注意的是 @Asynchronous 注解的引入以及能够配置 ManagedExecutorService。

虽然该注解可通过 Jakarta Enterprise Beans 规范获得，但新注解有几个明显的优势。首先，EJB 需要完整的 Jakarta EE 配置文件才能工作——例如在 Core Profile 中不可用。

其次，旧注解使用来自应用程序服务器的默认线程池，而新版本允许你指定要使用的线程池，如果你不想使用默认线程池的话。这可能带来巨大的好处，因为它允许你为某些任务组使用专用线程池。

这里有一个示例：

```
```java
public class PaymentService {
    @Asynchronous(executor = "java:app/concurrent/OrderExecutorService")
    public CompletableFuture<OrderStatus> processOrder
        (final Order order) {
        try {
            var status = processAvailability(order);
            return CompletableFuture.completedFuture(status);
        } catch (OrderProcessingException ex) {
            throw new CompletionException(ex);
        }
    }
}
```
```

我们只需用 @Synchronous 注解该方法，并可选择指定要使用的执行器服务。

## 添加第一个云原生特性——弹性

从技术角度来看，为你的系统添加弹性并不困难。真正的挑战在于决定在哪里为代码添加弹性以及添加哪种类型的弹性。为了为代码添加弹性，我们将使用 MicroProfile，因为它非常适合 Jakarta EE 应用程序。

你可以通过向 pom.xml 文件添加以下条目来添加 MicroProfile：

```
```xml
<dependency>
    <groupId>org.eclipse.microprofile</groupId>
    <artifactId>microprofile</artifactId>
    <version>5.0</version>
    <type>pom</type>
    <scope>provided</scope>
</dependency>
```
```

通过将此 xml 添加到你的 pom.xml 文件中，你就可以在应用程序中使用 MicroProfile。

接下来，执行构建和启动应用程序的标准步骤。它们现在应该不再让你感到惊讶了。

对于 CargoTracker 应用程序，你需要运行以下命令：

- `mvn clean install`
- `mvn cargo:run`

在应用程序中可以使用 MicroProfile 后，我们可以使用以下选项为应用程序添加弹性：

- `@Timeout`
- `@Retry`
- `@Fallback`
- `@Bulkhead`
- `@CircuitBreaker`
- `@Asynchronous`

如前所述，@Asynchronous 是一个特例，因此此处不予处理。我们将在此处提供其他弹性方法的示例。

@Timeout 是一种弹性方法，我们希望将其添加到对可能花费很长时间的操作或其他系统的调用中。为了防止系统变得无响应，我们可以限制操作运行的时间，在此之后将其切断。

在我们的代码中使用 @Timeout 选项的理想位置是图遍历代码。它位于 ExternalRoutingService 类中。在那里，fetchRoutesForSpecification 方法执行 web 客户端调用以检索路由规范。

从技术上讲，外部系统可能响应缓慢，甚至更糟，无法使用。

为了限制这对我们系统的影响，我们可以添加 @Timeout 注解。代码现在看起来像这样：

```
```java
@Timeout(value = 2000)
public List<Itinerary> fetchRoutesForSpecification(RouteSpecification
routeSpecification) {
```
```

如果对外部系统的调用失败，现在由于你添加的注解，它会尽早失败。但如果它失败了，你难道不想重试吗？毕竟，这可能只是导致此问题的简单网络故障。它可能是一个可恢复的错误。

为了支持重试，我们需要添加以下代码。它将使我们在重试之间等待 250 毫秒，并且它只会重试两次，而不是默认的 3 次：

```
```java
@Timeout(value = 2000)
@Retry(maxRetries = 2, delay = 250)
public List<Itinerary> fetchRoutesForSpecification(RouteSpecification
routeSpecification) {
```

