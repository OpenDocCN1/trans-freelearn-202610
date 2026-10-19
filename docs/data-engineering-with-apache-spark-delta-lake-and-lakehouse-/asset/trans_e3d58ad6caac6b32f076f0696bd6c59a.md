

+   - 在执行了某些价格调整后，相同的记录于 2021 年 4 月 4 日 20:00 使用 `electroniz_batch_ingestion_pipeline` 运行后以如下方式被摄取到 bronze 层。

| order_number | order_date | sale_price | order_mode | updated_at |
|---|---|---|---|---|
| 500 | 02/16/2016 | 93.77 | EDIT | 2021-04-28T19:38:49.230+0000 |
| 1254 | 07/26/2016 | 49.78 | EDIT | 2021-04-28T19:38:49.217+0000 |
| 1501 | 06/23/2017 | 47.65 | EDIT | 2021-04-28T19:38:49.197+0000 |
| 2234 | 03/29/2016 | 34.40 | EDIT | 2021-04-28T19:38:49.223+0000 |
| 2345 | 10/03/2016 | 32.10 | EDIT | 2021-04-28T19:38:49.207+0000 |

Figure 7.13 – Edited orders ingested into the electroniz lakehouse creating duplicates

与客户类似，订单在 bronze 层中也被重复了。在此数据被使用之前，需要对其进行适当的去重，使得最终用户只能看到记录的最新版本。

## Insecure data

在第 4 章《理解数据管道》中，在发现阶段，Electroniz 提到湖屋必须遵守 PCI 规定，因此所有 PII 数据都需要得到适当的屏蔽和加密。在仔细审查数据后，我们发现所有 PII 列在 bronze 层中都以明文形式可见，如下所示：

| customer_id | email | customer_name | address | phone | credit_card |
|---|---|---|---|---|---|
| 1 | amet.metus@Nullatinciduntneque.net | Ariel Hale | Ap #660-3260 Pellentesque St | 1-973-833-9836 | 5124442517412973 |
| 2 | sollicitudin@enimtempor.ca | Aubrey Norris | Ap #943-1347 Imperdiet Avenue | 07672 321093 | 5103960625359419 |
| 3 | Donec.non@dapibusrutrum.com | Bruno Hebert | 8566 Nisi Avenue | 02794 010514 | 513218470727440 |
| 4 | nec@lectus.net | Ira Lucas | 936-3011 Convallis Road | 1-117-676-2784 | 5164946381862809 |
| 5 | nec@orci luctuset.co.uk | Hannah Ferrell | P.O. Box 755, 7941 Aenean St | 1 (867) 533-2852 | 5256394502723692 |
| 6 | egestas@Sed eu eros.edu | May Bentley | P.O. Box 507, 7752 A Street | 1 (581) 785-4024 | 5553254253392933 |
| 7 | luctus. ut@eiusmod.com | Tario Sindh | P.O. Box 716, 3443 Metus Ave | +61 6777750639 | 5103138625325504 |

Figure 7.14 – Unmasked data in the electroniz lakehouse

在使其可供最终用户使用之前，不安全的数据需要在所有数据消费层中得到适当的屏蔽：

> **Important note**
> 为了保护个人隐私，**《通用数据保护条例》(GDPR)** 强制要求所有收集、存储和处理 PII 数据的组织必须对数据进行屏蔽。

## The process of curating raw data

到目前为止，希望大家能够很容易地想象到数据整理的需求是非常真实的。现在，让我们专注于实现这一目标的实际流程。

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_225_0.png)

## Inspecting data

数据整理的过程始于检查样本数据。通常，这是数据工程师和客户团队成员之间的共同努力。你可以先通过视觉方式检查涵盖多样化数据源的数据，尽管在许多情况下你可能需要实现编程逻辑来发现非标准化、无效、不一致、非统一、重复或不安全的数据。

交付物：详细报告，列出所有需要进行数据整理的情况，并包括修复每种情况的计划。在报告中，欢迎包含用于解决特定情况的业务逻辑的伪代码，如下所示：

```
IF raw_data.country IN ('USA', 'United States', 'United States of America') THEN
    curated_data.country = 'USA'
```

## Getting approval

在检查阶段创建的报告应正式提交给客户团队进行审查和批准。这确保客户同意将实施的业务逻辑来整理数据。

交付物：客户团队的正式批准。

## Cleaning data

一旦获得批准，就该开始实际实施实现数据整理的业务逻辑了。作为数据工程师，你可以自由选择多种框架、语言和工具来使用，尽管最好使用客户熟悉的东西。一般来说，最常用的两种是 Apache Spark 和 SQL。一旦整理逻辑开发完成，它就会集成到整理管道中。

**Deliverable:** Logic that implements the curation.

## Verifying data

