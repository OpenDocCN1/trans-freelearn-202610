

# 配置数据目标

一旦批处理和流式摄取管道被调用，它们将从数据源获取数据并将结果转储到数据目标。青铜层的 数据目标是 Azure Data Lake Storage Gen2：

+   1. 我们现在将使用 Azure 客户端创建一个 Azure Data Lake Storage 账户。逐行复制以下每个命令，并将其粘贴到 Cloud Shell 窗口中。然后，按 Enter：

```
STORAGEACCOUNTNAME="traininglakehouse"
RESOURCEGROUPNAME="training_rg"
LOCATION="eastus"
az storage account create --name $STORAGEACCOUNTNAME --resource-group $RESOURCEGROUPNAME --kind StorageV2 --location $LOCATION --hns true --sku Standard_LRS --tags owner=data engineering project=lakehouse environment=development
```

如果前面的命令成功，您应该会看到如下输出：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_137_0.png)

图 5.19 – Data Lake Storage 账户的创建输出

+   2. 您可以通过转到 Azure 门户来确认是否已创建 Azure Data Lake Storage 账户。

导航到 Home | All Resources | traininglakehouse：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_138_0.png)

图 5.20 – Azure 门户上的 Azure Data Lake Storage 账户

3. Azure Data Lake Storage 将以 blob 的形式存储数据。在 Azure Data Lake Storage 中，blob 组织在文件系统层次结构中。为了启用层次结构，我们需要为青铜层创建一个命名空间。逐行复制以下每个命令，并将其粘贴到 Cloud Shell 窗口中。然后，按 Enter：

```
NAMESPACE="bronze"
az storage fs create -n $NAMESPACE --account-name $STORAGEACCOUNTNAME --metadata project=lakehouse environment=development layer=bronze --only-show-errors
```

如果前面的命令成功，您应该会看到如下输出：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_138_1.png)

图 5.21 – Data Lake Storage 命名空间的创建输出

至此，我们已成功为 Electroniz 湖仓的青铜层准备了数据源和数据目标。在下一节中，我们将开始构建摄取管道，该管道将从源获取数据并将其存储到目标中。

## 构建摄取管道

您可能还记得，在之前的章节中我们决定创建两个摄取管道——批处理摄取和流式摄取。这两个管道中的每一个都将使用不同的 Azure 服务集构建。对于批处理摄取，我们将使用 Azure Data Factory，对于流式摄取，我们将使用 Azure Event Hubs Capture。因此，让我们开始吧，因为我们还有很长的路要走。

## 构建批处理摄取管道

在继续实际创建批处理管道之前，让我提醒您 Electroniz 湖仓的一个关键要求。之前，Electroniz 表示销售数据库和在线商店中的交易在一天中非常频繁地发生。他们希望湖仓能够在最多 1 小时的延迟内保持最新数据。

为了满足这一要求，我们需要使用水印方法来构建管道。简单来说，水印是一个可以用来标识上一次摄取运行中已摄取数据的最后一点的列。通常，水印列的选择是在自增键列或时间戳列之间：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_139_0.png)

图 5.22 – 水印在每次摄取运行时更新

批处理管道需要以能够加载旧水印和新水印之间更改数据的方式创建。在每次摄取运行时，将创建一个新的水印，该水印将用作下一次运行的标记：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_140_0.png)

图 5.23 – 用于增量数据摄取的水印方法

考虑到增量摄取方法，让我们开始构建批处理摄取管道：

+   1. 我们将从创建 Azure 数据工厂开始。浏览到 Azure 门户上的 Home。使用屏幕顶部的搜索框，搜索数据工厂。单击服务下的 Data factories：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_140_1.png)

图 5.24 – 在 Azure 门户上搜索 Data Factory 服务

+   2. 单击 Create data factory 按钮并输入以下内容：

-   Resource group: training_rg
-   Region: East US
-   Name: traininglakehousedf
-   Version: V2

然后，单击 Next: Git configuration：

-   Configure Git later: Checked
-   单击 Review + create。您应该会收到 Validation passed 消息。

最后，单击 Create。部署完成后，单击 Go to resource：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_141_0.png)

图 5.25 – Azure 门户上的 Data Factory 服务

3. 在管道创建过程中，其中一个步骤需要您连接到 REST API，该 API 将从互联网下载货币转换数据。为此，我们将使用一个免费服务。您可以从 https://free.currencyconverterapi.com/ 获取 API。选择 Get Your Free API 按钮。一旦收到电子邮件，请用它替换 API_KEY 中 XXXXXXXXXXX 的值。

此外，我们还将把 Azure Data Lake 账户密钥存储在 Azure Key Vault 的密钥管理器中。在 Azure Cloud Shell 上调用以下命令。请确保将 ADLS_KEY 的值替换为您之前获取的 key1 的值：

```
RESOURCEGROUPNAME="training_rg"
LOCATION="eastus"
KEYVAULTNAME="trainingkv100"
ADLS_SECRETNAME="ADLSKEY"
```

```
ADLS_KEY="XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
API_SECRETNAME="CURRAPIKEY"
API_KEY="XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
az config set extension.use_dynamic_install=yes_without_prompt
az keyvault create --location $LOCATION --name $KEYVAULTNAME --resource-group $RESOURCEGROUPNAME
az keyvault secret set --name $ADLS_SECRETNAME --vault-name $KEYVAULTNAME --value $ADLS_KEY
az keyvault secret set --name $API_SECRETNAME --vault-name $KEYVAULTNAME --value $API_KEY
```

这会产生以下输出：

```
Azure> RESOURCEGROUPNAME="training_rg"
Azure> LOCATION="eastus"
Azure> KEYVAULTNAME="trainingkv100"
Azure> SECRETNAME="ADLS_KEY"
Azure> ADLS_KEY="+QpX+DYXC6qJXcAIXmUsWk+WYhV+43WaqVM+Nxi9SO3UQSJ+VDwXi
Azure>
Azure> az config set extension.use_dynamic_install=yes_without_prompt
Command group 'config' is experimental and under development. Referenc
Azure>
Azure> az keyvault create --location $LOCATION --name $KEYVAULTNAME --
{\ Finished ..
"id": "/subscriptions/0b35426c-85f3-45e8-9f96-577a13b738d5/resourceG
"location": "eastus",
"name": "trainingkv100",
"properties": {
  "accessPolicies": [
    {
      "applicationId": null,
      "objectId": "39ea52c6-0ad8-4736-b35a-da3222e579d0",
      "permissions": {
        "certificates": [
          "get",
```

图 5.26 – Azure Key Vault 创建

+   4. 要验证 Azure key vault 的创建，请导航到 Azure 门户。
+   导航到 Home | All Resources | trainingkv100。
+   然后，单击 Secrets | ADLSKEY。现在单击 CURRENT VERSION 下方的条目。

单击 **Show Secret Value** 并验证它是否显示 Azure Data Lake Storage 密钥：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_143_0.png)

图 5.27 – 在 Azure Key Vault 中创建新密钥

记下 **secret identifier** 的值。您稍后需要它。

5. 最后，我们希望为 Azure Data Factory 提供读取密钥的访问权限。

导航到 **Home | All Resources | trainingkv100 | Access Policies.**

然后，单击 **Add Access Policy.**

对于 Key permissions，选择 **Get** 和 **List.**

对于 Secret permissions，选择 **Get** 和 **List.**

对于 Select Principal，单击 **None Selected.** 对于 **Principal**，查找您之前创建的 Azure 数据工厂的名称：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_143_1.png)

图 5.28 – 为新密钥的访问选择主体

