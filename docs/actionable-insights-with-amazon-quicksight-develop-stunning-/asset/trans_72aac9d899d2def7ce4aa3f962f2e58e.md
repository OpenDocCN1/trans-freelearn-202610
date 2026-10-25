

## 技术要求

本章需要以下内容：

- 一个 AWS 账户。
- 一个配置了 Author 用户的 QuickSight 账户。
- 对于本章的某些部分，我们将使用在 _第 1 章，AWS 分析生态系统简介_ 中创建的架构。

## 添加 QuickSight 数据源

在本节中，我们将学习如何使用 Amazon QuickSight 添加和配置数据源。QuickSight 支持多种数据源。我们将首先列出支持的数据源。

## QuickSight 支持的数据源

让我们来看看可以在 Amazon QuickSight 中配置的不同类型的数据源。BI 工具的一个非常常见的场景是从分析数据库或数据仓库驱动可视化。具体来说，QuickSight 支持以下 **数据仓库**：

- 带自动发现的 Redshift
- Redshift 手动连接
- Snowflake
- Teradata

> **注意**
> Redshift 手动连接的一个用例是当 QuickSight 和 Redshift 集群位于不同的 AWS 区域时。

其他支持的 **关系型数据库系统** 包括以下内容：

- RDS
- MySQL
- PostgreSQL
- Oracle
- Amazon Aurora
- MariaDB

QuickSight 还支持以下内容：

- Amazon Timestream
- Amazon Elasticsearch Service

两者都是 **时间序列数据集** 的流行数据存储。

对于某些用例，用户可能需要处理 **单个文件**，例如 Microsoft Excel 文件 (`.xls`)。Amazon QuickSight 支持文件作为数据源，可以通过直接上传或通过指向 S3 上一个或多个文件位置的清单文件来实现。

QuickSight 还支持流行的 **大数据技术**；具体来说，包括以下内容：

- Amazon Athena
- Presto
- Spark

另一类是来自 **API** 和流行 SaaS 应用程序的数据源；具体来说，包括以下内容：

- Salesforce
- GitHub
- Twitter
- Jira
- ServiceNow
- Adobe Analytics

最后，QuickSight 支持 **AWS IoT Analytics** 来可视化 **物联网 (IoT)** 数据，并与 **S3 Analytics** 集成以可视化 S3 存储桶上的访问模式。

现在我们已经了解了数据源选项，在下一节中，我们将使用 **自动发现** 功能配置我们的第一个数据源。

## 配置我们的第一个数据源

在本节中，我们将配置我们的前两个数据源，连接我们在 _第 1 章，AWS 分析生态系统简介_ 中构建的架构。

在 QuickSight 中设置数据源之前，我们需要确保 QuickSight 服务有通往我们 Redshift 集群的网络路由。由于我们使用默认设置，我们的集群默认处于锁定状态。我们可以通过打开其安全组来确认这一点。安全组充当虚拟防火墙，用于控制入站和出站流量。默认安全组不会允许任何入站或出站流量，除非流量来自其自身（一个 **自引用安全组**）。因此，如果不采取任何操作，Amazon QuickSight 将无法访问 Redshift 集群。为了允许 QuickSight 访问我们的 Redshift 集群，我们需要编辑 Redshift 集群的安全组，并允许来自 Amazon QuickSight IP 地址范围的入站流量。

在下一节中，我们将提供启用 QuickSight 访问我们 Redshift 集群的分步指南。

## 启用对 Redshift 集群的访问

让我们开始：

1. 首先，让我们获取我们 Redshift 集群的安全组：

   ```bash
   aws redshift describe-clusters --cluster-identifier mycluster --region us-east-1
   ```

   AWS CLI 响应将包含有关集群的大量信息。安全组 ID 可以在 API 响应的 _VpcSecurityGroups_ 键下找到：

   ```json
   "VpcSecurityGroups": [
       {
           "Status": "active",
           "VpcSecurityGroupId": "sg-xxxxxxx"
       }
   ]
   ```

   或者，我们可以轻松地从 Redshift 控制台找到此信息。导航到 Redshift 控制台，选择您的 Redshift 集群 (**mycluster**) 然后选择 **Properties** 选项卡。安全信息位于 **Network and security settings** 下，因此展开此选项。

