

## 使用 QuickSight API 控制资源

在本节中，我们将学习如何使用 QuickSight API 控制 QuickSight 资源。本节并非旨在成为 QuickSight API 的完整参考指南，也不会涵盖使用 API 可以完成的每一种可能操作。有关 QuickSight API 的完整参考指南，请参阅 QuickSight 文档：
https://docs.aws.amazon.com/quicksight/latest/APIReference/Welcome.html

本节将为您提供使用 API 控制 QuickSight 资源的实际操作示例。这些示例将使您对可以使用 API 完成的活动类型有扎实的了解，进而帮助您管理 QuickSight 环境。
我们的第一个示例将是学习如何使用 QuickSight API 设置数据源。

## 使用 CLI 设置数据集

在第 2 章“Amazon QuickSight 简介”中，我们学习了如何创建数据源和数据集。要创建这些资源，我们通过网络浏览器访问了 QuickSight GUI。虽然 GUI 很用户友好，但我们可以使用 API 以编程方式创建数据集。当我们需要确保特定配置的一致性时，这可能是一个不错的选择，因为手动配置容易出错。让我们从第 1 章“AWS 分析生态系统简介”中创建的数据仓库创建一个数据源。

> Note
> 第 1 章“AWS 分析生态系统简介”中的教程包括创建一个示例 Amazon Redshift 集群，这将产生费用。对于本教程，您不需要集群正在运行。暂停的集群对于此示例来说效果很好，因为我们只需要配置数据集，并且不需要与其建立实时连接。

现在，让我们使用 CLI 创建一个 Redshift 数据源。我们将使用 create-data-source CLI 命令来创建 Redshift 数据源：
https://docs.aws.amazon.com/cli/latest/reference/quicksight/create-data-source.html

以下是我们需要配置的几个参数：

- AwsAccountId：此值表示您的 AWS 数字账户 ID。
- DataSourceId 和 Name：您需要定义一个唯一的账户和区域标识符以及一个用户友好的名称来描述您的数据源。
- Type：这是数据源的类型。对于 Redshift 数据源，这需要设置为 REDSHIFT。
- DataSourceParameters：数据源参数的类型将取决于您配置的数据源类型。对于 Redshift 数据源，您需要定义主机名、端口号、Redshift 数据库名称和集群标识符。
- Credentials：这些是 QuickSight 访问数据存储所需的凭据。对于 Redshift 数据源，您可以使用由用户名和密码组成的凭据对。
- Permissions：在本节中，您需要定义新创建数据集的权限，并确定哪个用户可以访问它以及他们拥有什么级别的访问权限。对于我们的示例，我们希望我们的读者用户能够从新创建的数据源创建数据集，但我们也希望限制该用户删除或更新数据源。

现在，让我们看看如何使用 QuickSight CLI 配置先前描述的 Redshift 连接：

1. 首先，打开文本编辑器并复制以下 JSON 配置。用您环境中的值替换高亮显示的值。对于权限，请注意，我们仅授予 PassDataSource、DescribeDataSource 和 DescribeDataSourcePermissions 操作的权限，这将允许此用户使用此数据源设置新数据集，但不会允许他们更新或删除数据源：

```json
{
  "AwsAccountId": "<account id>",
  "DataSourceId": "RedshiftDatasouceCLI",
  "Name": "CLI Datasouce",
  "Type": "REDSHIFT",
  "DataSourceParameters": {
    "RedshiftParameters": {
      "Host": "hostname",
      "Port": 5439,
      "Database": "dev",
      "ClusterId": "mycluster"
    }
  },
  "Credentials": {
    "CredentialPair": {
      "Username": "admin",
      "Password": "R3dsh1ft"
    }
  },
  "Permissions": [
    {
      "Principal": "arn:aws:quicksight:us-east-1:xxxxxxxxxxxx:user/default/author",
      "Actions": [
        "quicksight:DescribeDataSource",
        "quicksight:DescribeDataSourcePermissions",
        "quicksight:PassDataSource"
      ]
    }
  ]
}
```

2. 将文件另存为 create-data-source.json。
3. 运行 `aws quicksight create-data-source --cli-input-json file://create-data-source.json`。

