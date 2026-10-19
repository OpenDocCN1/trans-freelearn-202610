

2. 现在，点击 **我的 Azure DevOps 组织**。这将打开一个新浏览器窗口：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_418_0.png)

图 12.7 – 新 Azure DevOps 组织的详细信息

填写你的详细信息并按下 **继续**。

3. 点击 **创建新组织**：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_419_0.png)

# 开始使用 Azure DevOps

计划更好、协同编码、更快交付，使用 Azure DevOps

创建新组织

图 12.8 – 创建新的 Azure DevOps 组织

4. 你将看到 **开始使用 Azure DevOps** 窗口：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_419_1.png)

图 12.9 – 开始使用新的 Azure DevOps 组织

点击 **继续。**

5. 在下一个屏幕中，输入 **命名你的 Azure DevOps 组织** 的详细信息并选择离你最近的区域：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_420_0.png)

图 12.10 – 为新的 Azure DevOps 组织命名

添加 **验证码** 并点击 **继续。**

6. 提供 **项目名称**

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_420_1.png)

图 12.11 – 在新的 Azure DevOps 组织中创建新项目

点击 + **创建项目。**

7. 新项目的家庭屏幕应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_421_0.png)

图 12.12 – Azure DevOps 组织家庭屏幕

8. 在屏幕左下角，点击 项目设置 > 服务连接：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_421_1.png)

图 12.13 – 创建新的服务连接

点击 创建服务连接。

9. 选择 Azure 资源管理器 并点击 下一步：

图 12.14 – 选择新服务连接的连接类型

10. 在 新建 Azure 服务连接 屏幕上，选择 服务主体（自动） 并点击 下一步：

图 12.15 – 选择新服务连接的身份验证方法

11. 在 新建 Azure 服务连接 页面上，选择 订阅。订阅 框应自动刷新并显示正在使用的 Azure 订阅。提供 服务连接名称 的值并点击 下一步：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_423_0.png)

图 12.16 – 为新的服务连接命名

点击 保存。

12. Azure 服务连接确认屏幕应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_423_1.png)

图 12.17 – 新服务连接确认

13. 现在，我们需要导入 Packt Git 仓库。

使用左侧菜单，点击 代码库。

在 **导入仓库** 下，点击 **导入** 按钮。提供以下值：

- **仓库类型：** Git
- **克隆 URL：** https://github.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_424_0.png)

图 12.18 – 导入 Packt Git 仓库

14. 点击 **导入**。几秒钟后，项目的 Packt Git 仓库将被导入，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_424_1.png)

图 12.19 – Packt Git 仓库

15. 你可以为此练习使用 Azure DevOps 的免费层。在创建新的 Azure DevOps 组织后，你需要通过访问填写 **免费并行请求表**

https://aka.ms/azpipelines-parallelism-request：

# Azure DevOps 并行请求

此表单用于用户请求在 Azure DevOps 中增加并行度。

请注意，处理请求可能需要 2-3 个工作日。我们正在改进此流程。抱歉带来不便。

* 必填

1. 你的名字是？ *

输入你的答案

2. 你的电子邮件地址是？ *

输入你的答案

图 12.20 – Azure DevOps 并行请求页面

请注意，Azure 可能需要 2-3 个工作日来审查并回复你的请求。一旦你的请求获批，你将收到一封电子邮件，如下所示：

-------- 转发邮件 --------
发件人：<v-niezz@microsoft.com>
日期：2021 年 6 月 23 日星期三 上午 11:43
主题：免费层请求已完成
收件人：[已隐藏]
抄送：<azpipelines-freetier@microsoft.com>

嗨 [已隐藏]，

我们已收到你增加 Azure DevOps 中免费并行度的请求。

请注意，你的请求已 完成

请求详情：

姓名
电子邮件
组织名称
并行类型
私有

为你的组织请求免费并行度：Azure DevOps 并行请求表单

图 12.21 – Azure DevOps 并行请求确认邮件

一旦你收到此邮件，你就可以开始创建必要的 CI/CD 管道。

# 创建 Electroniz 基础设施 CI/CD 管道

最后，我们可以开始开发我们的第一个 CI/CD 管道。出于明显的原因，我们选择先开发基础设施管道。原因是代码管道直接依赖于由基础设施管道创建的 Azure 资源。让我们开始吧：

1. 你可能还记得，推荐的管道设计包含一个 Azure Key Vault 来存储 CI/CD 管道的密钥。在 Azure Cloud Shell 中运行以下命令来创建一个：

