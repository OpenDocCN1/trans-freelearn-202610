
![](img/d7c3f2018603810fae83e1a6fd98749e_175_0.png)

之后，选择名为 **新建 JSP 文件 (html)** 的选项，其描述为 **使用 html 标记的 JSP**（如下图所示），然后单击 **完成**：

![](img/d7c3f2018603810fae83e1a6fd98749e_175_1.png)

Eclipse 将生成以下代码。第一行是一个 JSP 指令元素。指令元素指定与页面本身相关的各种属性。因此，它不一定会改变发送到浏览器的响应，但会告知容器应如何处理该页面。第一行是一个页面指令，其中除了其他内容外，还指定了相关页面的内容类型：text/html。JSP 指令元素以 <%@ 开始，以 %> 结束，在这两个分隔符之间，名称/值对的列表代表所谓的指令属性。

除页面指令外，自动生成的代码主要包含 HTML 代码。在 JSP 用语中，一切不是 JSP 元素的内容都称为模板文本。模板文本是静态的，这意味着它会被发送到浏览器而不会经过任何转换。

至于自动生成的代码中的模板文本，存在一个标题占位符，而 body 部分为空。如果你熟悉 HTML 和 Java，编写 JSP 页面对你来说应该很轻松。如果你对 HTML 不太熟悉，创建 JSP 页面起初可能会有点棘手。

```
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html;
charset=ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
</body>
</html>
```
让我们先将标题占位符替换为我们在前一个示例中使用的标题，即 "Warm Greeting"：
<title>Warm Greeting</title>

接下来，让我们添加 Hello, World! 静态消息以及消息的动态部分：时间戳。添加前者很简单，只需复制示例 servlet 中显示的 HTML 代码的一部分即可。要包含后者，我们将在页面中嵌入 Java 表达式，将其放在 <%= 和 %> 之间，如以下代码片段所示：

```
<%@ page language="java" contentType="text/html; charset=ISO-8859-1" pageEncoding="ISO-8859-1"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>Warm Greeting</title>
</head>
<body>
<h1> Hello, World! </h1> <%-- title as template text --%>
<%-- timestamp as embedded expression --%>
<ul> <li>current time: <%= new java.util.Date() %> </li></ul>
</body>
</html>
```

如果你运行前面的示例，你将得到与执行示例 servlet 时完全相同的消息。现在，让我们回顾一下前面代码块中最重要的部分。首先，如你可能注意到的那样，放在 <%-- 和 --%> 之间的所有内容都会被容器忽略。你可以用它来描述代码中正在发生的事情或注释掉代码的一部分。鉴于 JSP 注释也是 JSP 元素，它们永远不会到达浏览器。其次，与其拼接字符串并使用纯 I/O 将其写入响应，前面的代码使用了一个脚本元素，通过它你可以在 JSP 中添加 Java 代码。当你只想添加 Java 代码而不将结果写入响应时，使用 <% 和 %>。当结果需要添加到响应中时，使用 <%= 和 %>。因此，我们使用了后者将时间戳写入响应。下一个示例展示了另一种实现相同功能的方法，但这次使用了更多 JSP 元素和 scriptlet。请注意，页面指令也用于导入 java 类（和包），如以下示例所示。此外，当导入多个类时，完全限定名称的列表必须用逗号分隔。向页面指令添加导入指令后，你就不必在 scriptlet 中使用完全限定的类名。

> 你也可以在 <% 和 %> 对内打印内容。为此，你必须使用 out 对象。该对象始终隐式存在。例如，你可以这样打印 Hello, World! 消息：<% out.println("Hello, World!") %>。不过值得一提的是，在 scriptlet 中使用 out.println() 是一种不良做法。只要可能，请改用表达式（<%= 和 %>）。

```
<%@ page language="java" contentType="text/html; charset=ISO-8859-1" pageEncoding="ISO-8859-1" import="java.util.Date" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>Warm Greeting</title>
</head>
<body>
<h1> <% out.println("Hello, World!"); %> </h1>
<% Date date = new Date(); %>
<ul> <li>current time: <%= date %> </li></ul>
</body>
</html>
```

> 切勿在表达式末尾加上分号。<%= date %> 是正确的，<%= date; %> 则不是。

## 生成 WAR 文件

Java Web 应用程序以 WAR（Web application Archive）文件形式分发。WAR 文件是一个压缩文件，它保留了 Web 应用程序的结构（除了 Web 应用程序上下文目录，即应用程序的顶级目录），并包含一个名为 META-INF 的额外文件夹。这些压缩文件具有 .war 扩展名。META-INF 文件夹包含 WAR 文件中文件相关的元数据。该文件夹中保存的一个重要文件是 MANIFEST.MF 文件。除了其他信息外，此类特殊文件还包含有关打包在 WAR 文件中的文件的信息。

当 WAR 文件被部署时，它们会被容器解包，从而创建以下图所示的结构。顶级目录的名称取决于所使用的容器。例如，Tomcat 会将顶级目录命名为 WAR 文件的名称。因此，例如，Tomcat 在解包 `deployedapp.war` 文件时将创建以下结构。如你所见，WAR 文件的内部结构与 Eclipse 动态 Web 项目的结构几乎相同：

![](img/d7c3f2018603810fae83e1a6fd98749e_179_0.png)

要从项目创建 WAR 文件，请右键单击项目并选择 **导出**。选择如下图所示的 WAR 文件选项，通过 **浏览...** 按钮指定目标目录，然后单击 **完成**。现在你可以通过将其放置在 `webapp` 目录下并重新启动容器，将你的 WAR 文件部署到 Tomcat。

![](img/d7c3f2018603810fae83e1a6fd98749e_179_1.png)

## 总结

在本章中，我们介绍了用于开发 Java Web 应用程序的主要技术概述。我们还强调了这些技术的一些优点。本章还介绍了这些技术的一个重要元素：容器。特别是，本章中使用的容器是 Tomcat。你看到了如何设置 Tomcat，以及如何执行一些基本操作，如启动和停止它；既可以通过命令行，也可以通过 Eclipse。

你了解到，要使 Eclipse 支持 Web 应用程序的开发，你需要安装一组统称为 Eclipse WTP 的插件。事实上，正如你所见，你不需要安装整套插件，本章解释了你必须安装的插件。

在了解了 servlet 的生命周期以及容器在此上下文中的作用后，使用已安装的插件集，我们回顾了基本 servlet 的创建以及一个更复杂的示例。在此过程中，我们解释了 `doGet` 和 `doPost` 之间的区别。因此，你现在知道何时重写它们。使用新 servlet API 的优点之一是使用注解。用 `@WebServlet` 注解 servlet 消除了编辑 `web.xml` 文件的需要。此外，你还了解到 servlet 可以通过 Eclipse 的内部浏览器进行测试：你只需要正确配置 servlet 容器。你还学习了如何使用 Eclipse WTP 创建 JSP。最后，本章展示了如何从 Eclipse Web 项目创建 WAR 文件。从下一章开始，我们将深入探讨更高级的主题。更具体地说，下一章将介绍使用 Eclipse 开发插件。
