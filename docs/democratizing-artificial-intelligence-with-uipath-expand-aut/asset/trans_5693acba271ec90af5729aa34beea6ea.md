

## 使用 AI Center

在本节中，我们将浏览 AI Center 的关键概念。通过熟悉这些概念，我们将积累利用 AI Center 的经验，这对于构建具有机器学习能力的自动化至关重要。

在创建自动化项目时，我们通常希望遵循两条通往生产环境的路径：

- **使用预训练模型：** 开发者应遵循以下流程：
  1. 创建项目
  2. 利用 ML 包
  3. 将其部署为 ML 技能
  4. 将 ML 模型拖放到 RPA 中
  
- **使用可重训模型：** 开发者应遵循以下流程：
  1. 创建项目
  2. 上传数据集
  3. 利用 ML 包
  4. 部署训练管道以训练模型
  5. 将其部署为 ML 技能
  6. 将 ML 模型拖放到 RPA 中
  7. 将 RPA 连接回 ML 模型，提供持续反馈

在本节中，我们将研究 AI Center 的关键概念，以便遵循这两条通往生产环境的路径。

## 介绍项目

每个 AI Center 用例都始于在 AI Center 中创建项目。项目是资源集合（ML 包、数据集和管道），您将在构建认知自动化用例时利用这些资源。

## 创建项目

让我们一起构建一个简单的文本分类项目，使用 AI Center，从在 AI Center 中创建项目开始我们的练习：

- 1. 导航到您的 UiPath Cloud，并点击 AI Center 开始：

![](img/05268897c8ac5d80eb3fead1cc8a822a_175_0.png)

图 6.42 – UiPath Cloud 中的 AI Center 主页

- 2. 进入 AI Center 后，点击 Create Project 并提供描述性的项目名称和描述。

提供名称和描述后，您将被导航到项目仪表板。在此项目仪表板中，您会注意到我们的数据集、包、管道和技能标签，我们将在后续章节中介绍。让我们继续进入 AI Center 中的数据集。

## 介绍数据集

现在我们已经创建了项目，让我们查看第一个标签 – 数据集。按定义，数据集仅仅是一组数据。在 AI Center 中也是如此，其中数据集是 AI Center 项目所利用的文件和文件夹集合。在数据集内，我们可以上传任何必要的文件和文件夹集，以便我们的 ML 模型访问数据。

## 查看开箱即用的包

如前几节所述，AI Center 为您提供了许多 UiPath 和开源 ML 包，让您能够快速开始认知自动化。以下开箱即用的包可用：

- UiPath Document Understanding
- UiPath Image Analysis
- UiPath Language Analysis
- UiPath Task Mining
- Open source Image Analysis
- Open source Language Analysis
- Open source Language Comprehension
- Open source Language Translation
- Open source Tabular Data
- Open source Time Series

在回顾了开箱即用的包后，让我们接下来看看如何创建包。

## 创建包

由于我们正在进行文本分类练习，让我们选择 UiPath 的开箱即用 ML 包：

- 1. 通过点击项目仪表板中的 **ML Packages** 标签导航到 ML Packages。
- 2. 当出现 **Create a new package** 时，点击 **Out of the box Packages**。
- 3. 在开箱即用的包下方，选择 **Language Analysis > EnglishTextClassification**，选择 **Package Version 6.0**。

提供 ML 包名称后，您应该在项目仪表板中看到新的 ML 包，如 *Figure 6.44* 所示：

![](img/05268897c8ac5d80eb3fead1cc8a822a_178_0.png)

图 6.44 – 一个新创建的 TextClassification ML 包

包创建完成后，让我们研究如何创建可以训练我们已部署 ML 包的管道。

## 介绍管道

在选择了 ML 包后，我们现在可以进入 ML 管道。ML 管道是产生 ML 模型的步骤系列。在管道过程中，我们将提供输入，完成后再产生一组可用输出和日志。

在 AI Center 中，有三种类型的管道：

- **训练管道：** 从输入的 ML 包和数据集产生新训练的 ML 包版本
- **评估管道：** 从新训练的 ML 包版本和数据集产生一组指标和日志
- **完整管道：** 完整的数据处理管道，同时执行训练和评估管道

