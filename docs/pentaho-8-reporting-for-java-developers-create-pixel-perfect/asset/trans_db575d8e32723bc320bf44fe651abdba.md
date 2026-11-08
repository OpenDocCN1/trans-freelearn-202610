
## 使用 Java 测试本地化

如你在上一节中所读到的，ResourceBundle 的控制是通过 locale 完成的。默认情况下使用系统区域设置，可以通过以下源代码显式指定：

```java
Locale locale = new Locale();
```

如果你想控制（并更改）默认区域设置，可以使用以下源代码设置其值（示例展示如何设置西班牙语）：

```java
Locale locale = new Locale("es");
Locale.setDefault(locale);
```

这段小代码在测试期间非常有用，如果你想以编程方式控制 Pentaho 报表 的区域设置。在下面的示例中，你将看到如何从默认区域设置切换到自定义区域设置以测试最终版本。

## 使用 Java 进行报表本地化的示例

为了在实践中展示使用 Java 进行报表本地化的示例，让我们使用 https://github.com/fcorti/pentaho-8-reporting-for-java-developers 仓库；具体来说，项目 `pentaho-reporting-localization`，存储在 `Chapter 13 - Internationalization, sub-reports and cross-tabs using Java` 文件夹中。

在查看源代码之前，让我们先检查 `resources` 文件夹的内容，其中可以找到两个属性文件：

- `my-first-reporting-project_en.properties`: 包含英语语言的资源键
- `my-first-reporting-project_es.properties`: 包含西班牙语的资源键

你已经学习了 *i18n* 表示的基础知识，并且已经在第 9 章 *Internationalization and Localization* 的示例中使用了这些属性。要将属性文件包含在项目的分发包中，请向 `pom.xml` 文件添加以下 `resources` 标签：

```xml
...
<build>
  <finalName>pentaho-reporting-web-app</finalName>
  <resources>
    <resource>
      <directory>resources</directory>
    </resource>
  </resources>
  <plugins>
    ...
  </plugins>
</build>
```

现在让我们进入众所周知的 `PentahoServlet` 类，其中可以找到以下源代码：

```java
...
// Defining the default locale.
Locale locale = new Locale("es");
Locale.setDefault(locale);
```

```java
// Adding a resource label to the report header band.
ResourceLabelElementFactory labelFactory = new ResourceLabelElementFactory();
labelFactory.setResourceKey("ORDERNUMBER");
labelFactory.setResourceBase("my-first-reporting-project");
...
Element labelField = labelFactory.createElement();
reportHeader.addElement(labelField);
...
```

如你所见，第一个任务是设置西班牙语为默认区域设置。这是通过使用 `Locale locale = new Locale("es")` 然后将 `locale` 变量用作默认值来完成的。完成后，`ResourceLabelElementFactory` 类被声明以创建 `resource-label` 元素。`setResourceKey` 方法使你能够指向属性文件中的正确描述，而 `setResourceBase` 方法使你能够指向正确的属性文件。其余所有方法都与之前示例中使用的类似。

要运行项目，请打开终端，进入项目文件夹，并按以下顺序执行命令：

```
mvn clean install
mvn package
java -jar target/dependency/jetty-runner.jar target/*.war
```

在浏览器中打开 http://localhost:8080，你应该可以看到 Pentaho 报表，如以下屏幕截图所示：

![](img/9337ac94537bf7e092e1ea6531dce8fd_602_0.png)

要更改默认语言，如本章前面所述，将区域设置更改为 `Locale locale = new Locale("en")`。这样，你将要求项目使用英语区域设置作为默认值。通过再次编译项目并重复上述命令，你将获得以下预览，其中标签仅通过更改区域设置而更改：

![](img/9337ac94537bf7e092e1ea6531dce8fd_603_0.png)

## 使用 Java 的子报表

如你在第 10 章 *Subreports and Cross Tabs* 中所学，Pentaho Reporting 中的子报表允许你在主报表中包含功能齐全的报表。在本节中，特别是从 Java 角度来看，你将达到相同的专业水平。在以下各节中，你将学习 Pentaho 子报表的基础知识，然后继续了解 GitHub 仓库中可用的完整工作源代码示例。

