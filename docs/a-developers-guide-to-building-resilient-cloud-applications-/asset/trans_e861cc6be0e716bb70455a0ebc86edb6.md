

我们使用 Azure 门户创建了一个 API 管理服务。这是一种交互式方法，但如果你喜欢使用命令行，可以使用 Azure Cloud Shell。我们将在下一节学习关于它的内容。

## 使用 Azure Cloud Shell

Azure Cloud Shell 是一个基于浏览器的交互式、已授权 shell，用于管理 Azure 资源。它为你提供了使用 Bash 或 PowerShell 作为 shell 体验的选项，具体取决于你的操作方式：

- 1. 登录到 Azure 门户并打开 Cloud Shell。

  ![](img/66aceec30d3e7dead2f71bb68a887688_50_0.png)

  图 2.18 – Azure Cloud Shell

  当 Cloud Shell 门户打开时，你有两个选项：PowerShell 或 Bash。我们将选择 Bash 环境：

  ![](img/66aceec30d3e7dead2f71bb68a887688_50_1.png)

  图 2.19 – 使用 Bash 环境的 Azure Cloud Shell

- 2. 我们将定义变量以便在命令行界面 (CLI) 命令中使用。在以下代码片段中，我们仅定义了 API 管理实例的名称、位置和电子邮件地址：

  ```bash
  $ myApiName=apicollectionpackt
  $ myLocation=EastUS2
  $ myEmail=rebai.hamida@gmail.com
  ```

- 3. 我们将创建一个资源组。以下命令将创建一个名为 packtbookrg 的资源组：

  ```bash
  $ az group create --name packtbookrg --location $myLocation
  ```

- 4. 我们将创建一个 API 管理实例。`az apim create` 命令用于创建实例。`--sku-name Consumption` 选项在我们的案例中仅用于加快演练过程，但我们可以使用任何库存保持单位 (SKU)（定价层）：

  ```bash
  $ az apim create -n $myApiName \
  --location $myLocation \
  --publisher-email $myEmail \
  --resource-group packtbookrg \
  --publisher-name First-APIM-Exercise \
  --sku-name Consumption
  ```

**重要说明**
操作大约需要 5 分钟完成。

创建 API 管理服务后，我们将在下一节导入并发布 API。

## 导入 API

本节向你展示如何导入和发布 OpenAPI 规范后端 API（以前称为 Swagger 规范）。

在本节中，你将了解我们在示例中使用的最小 Web API。

### 最小 Web API 简介

这是一种构建 API 的新方法，无需模型-视图-控制器 (MVC) 的复杂结构，且名称上是最小的。它包含构建 HTTP API 所需的基本组件。你只需要 csproj 和 program.cs。

使用最小 Web API 的好处如下：

- 它比使用 ASP.NET Web API 更简单
- 它易于学习和使用
- 你不需要 MVC 结构 – 无需控制器！
- 它只需最少的代码即可构建和编译应用程序，这意味着应用程序运行速度更快，因此性能更好
- 它具有 .NET 6 和 C# 10 的最新改进和功能

### 使用 Visual Studio 2022 创建最小 Web API

在本节中，我们将使用 Visual Studio 2022 创建 ASP.NET Core Web API 应用程序。

按照以下步骤在 Visual Studio 2022 中创建最小 Web API：

- 1. 打开 Visual Studio 2022 并选择 **Create a new project**。
- 2. 选择 **ASP.NET Core Web API** 项目模板：

  ![](img/66aceec30d3e7dead2f71bb68a887688_52_0.png)

  图 2.20 – ASP.NET Core Web API 模板

- 3. 禁用 **Use controllers (uncheck to use minimal APIs)** 以便能够使用最小 API。

  ![](img/66aceec30d3e7dead2f71bb68a887688_52_1.png)

  图 2.21 – 在 Visual Studio 2022 中启用最小 API

- 4. 当项目创建完成后，打开 program.cs 查看 API 实现：

  ```csharp
  app.MapGet("/weatherforecast", (HttpContext httpContext) =>
  {
      var forecast = Enumerable.Range(1, 5).Select(index =>
          new WeatherForecast
          {
              Date = DateTime.Now.AddDays(index),
              TemperatureC = Random.Shared.Next(-20, 55),
              Summary = summaries[Random.Shared.Next(summaries.Length)]
          })
          .ToArray();
      return forecast;
  })
  .WithName("GetWeatherForecast");
  ```

  图 2.22 – Program.cs 中的最小 API 示例

在探索了最小 Web API 后，我们将在下一节学习如何将其导入 API 管理服务。

