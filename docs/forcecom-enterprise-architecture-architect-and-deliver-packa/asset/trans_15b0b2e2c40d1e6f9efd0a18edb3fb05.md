
配置集和权限集也引用 Visualforce 页面；用户必须先获得页面访问权限，然后才能从这些区域使用它。

## 闪电组件

以下是接受闪电组件的平台区域列表：

- 记录页面
- 主页
- 闪电选项卡和页面
- 闪电组件操作
- Lightning Experience 工具栏
- Lightning 应用生成器
- Lightning 社区和社区生成器
- 面向 Visualforce 的 Lightning Out
- 面向任何应用的 Lightning Out

在第 6 章"应用程序域层"中，我们探索了将验证合规性闪电组件添加到记录页面。在下一章中，我们将构建一些新的闪电组件，以便更仔细地查看前面列表中的其他区域。您还应参阅 Lightning 开发者指南以获取完整详情。

## 生成可下载内容

contentType 属性允许您控制浏览器如何解释页面输出。使用该属性，例如，可以输出 CSV、JSON 或 XML 内容。使用控制器绑定，这可以是动态生成的输出。这对于生成要下载的内容非常有用。对 FormulaForce 应用程序所做的以下更改已包含在本章代码中：

- 向 RaceService 添加了新方法 generateSummaryAsCSV
- 向 RaceSummaryController 添加了新方法 getCSVContent
- 添加了新的 racesummary.aspx Visualforce 页面
- 向 Race 布局添加了新的自定义按钮"下载摘要"

新方法已添加到现有的 RaceSummaryController 类中，但仅由新页面使用。请随时查看新的服务方法；请注意，它还使用了前一章中引入的 ContestantsSelector.selectByRaceIdWithContestantSummary 方法。以下是调用该方法的新控制器方法：

```
public String getCSVContent() {
    return RaceService.generateSummaryAsCSV(
        new Set<Id> { standardController.getId() } );
}
```

> RaceService 类包含生成 CSV 文件的逻辑，而非控制器类。虽然乍一看您可能会认为这是控制器逻辑，但若从通用数据导出的角度考虑，未来也可能存在供外部开发者用于自动化 CSV 导出流程的 API 需求；因此，将此逻辑放置在服务层中有助于实现此目标。

新的 Visualforce 页面使用 contentType 属性，如下所示：

```
<apex:page
    standardController="Race__c"
    extensions="RaceSummaryController"
    contentType="application/csv#{!Race__c.Name} Race Summary">{!CSVContent}
</apex:page>
```

由于此页面使用标准控制器，因此可以创建自定义按钮；单击后，页面不会打开，浏览器会提示您下载文件：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_285_0.png)

按下"下载摘要"按钮会出现以下提示，以下载生成的 CSV 文件：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_285_1.png)

> 您可能注意到，在前面的 Visualforce 代码中，apex:page 的 contentType 属性包含 "#" 和一些文本。在内容类型后放置 # 符号后，您可以在文件下载时为用户指定默认文件名。还要注意，Visualforce 公式也可用于属性值，Race 名称正是以这种方式出现在文件名中。如果需要，这还允许您动态更改 contentType。

> 虽然此示例很好地说明了 contentType 属性，但应注意，平台报表引擎也支持 CSV 导出。因此，如果您正在考虑 CSV 导出功能，值得首先考虑让用户使用报表；这样，他们对输出拥有更多控制自由。

## 生成可打印内容

Salesforce 拥有内置的 PDF 生成引擎，可将您的 HTML 标记转换为 PDF。这是一项非常有用的功能，可用于生成发票或采购订单等更正式的文档。

您可以在 Visualforce 页面的 apex:page 元素上使用 renderAs 属性访问该功能，并将其设置为 pdf。请注意，通常应为此目的专门设置一个特定的 Visualforce 页面，而不是尝试在用于其他目的的页面上使用此属性。

> 请尽可能多地使用原生 HTML 和 CSS；Visualforce 标准组件在此模式下不一定能呈现良好。出于此原因，使用 standardStylesheets 属性禁用 Salesforce CSS 也很有用。

您还可以通过使用 PageReference.getContentAsPDF 方法并将生成的 PDF 附加到记录中以供将来使用，以编程方式访问此功能。如果您希望不使用 Visualforce 页面来生成 PDF 内容，可以以编程方式生成 HTML 并利用 Blob.toPDF 方法。第 2 章"利用平台功能"中已介绍翻译和本地化主题，因此我们不会在此处详细回顾。只需再次强调，从一开始就考虑翻译和本地化要求非常重要，因为事后添加这些功能可能代价高昂。另外，请注意，在前面的示例中，Visualforce 页面中使用了 $ObjectType 全局变量来访问列标题的 **自定义字段** 标签。确保您利用此类方法将减少需要创建的 **自定义标签** 数量，并最终降低翻译成本。

> > 闪电组件目前不支持 $ObjectType；作为替代方法，您可以使用 Apex Describe 通过 Apex 控制器公开此内容。

在 Visualforce 中，您可以使用 `apex:inputField` 和 `apex:outputField` 组件来呈现和接收用户输入，这些组件对区域设置敏感。在 Lightning 中，请使用 `lightning:formattedNumber` 和 `lightning:formattedDateTime` 组件。

## 替代页面语言

默认情况下，Visualforce 页面将通过用户的区域设置查找您在页面或 Apex 代码中引用的自定义字段标签和自定义标签的翻译文本。但是，如果您希望替代此设置，可以指定 `<apex:page language` 属性；例如，如果您希望使用 Visualforce 页面以不同语言生成简报，并在发送前预览它们，则非常有用。

> > 闪电组件没有等效功能；它们始终使用用户的语言。

这不适用于 `Name` 等描述性字段值或您添加到对象中的任何自定义描述字段。要为用户输入的数据提供翻译，您必须自行开发此类功能。平台不提供对此的支持。

## 客户端-服务器通信

从根本上讲，用户所选设备（客户端）与 Salesforce 服务器上可用的数据和逻辑之间的任何通信都使用 HTTP 协议进行。作为 Force.com 开发者，您很少会涉及向服务器形成正确的 HTTP POST 或 HTTP GET 请求并解析响应的底层工作。

例如，Visualforce 的 `apex:commandButton` 和 `apex:actionFunction` (AJAX) 组件非常出色地为您处理此问题，因此您只需使用这些组件，就可以以很少的努力调用 Apex 代码。Salesforce 还会为您处理安全方面的问题，确保用户已登录并拥有用于向服务器发出请求的有效会话。对于闪电组件，没有通过组件标记的直接等效组件。相反，可以从组件的客户端控制器方法调用 `$A.enqueueAction` JavaScript 方法来访问 Apex 代码。

取决于客户端代码运行的上下文，Force.com 提供了多种通信选项，用于对标准或自定义对象执行 CRUD 操作，以及/或调用您的应用程序的 Apex 代码以执行更复杂的任务。

重要的是，您应将逻辑正确地放置在域层和服务层中，以确保无论客户端通信选项如何，您的应用程序后端都能保持一致的行为，并且每个客户端都可使用相同级别的功能。本章的各个部分将展示如何确保，无论采用何种通信方法，这些层都能被有效重用并统一访问。

## 客户端通信选项

尽管即将到来的各个部分将更详细地讨论各种通信，但对其进行完整演练超出了本书的范围。Salesforce 文档和开发者网站提供了大量关于其使用的示例。

本章的目的是了解使用它们背后的架构决策、相关限制，以及每个选项如何与前几章中介绍的 Apex 代码层进行交互。
