
| | |
|---|---|
| layout: watermark | 报告中的其他频段。此元素包含一个名为 `root-level-content` 的子元素，其中包含对子元素的全部引用。请注意，`watermark` 是一个报表元素，因此它可能包含前面定义的一般元素属性和子元素。 |
| layout:page-header | `page-header` XML 元素定义了每个页面的页眉内容。此元素包含一个名为 `root-level-content` 的子元素，其中包含对子元素的全部引用。请注意，`page-header` 是一个报表元素，因此它可能包含前面定义的一般元素属性和子元素。 |
| layout:page-footer | `page-footer` XML 元素定义了每个页面的页脚内容。此元素包含一个名为 `root-level-content` 的子元素，其中包含对子元素的全部引用。请注意，`page-footer` 是一个报表元素，因此它可能包含前面定义的一般元素属性和子元素。 |

## 手动创建 .prpt 捆绑文件

现在您已经了解了构成 `.prpt` 捆绑文件的所有不同文件，让我们看看如何手动创建它。不要在这里列出组成 `.prpt` 捆绑文件所需的所有强制性文件内容，让我们使用 https://github.com/fcorti/pentaho-8-reporting-for-java-developers 仓库；具体来说，让我们使用存储在 `Chapter 11 - The PRPT format and the Java API to build it/my_reports/uncompressed_prpt.` 中的文件夹。

请花时间检查文件夹结构及其包含的每个文件。它们共同定义了一个非常简单的报表，其中包含 JDBC 数据源和少量元素。

一旦确定了文件夹结构，请选择其中包含的所有文件并根据您的操作系统对其进行压缩（使用 `.zip` 格式）。将生成的 file 保存为 `example_xml.prpt`。现在，在 Pentaho Report Designer 中打开 `example_xml.prpt` 文件并单击眼睛图标（👁）以预览它。您应该会看到一个如下所示的报表：

|  |  |  |
|---|---|---|
| 10,100 | Mon Jan 06 00:00:00 CET 2003 | Shipped |
| 10,101 | Thu Jan 09 00:00:00 CET 2003 | Shipped |
| 10,102 | Fri Jan 10 00:00:00 CET 2003 | Shipped |
| 10,103 | Wed Jan 29 00:00:00 CET 2003 | Shipped |
| 10,104 | Fri Jan 31 00:00:00 CET 2003 | Shipped |
| 10,105 | Tue Feb 11 00:00:00 CET 2003 | Shipped |
| 10,106 | Mon Feb 17 00:00:00 CET 2003 | Shipped |
| 10,107 | Mon Feb 24 00:00:00 CET 2003 | Shipped |
| 10,108 | Mon Mar 03 00:00:00 CET 2003 | Shipped |
| 10,109 | Mon Mar 10 00:00:00 CET 2003 | Shipped |
| 10,110 | Tue Mar 18 00:00:00 CET 2003 | Shipped |
| 10,111 | Tue Mar 25 00:00:00 CET 2003 | Shipped |
| 10,112 | Mon Mar 24 00:00:00 CET 2003 | Shipped |

![](img/9337ac94537bf7e092e1ea6531dce8fd_537_0.png)

在 https://github.com/fcorti/pentaho-8-reporting-for-java-developers 仓库中，在路径 `Chapter 11 - The PRPT format and the Java API to build it/my_reports` 中，您可以找到 `example_xml.prpt`，它完全实现了这里描述的示例。

## 使用 Pentaho Reporting Java API 构建报表

现在您已经使用 Pentaho Reporting XML 格式构建了报表，是时候学习如何通过使用 Pentaho Reporting Java API 构建报表来进行类似练习了。为了避免遍历 Pentaho Reporting 的整个 Javadoc，本章仅涵盖要点。其中包括对重要包的引用，以便更轻松地找到构建报表所需的类、工厂和接口。Pentaho Reporting 的 Javadoc 可在 http://javadoc.pentaho.com/reporting/ 获取。

使用 Pentaho Reporting API 的第一步是初始化报表引擎并创建一个空的 MasterReport 对象：

```
// Initialize the reporting engine
ClassicEngineBoot.getInstance().start();

// Create a report object
MasterReport report = new MasterReport();
```

ClassicEngineBoot 和 MasterReport 类位于 org.pentaho.reporting.engine.classic.core Java 包中。