## 网络和安全设置

| 虚拟私有云 (VPC) | 可用区 | VPC 安全组 | 公开可访问 |
|---|---|---|---|
| default | us-east-1f | 指定哪些实例和设备可以连接到集群。 | 允许 VPC 外部的实例和设备通过集群端点连接到您的数据库。 |
| default | 增强型 VPC 路由 已禁用 | | 已启用 |
| 端点 URL | | | |

图 3.1 – Redshift 网络和安全设置选项卡

2. 现在我们有了 VPC 安全组 ID，让我们检查其内容以验证它是否不允许对 Amazon QuickSight 的入站访问。单击安全组 ID 以打开 EC2 控制台并观察其规则。在 EC2 控制台中，单击安全组 ID 然后选择入站规则。您应该能够看到一个自引用入站规则，但没有其他规则。Amazon QuickSight 不是此安全组的成员；因此，此阶段无法访问。

3. 现在我们已经验证了入站规则，我们将添加对 Amazon QuickSight 的入站访问。为此，请选择 **Edit inbound rules** 以添加新规则。从下拉菜单中选择 **Redshift** 作为规则类型。对于源，我们需要添加 QuickSight 服务 IP 地址空间。为此，我们需要查阅 AWS 文档 `https://docs.aws.amazon.com/quicksight/latest/user/regions.html` 并找到我们正在使用的区域的 IP 地址空间。在我们的示例中，我们使用的是 **N. Virginia**。该区域的 QuickSight IP 地址范围是 `52.23.63.224/27`。将此 IP 范围添加为源。最好为此规则添加描述，以帮助您记住此配置的目的，例如 `QuickSight Access to Redshift`。

4. 验证值如下：

   - **类型**: `Redshift`
   - **协议**: `TCP`
   - **端口范围**: `5439`
   - **源**: `Custom – 52.23.63.224/27`
   - **描述**: `QuickSight Access to Redshift`

5. 验证这些值后，单击 **Save rules**。

现在我们已经配置了 QuickSight 和 Redshift 之间的网络访问，在下一节中，我们将设置我们的第一个 Redshift 数据源。

## 设置 Redshift 数据源

在本节中，我们将设置我们的 Redshift 数据源。对于此步骤，我们需要使用在 _第 2 章，Amazon QuickSight 简介_ 中创建的 Author 用户访问 Amazon QuickSight：

1. 首先，以 **Author** 用户身份登录 Amazon QuickSight。登录后，导航到左侧菜单中的 **Datasets** 并选择 **New Dataset**。

2. 在下一屏幕中，您将看到支持的 QuickSight 数据源列表。选择 **Redshift (Auto-discovered)**。

   ![](img/33bad1d211a2d93fd743a1978d403f9a_64_0.png)

   图 3.2 – QuickSight 数据源

3. 在以下屏幕中，添加 Redshift 数据源所需的信息。您应该能够从 **Instance ID** 下拉菜单中选择您的集群。

## 新建 Redshift 数据源

### 数据源名称
Redshift - Taxi Data

### 实例 ID
选择集群 ID

### 连接类型
公共网络

### 数据库名称
dev

### 用户名
admin

### 密码
********

验证连接    已启用 SSL    创建数据源

图 3.3 – 设置 Redshift 数据源

4. 表单完成后，单击 **Create data source**。

> **注意**
> **自动发现** 选项查找与 QuickSight 位于同一区域的集群。
> 要验证 QuickSight 区域或更改为所需区域，请导航到屏幕右上角，单击用户名旁边的下拉图标以显示区域设置。