## 使用 Java 的子报表

为了了解使用 Java 开发子报表，让我们描述用于其定义的各种组件。首先是 `SubReport` 类，用于在 `MasterReport` 中声明和管理子报表实例。与常规元素不同，子报表是使用 `addSubReport(SubReport subReport)` 方法添加到报表频带中的。

如你在第 10 章 *Subreports and Cross Tabs* 中所见，子报表在内容定义时始终使用不同的数据源。使用 Java，`MasterReport` 中声明的所有查询默认由子报表继承，但需要使用 `setQuery` 方法为 `MasterReport` 和 `SubReport` 指定查询。

如常发生的情况，子报表可能会使用来自 `MasterReport` 的一个或多个参数来指导正确的渲染。为了指定输入参数，`SubReport` 类提供了 `addInputParameter(String outerName, String sourceColumn)` 方法。

除了前面讨论的话题外，`SubReport` 的使用与常规 `MasterReport` 类似，你可以在其中添加元素、自定义布局等。

## 使用 Java 的子报表示例

为了在实践中展示使用 Java 的子报表示例，让我们使用 https://github.com/fcorti/pentaho-8-reporting-for-java-developers 仓库；具体来说，项目 `pentaho-reporting-subreport`，存储在 `Chapter 13 - Internationalization, sub-reports and cross-tabs using Java` 文件夹中。

如 `PentahoServlet` 类所示，`DataFactory` 使用以下代码片段声明 `default` 查询和 `default2` 查询：

```java
// Defining the queries.
SQLReportDataFactory dataFactory = new SQLReportDataFactory(provider);
dataFactory.setQuery(
"default",
"SELECT ORDERNUMBER, ORDERDATE FROM ORDERS");
dataFactory.setQuery(
"default2",
"SELECT PRODUCTCODE, QUANTITYORDERED, PRICEEACH FROM ORDERDETAILS " +
"WHERE ORDERNUMBER=${ORDERNUMBER} ORDER BY ORDERLINENUMBER ASC LIMIT 5");
report.setDataFactory(dataFactory);
```

![](img/9337ac94537bf7e092e1ea6531dce8fd_606_0.png)

> *请非常注意为 `DataFactory` 中声明的查询提供唯一名称。如果不这样做，你的应用程序的行为将不可预测，并且你将浪费大量时间痛苦地理解问题究竟出在哪里。*

如本章前面所述，`MasterReport` 和 `SubReport` 使用以下代码声明它们将要使用的查询：

```java
...
report.setQuery("default");
...
subReport.setQuery("default2");
...
```

在下面的 Java 代码片段中，你可以找到 `SubReport` 对象的声明、向子报表频带添加元素的示例，以及将子报表包含在主报表中的示例：

```java
...
// Creating the subreport.
SubReport subReport = new SubReport();
subReport.setQuery("default2");
subReport.addInputParameter("ORDERNUMBER", "ORDERNUMBER");
...
// Adding a field to the subreport's details band.
TextFieldElementFactory textFactory2 = new TextFieldElementFactory();
...
Element textField2 = textFactory2.createElement();
subReport.getItemBand().addElement(textField2);
...
// Adding the subreport to the report's details band.
itemBand.addSubReport(subReport);
...
```

要运行项目，请打开终端，进入项目文件夹，并按以下顺序执行命令：

```
mvn clean install
mvn package
java -jar target/dependency/jetty-runner.jar target/*.war
```

在浏览器中打开 http://localhost:8080，你应该可以看到 Pentaho 报表，如以下屏幕截图所示：

![](img/9337ac94537bf7e092e1ea6531dce8fd_607_0.png)

![](img/9337ac94537bf7e092e1ea6531dce8fd_607_1.png)

如你所见，结果的 Pentaho 报表与我们在第 10 章 Subreports and Cross Tabs 中使用 Pentaho Report Designer 开发的报表完全相同。换句话说，在这里你达到了对 Pentaho Report Designer 的相同专业水平，但使用的是你的 Java
