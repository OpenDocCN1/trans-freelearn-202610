

点击保存：

Library > electroniz_infra_variables

变量组   保存   克隆   安全   管道权限

# 属性

变量组名称

electroniz_infra_variables

描述

这些变量由基础设施部署文件使用

将来自 Azure Key Vault 的机密链接为变量

# 变量

| 名称 | 值 |
|---|---|
| DATABRICKS_WORKSPACE_NAME | elzdbws |
| DATAFACTORY_NAME | elzdf |
| ENVIRONMENT | prod |
| EVENTHUB_CONTAINER | ecomm logs |

图 12.25 – Azure Pipelines 中的 electroniz_infra_variables 变量组

6. 我们现在准备创建基础设施管道。使用左侧菜单，点击 Pipelines：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_430_0.png)

# 创建你的第一个管道

使用我们的向导自动化你的构建和发布流程，并在几分钟内从代码到云托管。

创建管道

图 12.26 – 创建新的 Azure pipeline

点击 **创建管道。**

7. 此时，你将看到 **你的代码在哪里？** 屏幕：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_431_0.png)

图 12.27 – 为 Azure pipeline 配置代码仓库

选择 **Azure Repos Git。**

8. 你现在将看到 **选择一个仓库** 屏幕：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_431_1.png)

图 12.28 – 为 Azure pipeline 选择代码仓库

点击 **electroniz_pipelines。**

9. 接下来是配置你的管道屏幕：

新管道
配置你的管道

入门管道
从一个可自定义的最小管道开始，用于构建和部署你的代码。

现有 Azure Pipelines YAML 文件
在仓库的任何分支中选择一个 Azure Pipelines YAML 文件。

图 12.29 – 为 Azure pipeline 选择现有的 YAML 文件

选择 现有 Azure Pipelines YAML 文件。

10. 在 选择一个现有 YAML 文件 屏幕上，提供以下值：

- 分支：main
- 点击路径；值将被自动填充：/project/infra/azure-infra-pipeline.yml：

选择一个现有 YAML 文件
在仓库的任何分支中选择一个 Azure Pipelines YAML 文件。
分支
main
路径
/project/infra/azure-infra-pipeline.yml
从下拉列表中选择文件或输入文件路径
electroniz_pipelines

图 12.30 – 为 Azure pipeline 选择基础设施 YAML 文件

点击 继续。

11. 在 查看你的管道 YAML 屏幕上，你应该会看到以下内容：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_433_0.png)

图 12.31 – 基础设施 YAML 文件的解剖结构

让我们简要查看 YAML 文件的结构：

- **触发器**：指定将为 CI/CD 调用的分支。
- **变量**：CI/CD 管道的配置数据。可以指向库中命名的变量组。
- **阶段、作业和步骤**：按顺序运行的一组作业。作业是一系列步骤，而步骤是实际执行特定功能的任务和脚本。

点击 保存并运行 按钮旁边的箭头并点击 保存。

12. 使用左侧菜单，点击 Pipelines。屏幕右手侧，点击三个点并点击 重命名/移动：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_434_0.png)

图 12.32 – 重命名基础设施 Azure pipeline

13. 按如下方式更改管道名称：

# 重命名/移动管道

名称

electroniz_infra_cicd_pipeline

选择文件夹

\

取消 保存

图 12.33 – 为基础设施 Azure pipeline 命名

点击 保存。

14. 再次点击三个点，这次点击 运行管道：

# 运行管道

选择下面的参数并手动运行管道

分支/标签

main

选择分支、提交或标签

# 高级选项

变量
此管道未定义变量

要运行的阶段
按配置运行

资源
使用所有资源的最新版本

启用系统诊断

取消

运行

图 12.34 – 运行基础设施 Azure pipeline

点击 运行。

15. electroniz_infra_cicd_pipeline 现在将被触发：

#20210804.4 Updated azure-infra-pipeline.yml
on electroniz_infra_cicd_pipeline

摘要

由 手动运行

查看 73 处更改

