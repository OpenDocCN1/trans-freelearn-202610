

13. 按如下方式更改管道名称：

# 重命名/移动管道

名称
electroniz_infra_cicd_pipeline
选择文件夹
\

图 12.58 – 为代码命名 Azure 管道

点击保存。

14. 再次点击三个点，然后点击运行管道：

# 运行管道

选择以下参数并手动运行管道

分支/标签
main
选择分支、提交或标签
高级选项
变量
此管道未定义变量
运行阶段
按配置运行
资源
使用所有资源最新版本
启用系统诊断

图 12.59 – 运行代码 Azure 管道

15. electroniz_code_cicd_pipeline 现在将被触发：

# 图 12.60 – 触发代码 Azure 管道

16. 监控管道在代码部署的各个阶段的运行情况。部署完成后，阶段会标记为绿色对号：

# 图 12.61 – 监控代码 Azure 管道

17. 在另一个标签页中，导航到以下 Azure 门户链接：Home > Resource Groups > elz_prod > prod-dlake-elzdf

点击 Open 下的 Open Azure Data Factory Studio：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_452_0.png)

图 12.62 – 确认在 Azure Data Factory 中创建的数据管道

我们可以看到在 Azure Data Factory 中创建的四个数据管道，这足以证明 electroniz_code_cicd_pipeline 运行良好。但工作尚未完全完成。我们刚刚为现有代码创建了一个 CI/CD 管道。现在，我们需要使其适用于未来的更改和新增。

## 创建 CI/CD 生命周期

希望我们都同意新代码的开发是一个持续的过程。之前，我们讨论了传统方法的弊端，即将若干更改提交到代码仓库，然后进行合并测试。CI/CD 通过持续集成代码并自动部署更改来缓解其中一些问题。在高层面上，CI/CD 管道的代码合并周期如下所示：

- **构建**：更改后，开发人员在共享仓库中提交代码，通常在功能分支下。
- **审批**：开发人员生成拉取请求供团队审批。如果代码未通过审批，评审者会提出修复建议或批准代码合并。
- **合并**：评审成功后，功能分支与主分支合并。
- **部署**：流程自动将代码部署到生产环境：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_453_0.png)

# 图 12.63 – CI/CD 生命周期

在以下步骤中，我们将了解其实际工作原理。以下步骤将带我们了解一个示例场景：

- 1. 开发人员提交代码更改并为评审者创建拉取请求以供审批。
- 2. 评审者审批更改并将其合并到主分支。
- 3. 自动 CI/CD 管道部署代码。

请注意，以下步骤将由多个团队成员执行，因此在每个步骤中都提到了负责人。

## 构建 – 开发人员

可以通过执行以下步骤完成：

- 1. 导航到 Repos > Branches。点击 New Branch 并创建新分支，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_454_0.png)

图 12.64 – 创建新的代码分支

点击创建：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_454_1.png)

图 12.65 – developer1 分支创建

2. 现在，点击 developer1。你现在已连接到 developer1 分支：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_455_0.png)

图 12.66 – 确认切换到 developer1 分支

3. 导航到 project/cicd/azure-code-pipeline.yml。点击编辑。现在，将以下代码粘贴到文件末尾：

```
- stage: Lakehouse_Database_Scripts
  jobs:
    - job: DeployDatabaseScripts
      pool:
        vmImage: ubuntu-latest
      steps:
        - task: AzureKeyVault@1
          inputs:
            azureSubscription: '$(SERVICE_CONNECTION)'
            keyVaultName: '$(KEY_VAULT_NAME)'
            secretsFilter: '*'
        - task: CmdLine@1
          displayName: Run Sqlcmd
          inputs:
            filename: sqlcmd
            arguments: '-S $(SYNAPSE_CON_STRING) -U $(SQLUSER) -P $(SQLPASSWORD) -i project/aggregation/sql/create_db.sql'
        - task: CmdLine@1
          displayName: Run Sqlcmd
          inputs:
            filename: sqlcmd
```