## 创建管道

对于本练习，我们将使用训练和测试数据集运行训练和评估管道：

- 1. 通过点击项目仪表板中的 **Pipelines** 标签导航到 **Pipelines**。
- 2. 点击 **Create New** 创建新管道，选择以下内容：
  - **Pipeline type:** Train run
  - **Choose package:** TextClassification
  - Choose package major version: 6
  - Choose package minor version: 0
  - Choose input dataset: Amazon Reviews/train/
  - Enable GPU: No:

![](img/05268897c8ac5d80eb3fead1cc8a822a_179_0.png)

图 6.45 – 创建训练管道

3. 点击 **Create** 开始训练管道。

> **重要提示**
> 数据集的大小会影响管道持续时间。选择 **Enable GPU** 可带来更快的输出，但 GPU 在 UiPath Community 版中不可用。

4. 再次执行步骤 2 和 3，选择 **Evaluation run** 作为管道类型，Test Classification 包的版本 6.1，以及测试数据集文件夹作为输入数据集：

# Create new pipeline run

![](img/05268897c8ac5d80eb3fead1cc8a822a_180_0.png)

图 6.46 – 创建评估管道

完成后，评估管道将返回一些工件作为输出：

- `Evaluation.csv`: 用于评估模型的数据
- `Evaluation-report.pdf`: 管道运行的输出报告

点击 **Pipelines** 页面上的评估管道将展示管道的输出和日志：

![](img/05268897c8ac5d80eb3fead1cc8a822a_181_0.png)

图 6.47 – 评估管道输出

就这样！在下一节中，我们将部署一个 ML 技能并将其附加到 RPA 工作流。

## 介绍 ML 技能

与包是 UiPath Studio 中已部署的 RPA 项目类似，ML 技能是 AI Center 中已部署的 ML 包。

部署到 AI Center 后，ML 技能可被自动化使用。在 UiPath Studio 中选择 ML 技能活动时，您可以从部署在 AI Fabric 上的 ML 技能列表中选择，并根据需要传递输入数据并利用输出数据：

![](img/05268897c8ac5d80eb3fead1cc8a822a_182_0.png)

图 6.48 – UiPath Studio 中的 MLSkill 活动连接到 AI Center 的 ML 技能

> **重要提示**
> 在 UiPath Studio 中使用 ML 技能需要 UiPath Studio v2019.10+、UiPath Robot v2019.10+ 以及安装了 `UiPath.MLServices.Activities` 包。

## 创建 ML 技能

在模型训练并评估后，我们现在可以将其部署为可与 RPA 一起使用的技能：

- 1. 通过点击项目仪表板中的 **ML Skills** 标签导航到 **ML Skills**。
- 2. 点击 **Create New** 创建新的 ML 技能，选择以下内容：
  - **Name:** `Amazon_Reviews`
  - **Choose package:** **TextClassification**
  - **Choose package major version:** **6**
  - **Choose package minor version:** **1**

![](img/05268897c8ac5d80eb3fead1cc8a822a_183_0.png)

图 6.49 – 创建 ML 技能

- 3. 点击 **Create** 创建 ML 技能。技能在可用之前需要一些时间进行打包。

- 4. 在 UiPath Studio 中启动新项目并打开 Main.xaml。

> **重要提示**
> 在开始本练习之前，请确保您已安装以下内容：
> UiPath.MLServices.Activites v1.1.8 或更高版本。

- 5. 在 Main.xaml 中，创建一个字符串变量，amazon_review。

- 6. 拖放一个 Assign 活动，将 **To** 字段设置为 amazon_review，将 **Value** 字段设置为以下内容：

```
"I love this product!"
```

- 7. 拖放一个 ML Skill 活动，选择我们的 Amazon_Reviews 技能，并将 **Item** 输入分配为我们的 amazon_review 字符串，并创建 json_response 变量作为 **JSONResponse** 输出：

![](img/05268897c8ac5d80eb3fead1cc8a822a_184_0.png)

图 6.50 – Amazon_Reviews ML 技能

- 8. 在 ML 技能活动之后，拖放一个 Message Box 活动，将 json_response 变量作为输入放置。

