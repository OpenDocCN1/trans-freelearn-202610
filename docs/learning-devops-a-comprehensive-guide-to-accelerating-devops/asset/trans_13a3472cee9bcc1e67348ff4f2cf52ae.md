

下面的代码说明响应时间小于 400 ms：

```
pm.test("Response time is less than 400ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(400);
});
```

下面的代码说明 JSON 格式的响应不为空：

```
pm.test("Json response is not empty", function () {
    pm.expect(pm.response).to.be.json;
});
```

下面的代码说明，在 JSON 响应中，`userId` 属性等于 1：

```
pm.test("Json response userId eq 1", function () {
    var jsonRes = pm.response.json();
    pm.expect(jsonRes.userId).to.eq(1);
});
```

因此，最终，我们请求的 **Tests** 选项卡（用于测试我们的 API）包含所有这些代码，如下面的截图所示：

![](img/d3344b39c888aab0b832625465f837c9_376_0.png)

图 11.14 – Postman Tests 代码

我们已完成带有测试编写的 Postman 请求，它将根据 API 的反馈代码、性能和响应内容来检查 API 的正常运行。

> 注意
> 要了解有关 Postman 测试和脚本的更多信息，可以阅读 https://learning.getpostman.com/docs/postman/scripts/intro_to_scripts 上的文档。

在本节中，我们刚刚了解了如何在 Postman 中编写 API 测试来检查我们 API 的正常运行。我们现在将在本地运行我们的 Postman 请求以测试我们的 API。

## 在本地执行 Postman 请求测试

到目前为止，在 Postman 中，我们已创建一个集合，其中两个请求包含我们要测试的 API 的参数和测试。要使用参数和测试来检查 API 的正常运行，我们现在必须执行位于 Postman 中的请求。请注意，只有在执行结束时，我们才会知道我们的 API 是否符合我们的预期。

要执行 Postman 请求，请执行以下操作：

1. 首先必须选择所需的环境。
2. 单击请求的 **Send** 按钮，如下面的截图所示：

![](img/d3344b39c888aab0b832625465f837c9_377_0.png)

图 11.15 – Postman 注册

3. 在 **Body** 选项卡中，我们可以查看查询响应的内容，如果我们想以 JSON 格式显示它，可以选择显示格式。下面的截图显示了以 JSON 格式显示的请求响应：

![](img/d3344b39c888aab0b832625465f837c9_378_0.png)

4. **Test Results** 选项卡显示我们之前编写的测试的执行结果，在我们的情况中，四个测试都已正确执行——它们全部显示为绿色，如下面的截图所示：

![](img/d3344b39c888aab0b832625465f837c9_378_1.png)

在前面的截图中，我们可以看到 Postman 请求的返回代码等于 200，这对应于请求成功执行的返回代码，以及其 23 ms 的执行时间，这低于我为自己设定的阈值（400 ms）作为示例。

如果某个测试失败，它将显示为红色以便清晰识别。失败测试的示例如下面的截图所示：

![](img/d3344b39c888aab0b832625465f837c9_379_0.png)

图 11.18 – Postman 测试失败

我们刚刚了解了执行 Postman 请求以测试 API，但此执行仅适用于当前请求。如果我们想要执行集合中的所有 Postman 请求，可以使用 **Postman Collection Runner**。

Postman Collection Runner 是 Postman 的一个功能，它会按请求在集合中组织的顺序自动执行集合中的所有请求。

你可以通过访问 `https://learning.getpostman.com/docs/postman/collection_runs/starting_a_collection_run/` 了解更多有关集合文档的信息。

下面的两个截图显示了 **Runner** 执行步骤，在其中我们选择要执行的集合、环境和迭代次数。要启动其执行，我们单击 **Run DemoBook** 按钮：

![](img/d3344b39c888aab0b832625465f837c9_380_0.png)

图 11.19 – Postman Runner

因此，在 **Runner** 选项卡中，我们可以看到集合中所有请求的测试执行结果，如下面的截图所示：

![](img/d3344b39c888aab0b832625465f837c9_380_1.png)

图 11.20 – Postman Runner 执行

![](img/d3344b39c888aab0b832625465f837c9_381_0.png)

在本节中，我们学习了如何执行 Postman 请求以单元方式测试 API，然后再使用 Postman Runner 执行集合中的所有请求。在下一节中，我们将介绍 Newman，它允许我们自动化执行 Postman 测试。

## 理解 Newman 概念

在本章中，到目前为止，我们一直在讨论在本地使用 Postman 来测试我们开发或使用的 API。但在单元测试、验收测试和集成测试中，重要的是它们是自动化的，以便可以在 CI/CD 管道中执行。

Postman 本身是一个图形工具，不会自动化自身，但有一个名为 **Newman** 的另一个工具，它可以自动化在 Postman 中编写的测试。

![](img/d3344b39c888aab0b832625465f837c9_381_1.png)

Newman 是一个免费的命令行工具，其巨大的优势在于可以自动化已经在 Postman 中编写的测试。它允许我们将 API 测试执行集成到 CI/CD 脚本或流程中。

此外，它还提供生成不同格式（HTML、JUnit 和 JSON）的测试结果报告的可能性。

不过，Newman 不允许我们执行以下操作：

- 创建或配置 Postman 请求；正如我们将看到的，由 Newman 执行的请求将从 Postman 导出。
- 仅执行集合中的一个请求——它执行集合中的所有请求。

## 使用 Postman 测试 API

> 注意
> 要了解有关 Newman 的更多信息，可以访问 `https://www.npmjs.com/package/newman` 上的产品页面。

要使用 Newman，我们需要——如本章 _Technical requirements_ 部分所述——安装 Node.js 和 npm，它们可在 `https://nodejs.org/en/` 上获得（此安装程序同时安装这两个工具）。

然后，要安装 Newman，我们必须在终端中执行此命令：

```
npm install -g newman
```

下面的截图显示了命令的执行：

![](img/d3344b39c888aab0b832625465f837c9_382_0.png)

图 11.21 – Newman 安装

此命令在全局范围内安装 npm `newman` 包及其所有依赖项，也就是说，它在整个本地机器上都可访问。

安装后，我们可以通过运行 `newman --help` 命令来测试其安装，该命令显示要使用的参数和选项，如下面的截图所示：

![](img/d3344b39c888aab0b832625465f837c9_382_1.png)

图 11.22 – Newman 帮助命令

在本节中，我们通过讨论 Newman 的优势来介绍它，并学习了如何安装它。在下一节中，我们将导出 Postman 的集合和环境以供 Newman 使用。

## 为 Newman 准备 Postman 集合

正如我们刚刚看到的，Newman 是 Postman 的客户端工具，要工作，它需要我们在 Postman 中创建的集合、请求和环境的配置。

这就是为什么，在运行 Newman 之前，我们将不得不导出 Postman 的集合和环境，并且此导出将作为 Newman 的参数。那么，让我们开始导出我们在 Postman 中创建的 DemoBook 集合。

### 导出集合

导出 Postman 集合包括获取一个 JSON 文件，该文件包含此集合及其中请求的所有设置。

正是从这个 JSON 文件，Newman 才能运行与我们从 Postman 运行时相同的 API 测试。

要执行此导出，请执行以下任务：

1. 转到我们要导出的集合的上下文菜单。
2. 选择 **Export** 操作。
3. 然后，在打开的窗口中，取消选中 **Collection v2.1 (recommended)** 复选框。
4. 最后，通过单击 **Export** 按钮进行验证。