数据整理的最后一步是验证整理逻辑是否按预期工作。通常，此活动应由客户团队成员执行，因为他们更接近数据，因此比数据工程师更有可能更好地理解数据。

**Deliverable:** Formal approval by the Customer Team.

> **Important note**
> 从一个项目到另一个项目，数据工程师学会识别需要整理的数据中的常见趋势。在某些情况下，立即开始修复数据很诱人，但同样重要的是遵循定义良好的整理流程。

现在我们对数据整理的需求有了充分的理解，并且对流程有了扎实的理解，让我们构建一个实际的整理管道。为简单起见，我们假设在遵循整理流程后，我们已根据 *Need for Curating Raw Data* 部分概述的发现向 Electroniz 提交了报告。Electroniz 团队已审查了报告并给予我们最终批准继续下一步。因此，我们将使用 Apache Spark 在 **Azure Databricks** 上开始开发整理逻辑。

## Developing a data curation pipeline

我们准备开始开发 Electroniz 整理管道的开发。数据现在将从未清洗的状态转变为更清洗和可用的状态。我之前承诺会让你了解架构图中的哪个区域正在被处理，这里就是：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_227_0.png)

Figure 7.16 – Electroniz curation pipeline

在下一节中，我们将创建前面图中突出显示的整理管道。

## Preparing Azure resources

我们将首先准备所需资源，如下所示：

+   1. 我们将首先在 Azure 数据湖存储中为 silver 层创建一个新的命名空间。

我之前提到过 Azure 中的存储账户名称在全球范围内是唯一的。在整个练习中，我们使用存储账户名称 traininglakehouse。你需要根据你创建的账户名称进行编辑：

```
SILVER_NAMESPACE="silver"
```

```
STORAGEACCOUNTNAME="traininglakehouse"
```

```
RESOURCEGROUPNAME="training_rg"
```

```
LOCATION="eastus"
az storage fs create -n $SILVER_NAMESPACE --account-name $STORAGEACCOUNTNAME --metadata project=lakehouse environment=development layer=bronze --only-show-errors
```

这会产生以下输出：

```
Azure> SILVER_NAMESPACE="silver"
Azure> STORAGEACCOUNTNAME="traininglakehouse"
Azure> RESOURCEGROUPNAME="training_rg"
Azure> LOCATION="eastus"
Azure> az storage fs create -n $SILVER_NAMESPACE --account-name
show-errors
{
  "client_request_id": "2235bb22-b42e-11eb-8c3c-0a580af46dd2",
  "date": "2021-05-13T20:59:39+00:00",
  "error_code": null,
  "etag": "\"0x8D916520671CA2A\"",
  "last_modified": "2021-05-13T20:59:40+00:00",
  "request_id": "1236c1f0-301e-0024-4f3a-48240f000000",
  "version": "2020-02-10"
}
```

Figure 7.17 – Azure data lake namespace for silver layer

如果上述命令成功，你应该能够通过浏览到 Home > All Resources > traininglakehouse 在 Azure 门户上看到新创建的容器：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_228_0.png)

Figure 7.18 – Silver namespace in traininglakehouse

+   2. 我们还需要一个 Azure Data Factory 用于此练习。你可能还记得我们在第 5 章《数据收集阶段 – Bronze 层》中之前创建了 traininglakehousedf。希望它仍然在你的 Azure 订阅中。如果不小心删除了它，你将需要按照 Build Building a batch ingestion pipeline 部分的步骤重新创建它。

+   3. 再次，我们将使用 Azure Databricks 来读取、执行整理并以 delta 格式写入数据。为了读取和写入数据，Azure Databricks 需要 Azure 数据湖存储的存储账户密钥。在 Azure Cloud Shell 上调用以下命令：

```
STORAGEACCOUNTNAME="traininglakehouse"
az storage account keys list --account-name $STORAGEACCOUNTNAME
```

如果上述命令按预期工作，你应该会看到两个存储密钥如下。请注意命令中 key1 的值。我们稍后将需要它用于 Databricks 工作区和 Data Factory 配置：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_229_0.png)

Figure 7.19 – traininglakehouse storage account keys

+   4. 为了整理数据并将其存储在 silver 层，我们将使用以下说明创建一个新的 Azure Databricks 工作区。在 Azure Cloud Shell 上调用以下命令。Azure Databricks 工作区名称在全球范围内是唯一的，因此请相应地编辑 WORKSPACE 名称：

```
RESOURCEGROUPNAME="training_rg"
LOCATION="eastus"
WORKSPACE="trainingdatabricks"
az config set extension.use_dynamic_install=yes_without_prompt
az databricks workspace create --resource-group $RESOURCEGROUPNAME --name $WORKSPACE --location $LOCATION --sku trial
```