一旦创建了报表对象，您就可以逐步处理构成整个报表的各个组件。以下各节演示如何设置数据源、向报表添加参数、包含函数、向报表添加布局频段、向报表添加元素（您将学习如何添加标签和图表作为示例）以及自定义报表页面定义。

```
// Define a simple TableModel.
DefaultTableModel tableModel = new DefaultTableModel (
 new Object[][]
 {
 {"Line One", "Product One", 10, 100},
 {"Line Two", "Product Two", 20, 200},
 ...
 },
 new String[] {"Line", "Product", "Cost", "Quantity"}
);

// Create a TableDataFactory.
final TableDataFactory dataFactory = new TableDataFactory();
dataFactory.addTable("default", tableModel);

// Add the factory to the report.
report.setDataFactory(dataFactory);
```

```
// Define a plain parameter.
PlainParameter entry = new PlainParameter("Product", String.class);

// Make the parameter Mandatory.
entry.setMandatory(true);

// Add the parameter to our parameter definition.
paramDef.addParameterDefinition(entry);
```

```
// Create a formula expression.
FormulaExpression formula = new FormulaExpression();

// Configure the formulas properties.
formula.setName("totalCost");

formula.setFormula("=[Cost]*[Quantity]");

// Add the expression to the report.
report.addExpression(formula);
```

## 定义报表布局

与本章前面描述的 `layout.xml` 文档类似，定义构成报表布局的 Java Bean 包括报表页眉和页脚等通用报表频段，以及还包括明细频段的组频段层次结构。

### 通用报表频段

对于报表中定义的每个频段，例如报表页眉、报表页脚、页面页眉和页面页脚，都有等效的 Java 类可用，所有这些类都扩展自位于 `org.pentaho.reporting.engine.classic.core` 包中的通用抽象类 `Band`。这些频段都位于同一个包中。

每个频段都定义了一组 Java Bean 属性，但大多数只是使用在 `Band` 抽象类中定义的属性。该类定义的主要方法之一是 `addElement(Element element)`，您将在创建元素并将其添加到其父频段时稍后使用。`Band` 实现的子集还实现了 `RootLevelBand` 接口，该接口允许通过 `addSubReport(SubReport subreport)` 方法向频段添加子报表。诸如页面页眉、页面页脚和水印之类的频段不允许渲染子报表。以下是设置报表中报表页眉和报表页脚的示例：

```
// Creating report header and footer objects.
ReportHeader reportHeader = new ReportHeader();
ReportFooter reportFooter = new ReportFooter();
```

```
// Adding the report header and footer to the report.
report.setReportHeader(reportHeader);
report.setReportFooter(reportFooter);
```

这已经不能更简单了！

### 组频段层次结构

组频段也遵循这种简单的 Java Bean 模式。由于可以嵌套组，因此 API 稍微复杂一些。RelationalGroup 类用于常规的关系分组。CrosstabGroup 用于在报表中定义交叉表。CrosstabGroup API 超出了本章的范围，但在 Javadoc 中有很好的文档记录。

RelationalGroup 类可能包含组页眉和组页脚，以及 GroupBody 实现。GroupBody 是一个抽象类，SubGroupBody 和 GroupDataBody 扩展了它。SubGroupBody 允许嵌套组，而 GroupDataBody 包含 ItemBand 以及另外三个使用较少的频段，DetailHeader、DetailsFooter 和 NoDataBand。

要配置 RelationalGroup 类，您必须提供一组定义分组的数据行字段。这可以通过 addField(String field)、setFields(List fields) 或 setFieldsArray(String[] fieldsArray) 方法调用来完成。在此示例中，您将定义 RelationalGroup 和 ItemBand，并按 Name 字段分组。请注意，ItemBand 类代表报表中的明细频段。

```
// Creating the root relational group.
RelationalGroup group = new RelationalGroup();

// Using "Line" as the root grouping.
group.addField("Line");

// Creating a group data body.
GroupDataBody groupData = new GroupDataBody();

// Creating an item band.
ItemBand itemBand = new ItemBand();

// Place the item band within the group data body.
groupData.setItemBand(itemBand);

// Place the group data body within the root group:
group.setBody(groupData);

// Setting the root group within the report.
report.setRootGroup(group);
```