### 将 API 导入 API 管理

让我们查看将 API 导入 API 管理的步骤：

- 1. 在 Azure 门户中搜索并选择 API 管理服务。
- 2. 接下来，在 API 管理窗口中，选择你已创建的 API 管理实例。
- 3. 在 API 管理服务导航窗格中选择 APIs。

  ![](img/66aceec30d3e7dead2f71bb68a887688_53_0.png)

  图 2.23 – API 管理中的 APIs

- 4. 从列表中选择 OpenAPI。之后，我们将在弹出窗口中选择 Full：

  ![](img/66aceec30d3e7dead2f71bb68a887688_54_0.png)

  图 2.24 – 选择 OpenAPI

我们需要在“Create from OpenAPI specification”弹出窗口中填写所有所需信息：

### Create from OpenAPI specification

![](img/66aceec30d3e7dead2f71bb68a887688_55_0.png)

图 2.25 – Create from OpenAPI specification

- 5. 要从 **OpenAPI specification** 创建 API，我们需要添加所有配置设置。下表描述了前一个屏幕截图中显示的每个设置：

| 设置 | 描述 |
|---|---|
| **OpenAPI specification** | 这是指实现 API 的服务。请求将被转发到此地址。一旦输入 URL，表单中大部分所需信息会自动填写。 |
| **Display name** | 配置后，该名称将在开发者门户中显示。 |
| **Name** | 这是 API 的唯一名称。 |
| **Description** | 这是可选的，用于提供与你的 API 相关的更多信息。 |
| **API URL suffix** | API 管理通过其后缀区分 API。该后缀对每个 API 都是唯一的。它将被添加到 API 管理服务的基 URL。 |

点击 **Create** 导入 API。

### 配置后端设置

API 已创建，现在需要指定后端。

我们将选择右侧刀片中的设置并在 **Web service URL** 字段中输入 URL。之后，我们将取消选择 **Subscription required** 复选框。

一旦 API 已导入并且后端已配置，就是测试 API 的时候了。在下一节中，我们将学习如何在 Azure API 管理服务中测试我们的 API。

### 测试 API

我们需要选择 Test 选项卡。在左侧，我们会找到与我们的 API 相关的所有操作。在页面右侧是查询参数和标头（如有）。Ocp-Apim-Subscription-Key 会自动填充与此 API 关联的订阅密钥。另一个选择将显示方法列表。我们可以选择一种方法，例如 BillingList，然后点击 Send。

![](img/66aceec30d3e7dead2f71bb68a887688_58_0.png)

图 2.27 – 测试 API

后端响应 200 OK 并返回一些数据。

### HTTP 响应

```http
HTTP/1.1 200 OK
content-type: application/json; charset=utf-8
date: Sun, 17 Apr 2022 19:00:37 GMT
ocp-apim-apiid: healthcare-solution-billing-api
```

图 2.28 – 后端响应 200 OK

在下一节中，我们将学习如何使用 Azure API 管理来代理公共 API。

## 练习 2 – 使用 Azure API 管理代理公共 API

当我们设计解决方案以使用一个或多个 API 时，通信在不同服务、系统和脚本之间必须通过 API 进行。在描述这些通信并设计我们的 API 之前，我们必须考虑以下几个要素：

- 路由结构
- 身份验证和授权
- 速率限制

使用 Azure API 管理，在数据接收或发送之前，你可以轻松代理现有 API 并修改输入和输出。

在大多数情况下，我们希望修改现有公共 API 的结构、添加身份验证、限制传入请求的数量，甚至缓存获得的结果。这就是本节将要涵盖的内容，我们将发现管理 API 和快速使用它的便捷性。

我们可以使用 Azure Logic Apps 代理、Azure App Service 或 Azure Functions 应用，但在我们的示例中，我们将使用公共 API。

在本练习中，我们将使用之前导入的 API。

### 导入 OpenAPI 模式以进行代理

Azure API 管理服务将导入的不同格式。

在可用方法列表中，/BillingList 方法以 JSON 格式提供账单明细列表。使用标准 PowerShell 查询，如以下代码片段所示，将允许我们通过 API 代理检索结果：

```powershell
$Result = Invoke-RestMethod -URI 'https://healthcarebillingapi.azurewebsites.net/BillingList'
$Result.data.summary
```

开发人员倾向于对返回给他们的输出键进行更改。我们将选择需要修改的方法；之后，我们将在 **Outbound Processing** 中选择相同的方法并选择 **Add Policy**。之后，选择 **Other policies**，并在 **Transformation policies** 中查找 **Find and replace string in body** 选项。