```
arguments: '-S $(SYNAPSE_CON_STRING) -U $(SQLUSER) -P $(SQLPASSWORD) -i project/aggregation/sql/create_synapse_creds.sql'
- task: CmdLine@1
displayName: Run Sqlcmd
inputs:
  filename: sqlcmd
arguments: '-S $(SYNAPSE_CON_STRING) -U $(SQLUSER) -P $(SQLPASSWORD) -i project/aggregation/sql/create_silver_views.sql'
- task: CmdLine@1
displayName: Run Sqlcmd
inputs:
  filename: sqlcmd
arguments: '-S $(SYNAPSE_CON_STRING) -U $(SQLUSER) -P $(SQLPASSWORD) -i project/aggregation/sql/drop_gold_tables.sql'
- task: CmdLine@1
displayName: Run Sqlcmd
inputs:
  filename: sqlcmd
arguments: '-S $(SYNAPSE_CON_STRING) -U $(SQLUSER) -P $(SQLPASSWORD) -i project/aggregation/sql/create_gold_tables.sql'
```

确保代码缩进已设置适当。编辑文件后，它应如下所示：

## azure-code-pipeline.yml

```
106
107   action: 'Create Or Update Resource Group'
108   resourceGroupName: '${RESOURCE_GROUP}'
109   location: '${REGION}'
110   templateLocation: 'Linked artifact'
111   csmFile: 'project/cicd/electroniz_master_pipeline.json'
112   overrideParameters: '-factoryName ${DATAFACTORY_NAME}'
113   deploymentMode: 'Incremental'
114   deploymentName: 'dataFactoryDeployment'
115
116 - stage: Lakehouse_Database_Scripts
117   jobs:
118     - job: DeployDatabaseScripts
119       pool:
120         vmImage: ubuntu-latest
121       steps:
122         - task: AzureKeyVault@1
123           inputs:
124             azureSubscription: '${SERVICE_CONNECTION}'
125             keyVaultName: '${KEY_VAULT_NAME}'
126             secretsFilter: '*'
127         - task: CmdLine@1
128           displayName: Run Sqlcmd
129           inputs:
130             filename: sqlcmd
131             arguments: '-S ${SYNAPSE_CON_STRING} -U ${SQLUSER} -P ${SQLPASSWORD} -i project/aggregation/sql/create_db.sql'
132         - task: CmdLine@1
133           displayName: Run Sqlcmd
134           inputs:
135             filename: sqlcmd
136             arguments: '-S ${SYNAPSE_CON_STRING} -U ${SQLUSER} -P ${SQLPASSWORD} -i project/aggregation/sql/create_synapse_creds.sql'
```

图 12.67 – 向 azure-code-pipeline.yml 添加新阶段

按提交。

在下一个屏幕上，确保分支设置为 developer1。按提交。现在，按创建拉取请求按钮。创建新的拉取请求，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_457_0.png)

图 12.68 – 创建新的拉取请求

点击创建。

## 审批 – 评审者

可以通过执行以下步骤完成：

- 1. 评审者会收到新拉取请求的通知（通常通过电子邮件）。评审者可以单独或作为一个组评审更改并添加评论。一旦代码经过评审，请添加新评论，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_458_0.png)

图 12.69 – 评审拉取请求

## 合并 – 团队负责人

可以通过执行以下步骤完成：

- 1. 一旦所有评审者都发表了评论，团队负责人可以拒绝或批准更改。在我们的案例中，我们假设评审过程是积极的。点击批准：

图 12.70 – 批准拉取请求

2. 注意无合并冲突。这意味着新代码已准备好合并：

图 12.71 – 检查合并冲突

## 部署 – 自动化

可以通过执行以下步骤完成：

- 1. 从 developer1 分支向主分支合并代码后，YAML 部署文件中的触发器部分会将代码部署到指定环境。点击完成：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_460_0.png)

图 12.72 – 将 developer1 分支合并到主分支

- 2. 合并过程成功后会收到以下确认消息：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_460_1.png)

图 12.73 – 代码合并确认

