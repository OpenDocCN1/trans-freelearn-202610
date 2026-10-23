
其他API可能会提供注入注解。例如，JPA中的`@PersistenceUnit`注解，它会注入一个`EntityManagerFactory`对象。

## Java服务面（JSF）

JSF是一种基于服务器端组件的用户界面框架。它是用于在浏览器中渲染用户界面的可用技术之一。Java EE中的UI开发经历了许多迭代，从基本的servlet开始，即HTML被写入输出流。然后，加入了Java服务器页面（JSP），它允许业务逻辑和表示逻辑分离。接着，支持自定义标签库和标准标签库集，即Java标准标签库（JSTL）。最后，出现了JSF。

JSF遵循著名的**模型-视图-控制（MVC）**模式。它是一系列UI小部件和控件的集合。由于它是一个规范，随着时间的推移出现了许多竞争性实现。著名的例子有IceFaces、OpenFaces和MyFaces。

工作流程如下：你有一个网页，在上面添加表示UI元素的JSF自定义标签。每个页面都有一个或多个所谓的支持bean。支持bean由容器运行时管理，它们用于保存UI元素的属性和方法。此外，它们还可以保存事件监听器。

作为一个简单示例，请看下面的JSF代码，它取自Jakarta EE教程的Hello World示例：

```
<html lang="en"
    <h:body>
        <h:form>
            <h:graphicImage url="#{resource['images:duke.waving.gif']}" alt="Duke waving his hand"/>
            <h2>Hello, my name is Duke. What's yours?</h2>
            <h:inputText id="username"
                title="My name is: "
                value="#{hello.name}"
                required="true"
                requiredMessage="Error: A name is required."
                maxlength="25" />
            <p></p>
            <h:commandButton id="submit" value="Submit" action="response">
            </h:commandButton>
            <h:commandButton id="reset" value="Reset" type="reset">
            </h:commandButton>
        </h:form>
        ...
    </h:body>
</html>
```

这里，`<h:inputText>`元素定义了一个文本输入字段，而`<h:commandButton>`定义了一个可点击的按钮。所有这些元素都以`h`为前缀，`h`是JSF命名空间的缩写。

以下是该JSF页面的支持bean的代码：

```
```java
@Named
@RequestScoped
public class Hello {
    private String name;
    public Hello() {
    }
    public String getName() {
        return name;
    }
    public void setName(String user_name) {
        this.name = user_name;
    }
}
```
```

`inputText`字段通过`value="#{hello.name}"`声明与支持bean的`name`属性相连，其中`hello`是bean类的名称，以小写字母开头，`name`与支持bean中的`name`属性匹配。

Cargo Tracker应用对所有UI元素使用JSF。如果你想浏览JSF页面的源代码，请转到`src/main/webapp`。

## JAX-RS

很长一段时间里，SOAP在Web服务领域占据主导地位。Java通过JAX-WS（Java API for XML Web服务）支持SOAP。但新世纪之初，一种新型Web服务进入了舞台。Roy Fielding撰写了一篇关于这一新概念的论文，并将其命名为**表述性状态转移**（Representation State Transfer），简称REST。

RESTful Web服务，即遵循REST原则的Web服务，是一种以可被其他机器或服务访问的方式暴露机器上资源的方法。SOAP Web服务对开发者施加严格规则，而RESTful Web服务可以用多种编程语言开发。与SOAP只允许XML作为其内容相比，RESTful服务支持多种数据格式。重要的是要理解，REST使用标准HTTP动词（GET、POST、PUT、PATCH、DELETE和HEAD）来执行其操作。

REST变得极其流行，因为它允许组织以非常简单、易于理解的方式与外界共享其数据和信息。2002年，亚马逊创始人杰夫·贝佐斯写了一份备忘录，他在其中要求“_所有团队都应通过服务接口暴露其数据和功能，所有团队必须通过这些服务接口相互通信，_”

*并且最终不允许任何其他形式的相互通信。* 虽然亚马逊并不是第一家采用REST的大型公司，但以这种严格的方式采用REST为它的发展铺平了道路。

下图显示了自2005年以来公共API的惊人增长：

![](img/8948f94d0abcdb1ae94d6778da518d16_45_0.png)

图2.13 – 公共API可用性的上升

从最初的那些日子起，基于REST的公共API数量稳步增长，事实上，可以稳妥地说REST现在是最常见的Web服务类型。

## JSON绑定

JSON绑定是一种API规范，用于将Java对象序列化为JSON，反之亦然。JSON是一种简单的数据表示格式。它经常被用来代替XML，因为它不那么冗长。默认情况下，它支持Java默认的简单数据类型及其对应的对象类型。除此之外，还支持`java.util`包中几个与日期/时间相关的类，以及`java.time`包中的大多数类。许多集合接口（如`List`、`Map`和`Set`）及其在`java.util`包中的实现的（反）序列化也开箱即用地支持。

将来自RESTful调用的JSON负载绑定到其对应的对象，大部分由Java EE REST API运行时自动完成。同样，将RESTful调用的结果对象转换为JSON，也由Java EE REST API运行时自动处理。

尽管如此，在某些情况下，你可能希望自己在代码中执行这些转换——例如，通过使用众多转换选项中的任何一种来对转换过程进行更多控制。

默认情况下，映射是通过将JSON属性名与Java实例变量名匹配来完成的。以下JSON中的JSON `firstname`属性将映射到Java Person类中的Java `firstname`实例变量。

（反）序列化过程可以通过在类级别、实例变量级别或实例方法级别添加注解来影响。由于`@JsonProperty("familyname")`注解覆盖了默认映射，JSON `familyname`属性将映射到Java `name`实例变量：

```
```json
{
  "familyname": "Mc Nealy",
  "firstname": "Scott"
}
```

```java
public class Person {
    @JsonProperty("familyname")
    private String name;
    private String firstname;
}
```
```

Cargo Tracker应用不使用注解或自动（反）序列化。相反，它使用编程式转换，如`org.eclipse.cargotracker.interfaces.booking.rest`包中的`CargoMonitoringService`类所示。

## 事务

业务数据的基本特性之一是完整性。如果这种完整性丧失，任何基于这些数据的决策就会变得如同行走在流沙之上。事务有助于维护数据完整性。

它通过两种方式做到这一点：首先，通过控制对数据的并发访问；其次，在系统故障时，将数据恢复到先前状态。事务的结果只能是两种可能之一：要么所执行的操作成功完成并执行提交（commit），要么发生了某种错误并执行回滚（rollback）。提交将持久化操作的结果——例如，将更新后的值存储到数据库中。而回滚则会恢复系统到开始事务之前的状态。

Java EE支持两种类型的事务管理：**容器管理事务（CMTs）**和**Bean管理事务（BMTs）**。如果EJB上未指定任何内容，则CMT是默认机制。