仓库和版本
electroniz_pipelines
main e52909fb

开始时间和已用时间
今天下午 12:48
3m 15s

相关
0 个工作项
0 个工件

测试和覆盖率
开始使用

阶段 作业

Stores_Azure_SQL_Da...
1 个作业已完成 2m 44s

Lakehouse_Storage_A...
0/1 已完成 11s
DeployStorageAccount 11s

Lakehouse_Event_Hub
未开始

Lakehouse_Databricks
未开始

Lakehouse_Data
未开始

图 12.35 – 触发基础设施 Azure pipeline

16. 监控管道在基础设施部署的各个阶段进行：

图 12.36 – 监控基础设施 Azure pipeline

17. CI/CD 管道完成后，检查所有阶段的状态，如下所示。每个阶段的绿色勾号意味着作业已完成：

图 12.37 – 完成基础设施 Azure pipeline

18. 在浏览器另一个标签页中，导航到 Azure 门户，前往 Home > Resource Groups > elz_prod。

在管道运行时注意 Azure 资源的创建。你可能需要多次刷新浏览器窗口以检查新项目：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_437_0.png)

图 12.38 – 基础设施 Azure pipeline 创建的资源确认

19. 最终，管道完成后，管道窗口将如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_437_1.png)

图 12.39 – 基础设施 Azure pipeline 完成确认

20. 到目前为止，一切顺利。然而，你可能已经注意到我们手动触发了此 CI/CD 管道。手动触发管道仅在开发和测试模式下是可以接受的。在生产环境中，我们应旨在自动触发管道。

21. 在接下来的几个步骤中，我们将编辑现有模板，以便代码更改在提交到 main 分支后立即自动应用。

你可能还记得作为 electroniz_infra_cicd_pipeline 的一部分，我们创建了一个 Azure Data Lake 账户。注意最初实现的那三个标签：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_438_0.png)

图 12.40 – proddlakeelz Azure Data Lake Storage 账户的标签信息

为了模拟代码更改，我们将编辑 storage_accounts.json 模板文件并添加一个新标签。

导航到 Repos > project > infra > storage_accounts > storage_accounts.json。

添加一个名为 "group": "finance" 的新标签，如下所示：

图 12.41 – 在 storage_accounts.json 中添加新标签

点击 提交：

```
36 },
37 "resources": [
38 {
39 "name": "[variables('storageAccountName')]",
40 "type": "Microsoft.Storage/storageAccounts",
41 "apiVersion": "2021-02-01",
42 "sku": {
43 "name": "Standard_LRS"
44 },
45 "kind": "StorageV2",
46 "location": "[parameters('location')]",
47 "properties": {
48 "isHnsEnabled": true
49 },
50 "tags": {
51 "group": "finance",
52 "owner": "data engineering",
53 "project": "lakehouse",
54 "environment": "production"
55 },
56 },
57 "resources": [
58 {
59 "name": "[concat('default/', parameters('bronzeLayer'))]",
60 "type": "blobServices/containers",
61 "apiVersion": "2019-06-01",
62 "dependsOn": [
63 "[variables('storageAccountName')]
64 ],
65 "properties": {
66 "publicAccess": "None"
```

# 提交

评论
向 storage_accounts.json 添加了一个新标签
分支名称
main
要链接的工作项
按 ID 或标题搜索工作项

图 12.42 – 提交 storage_accounts.json

再次点击 提交。

22. 现在，导航回 Pipelines。这次，electroniz_infra_cicd_pipeline 应该自动触发。CI/CD 管道应仅选取上次以来的增量更改并将其应用到 Azure 资源：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_440_0.png)

图 12.43 - electroniz_infra_cicd_pipeline 自动触发

然后你将看到部署后新添加标签的确认：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_440_1.png)

图 12.44 – 部署后新添加标签的确认

至此，我们可以庆祝了——第一个 CI/CD 管道按预期工作！从现在开始，Azure 资源的增量更改可以自动推送到 Azure。我们的重点现在转向第二个管道。