此命令的结果将如下所示：

```json
{
  "Status": 202,
  "DataSourceId": "SampleRedshiftDatasouce",
  "RequestId": "xxxxxx-xxxxxx-xxxxxx-xxxxxx-xxxxxxxxxx",
  "CreationStatus": "CREATION_IN_PROGRESS",
  "Arn": "arn:aws:quicksight:us-east-1:xxxxxxxxxxxx:datasource/SampleRedshiftDatasouce"
}
```

现在我们已配置新数据源，让我们验证 Author 用户是否能够使用它来设置新数据集：

1. 对于本教程，我们可以通过以 `author` 用户身份登录 QuickSight 来简单验证新创建的数据源。
2. 然后，导航到 **Datasets**，然后单击 **New Datasets**。您应该能够在 **FROM EXISTING DATA SOURCES** 菜单下找到新创建的数据源，如下图所示：

![](img/33bad1d211a2d93fd743a1978d403f9a_176_0.png)

Figure 8.3 – 使用 CLI 创建数据源

3. 单击数据源。请注意，未显示删除或编辑数据源的按钮，因为该用户没有更新或删除此数据源所需的权限：

![](img/33bad1d211a2d93fd743a1978d403f9a_176_1.png)

Figure 8.4 – 示例 CLI 数据源权限

现在我们已学习如何配置数据源，我们将使用 QuickSight CLI 更改环境的 QuickSight 设置。

## 使用 QuickSight API 编辑账户设置

在本节中，我们将使用 QuickSight API 编辑账户上的设置。这允许我们在账户上配置账户自定义设置。对于我们的示例，我们将更改 QuickSight 账户的默认主题。

1. 首先，让我们使用 QuickSight API 检索全局 QuickSight 账户详细信息。使用 CLI 作为 QuickSight 或 AWS 管理员，键入以下命令，将 `aws-account-id` 值替换为您环境中的值：

```bash
$aws quicksight describe-account-settings --aws-account-id <account-id>
```

响应应如下所示：

```json
{
    "Status": 200,
    "AccountSettings": {
        "DefaultNamespace": "default",
        "Edition": "ENTERPRISE",
        "NotificationEmail": "your-email",
        "AccountName": "quicksight-account-name"
    },
    "RequestId": "xxxxxxxxxxxxxxxxxxxxxx"
}
```

2. 接下来，让我们键入以下命令以获取 QuickSight 账户自定义设置。账户自定义设置是指可以为 QuickSight 账户参数化的一组设置。目前，您可以通过账户自定义设置添加自定义默认主题：

```bash
$aws quicksight describe-account-customization --aws-account-id <account-id>
```

由于我们尚未添加任何自定义设置，响应很可能如下所示：

```
An error occurred (ResourceNotFoundException) when calling the DescribeAccountCustomization operation: Account customization does not exist for AwsAccountId <account-id>
```

3. 现在，让我们添加一个账户自定义设置并将 Midnight 设置为默认主题。使用 CLI 键入以下命令，将 aws-account-id 值替换为您的账户 ID 值：

```bash
aws quicksight create-account-customization \
--aws-account-id <account-id> --account-customization \
DefaultTheme=arn:aws:quicksight::aws:theme/MIDNIGHT \
--region=us-east-1
```

4. 现在，让我们通过再次运行步骤 2 中的 describe-account-customization 命令来确认新账户自定义设置的创建。这次，响应应如下所示：

```json
{
 "Status": 200,
 "RequestId": "xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
 "AccountCustomization": {
 "DefaultTheme": "arn:aws:quicksight::aws:theme/MIDNIGHT"
 },
 "Arn": "arn:aws:quicksight:us-east-1:xxxxxxxxxxxx:customization/account/xxxxxxxxxxxx ",
 "AwsAccountId": "xxxxxxxxxxxx"
}
```

> Note
> 账户自定义设置适用于账户和 **AWS 区域** 级别。

要验证自定义设置是否已应用于此账户，您可以在应用账户自定义设置的同一区域的账户上创建示例分析。

现在我们已学习如何使用 CLI 编辑账户设置，在下一节中，我们将学习如何使用模板 API，这在开发跨多个环境的分析和仪表板时很有用。