```
RESOURCEGROUPNAME="elz_prod"
LOCATION="eastus"
IAC_ENVIRONMENT=prod
KEYVAULTNAME="deploykvelz"
az group create --name $RESOURCEGROUPNAME --location $LOCATION
az keyvault purge -n $KEYVAULTNAME
az config set extension.use_dynamic_install=yes_without_prompt
az keyvault create --location $LOCATION --name $KEYVAULTNAME --resource-group $RESOURCEGROUPNAME --enabled-for-template-deployment true
```

2. 我们现在将向新创建的 Azure Key Vault 添加几个密钥。使用 Azure 门户，导航到 主页 > 资源组 > elz_prod > deploykvelz。

点击 密钥。然后，使用 生成/导入 创建两个新密钥，如下所示：

- 名称：SQLUSER 值：salesadmin
- 名称：SQLPASSWORD 值：<Azure SQL 管理员的密码>：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_427_0.png)

图 12.22 – 在 deploykvelz Azure Key Vault 中创建密钥

> **重要**
> 请注意，在前一步中，我们强制自己手动创建密钥。创建 Azure Key Vault 中的密钥过程很容易被脚本化，但那样你就需要违背最佳实践并将密码存储在配置文件中。

3. 点击左侧的 管道 菜单并选择 库：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_427_1.png)

图 12.23 – 在 Azure Pipelines 中导航到库

4. 创建 **新的变量组**：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_428_0.png)

图 12.24 – 在 Azure Pipelines 中创建新的变量组

点击 **+ 变量组**。

5. 使用以下详细信息创建新的变量组：

- **变量组名称**：`electroniz_infra_variables`
- **描述**：`这些变量由基础设施部署文件使用`

现在，每次按下 + 添加 按钮时添加以下变量：

- **名称**：`DATABRICKS_WORKSPACE_NAME` **值**：`elzdbws`
- **名称**：`DATAFACTORY_NAME` **值**：`elzdf`
- **名称**：`ENVIRONMENT` **值**：`prod`
- **名称**：`EVENTHUB_CONTAINER` **值**：`ecomm logs`
- **名称**：`EVENTHUB_NAME` **值**：`esaleshub`
- **名称**：`EVENTHUB_NAMESPACE` **值**：`esalesns`
- **名称**：`EVENTHUB_STORAGE_ACCOUNT` **值**：`elzlogs`
- **名称**：`EVENTHUB_SUBSCRIPTION` **值**：`esalesevent`
- **名称**：`EVENTHUB_TOPIC_NAME` **值**：`esales`
- **名称**：`KEY_VAULT_NAME` **值**：`deploykvelz`
- **名称:** KEY_VAULT_RESOURCE_GROUP **值:** elz_prod
- **名称:** LAKEHOUSE_BRONZE_LAYER **值:** bronze
- **名称:** LAKEHOUSE_SILVER_LAYER **值:** silver
- **名称:** LAKEHOUSE_GOLD_LAYER **值:** gold
- **名称:** LAKEHOUSE_STORAGE_ACCOUNT **值:** elz
- **名称:** PROJECT **值:** dlake

在很多情况下，Azure 资源名称是全局唯一的。尽量使项目名称尽可能唯一。由于很多人会使用相同的说明，具有唯一性将消除与唯一资源名称相关的不必要错误。

现在添加以下变量：

- **名称:** REGION **值:** East US
- **名称:** RESOURCE_GROUP **值:** elz_prod
- **名称:** SERVICE_CONNECTION **值:** electroniz_connection

确保以下条目与你之前创建的 **服务连接** 匹配：

- **名称:** SQL_DB_NAME **值:** salesdb
- **名称:** SQL_PASSWORD_SECRET **值:** SQLPASSWORD

> **重要**
> 请注意，此参数不是实际密码；它只是存储在我们之前创建的 Azure Key Vault 中的密码密钥的名称。

- **名称:** SQL_SERVERNAME **值:** elzsql
- **名称:** SQL_USERNAME_SECRET **值:** SQLUSER
- **名称:** SQL_USERNAME **值:** salesadmin
- **名称:** SUBSCRIPTION_ID **值:** XXXXX-XXX-XXX-XXX-XXXXXXXX

要获取 **SUBSCRIPTION_ID** 的值，请使用 Azure 门户并导航到 **主页 > 订阅**。记下订阅 ID 并在此处使用，如下所示：

- **名称:** SYNAPSE_WORKSPACE_NAME **值:** elzsyn

