

## 设置多租户

在本章中，我们将使用命名空间 API 设置一个简单的多租户环境。此练习将帮助你了解如何以 QuickSight 管理员用户身份设置多租户环境。它还将帮助你了解你的 BI 用户在多租户场景下的体验，以及 QuickSight 如何防止用户与其他租户共享资产。

## 创建命名空间

创建命名空间很简单，可以使用 QuickSight API 完成。在撰写本文时，无法通过 QuickSight **图形用户界面 (GUI)** 创建命名空间：

1. 首先，你需要以 QuickSight 管理员用户身份登录 Amazon QuickSight。
2. 接下来，打开 **AWS 命令行界面 (CLI)**，并键入以下命令，将高亮显示的值替换为你环境中的值。对于此练习，我们将创建一个名为 `companyA` 的自定义命名空间。请注意，我们已经为本书中使用的现有 Admin、Author 和 Reader 用户有一个默认命名空间：

   ```bash
   $aws quicksight create-namespace --aws-account-id <account-id> --namespace companyA --identity-store QUICKSIGHT
   ```

   这将导致新命名空间的异步创建。请注意，在 CLI 响应中，状态为 `CREATING`：

   ```json
   {
    "Status": 202,
    "Name": "companyA",
    "CapacityRegion": "us-east-1",
    "CreationStatus": "CREATING",
    "IdentityStore": "QUICKSIGHT",
    "RequestId": "xxxxxx-xxxxx-xxxxxxxxxxxxxx"
   }
   ```

3. 要确认新命名空间已创建，我们将使用 `list-namespaces` CLI 命令。键入以下命令，将高亮显示的值替换为你环境中的值：

   ```bash
   $aws quicksight list-namespaces --aws-account-id <aws-account-id>
   ```

请注意响应。它应该如下所示：

```json
{
  "Status": 200,
  "Namespaces": [
    {
      "Name": "companyA",
      "Arn": "arn:aws:quicksight:us-east-1:<account-id>:namespace/companyA",
      "CapacityRegion": "us-east-1",
      "CreationStatus": "CREATED",
      "IdentityStore": "QUICKSIGHT"
    },
    {
      "Name": "default",
      "Arn": "arn:aws:quicksight:us-east-1:<account-id>:namespace/default",
      "CapacityRegion": "us-east-1",
      "CreationStatus": "CREATED",
      "IdentityStore": "QUICKSIGHT"
    }
  ],
  "RequestId": "xxxxxxx-xxx-xxx-xxx-xxx"
}
```

两个命名空间现在都已成功创建。

> **Note**
> 命名空间跨越 **AWS 区域**，无论用户使用哪个区域，都会被分配到同一个 QuickSight 命名空间。

现在我们已经创建了新命名空间，在下一节中，我们将把新用户分配到该命名空间。

## 使用 QuickSight 命名空间

在本节中，我们将添加用户，然后了解命名空间不允许用户与其他命名空间共享资产。在本节结束时，我们将学习如何在命名空间之间配置共用资产。

## 添加用户

现在我们有了新命名空间，将使用新命名空间注册新用户。  
对于此练习，我们将在 QuickSight 中注册一个新 IAM 用户：

1. 首先，使用 **AWS 控制台** 创建一个名为 `author-a` 的新 IAM 用户。选择 IAM，然后从左侧菜单中选择用户。

2. 选择 **添加用户**。在下一屏幕上，选择用户名，然后选中 **密码 – AWS 管理控制台访问** 复选框，如下图所示：

## 设置用户详细信息

你可以一次添加多个具有相同访问类型和权限的用户。了解更多

![](img/33bad1d211a2d93fd743a1978d403f9a_223_0.png)

## 选择 AWS 访问类型

选择这些用户主要如何访问 AWS。如果你只选择编程访问，它并 **不会** 防止用户使用假设角色访问控制台。访问密钥和自动生成的密码将在最后一步提供。了解更多

![](img/33bad1d211a2d93fd743a1978d403f9a_223_1.png)

图 10.2 – 创建 IAM 用户

3. 在下一屏幕上，保持权限为空并单击 **创建**。请记下用户 arn 值，我们将在下一步中使用它。

4. 现在我们已经创建了新用户，将在 `companyA` 命名空间下将其注册到 QuickSight。使用 CLI，键入以下命令，将高亮显示的值替换为你环境中的值：

```bash
$aws quicksight register-user --identity-type IAM \
--email <your-email> --user-role AUTHOR --iam-arn arn:aws:iam::<aws-account-id>:user/author-a \
--aws-account-id <aws-account-id> --namespace companyA
```

要确认用户创建，我们将使用 `list-users` CLI 命令。请注意，我们将 `companyA` 指定为命名空间：

```bash
$aws quicksight list-users --aws-account-id <aws-account-id> --namespace companyA
```

响应将如下所示：

```json
{
  "Status": 200,
  "UserList": [
    {
      "Arn": "arn:aws:quicksight:us-east-1:xxxxxxxxxxxx:user/companyA/author-a",
      "UserName": "author-a",
      "Email": "xxx@xxx",
      "Role": "AUTHOR",
      "IdentityType": "IAM",
      "Active": true,
      "PrincipalId": "federated/iam/xxxxx"
    }
  ],
  "RequestId": "xxxx-xxxx-xxxx-xxxx"
}
```

5. 现在我们已成功创建用户，让我们尝试了解命名空间的影响。打开 AWS 控制台，以新创建的用户身份登录 QuickSight。展开右上角菜单。请注意命名空间名称，如下图所示：

![](img/33bad1d211a2d93fd743a1978d403f9a_224_0.png)

图 10.3 – 登录多租户 QuickSight 账户

6. 接下来，我们要确保此用户无法将其仪表板、分析或数据集与其他命名空间的用户共享。检查这一点的最简单方法是快速创建一个新数据集、分析或仪表板，并尝试与其他用户共享。一旦资产创建完成，打开其权限管理器，尝试邀请一个属于不同命名空间的 Author 或 Reader 用户：

# 管理数据集权限

你可以管理访问此数据集的用户、分析和仪表板的设置和权限。

![](img/33bad1d211a2d93fd743a1978d403f9a_225_0.png)

已与 1 个用户共享

| 名称 | 权限 | 操作 |
|---|---|---|
| author-a | Owner | |
| 关闭 | | 邀请用户 |

图 10.4 – 在多租户环境中共享资产

共享资产时，QuickSight 在找到相关用户时会自动补全。你会注意到，当你输入 `author` 时，QuickSight 没有任何建议，且无法共享数据集。原因是 Author 用户属于与 `author-a` 用户不同的命名空间。

现在我们已经了解命名空间如何防止用户与其他用户共享资产，我们将了解如何在不同命名空间的用户之间配置共用资产。

## 共享共用资产

可能存在需要让不同命名空间的用户可见的资产的用例。例如，一家大型公司可能希望将特定仪表板与属于不同部门（配置为命名空间）的用户共享。要完成此操作，我们需要使用 QuickSight API。如前一节所学，通过 QuickSight 控制台无法完成此操作，因为一个命名空间的用户无法发现另一个命名空间的用户。

为了更好地理解此概念，我们将使用第 4 章“开发可视化和仪表板”中配置的纽约出租车仪表板。对于本教程，我们将使用 QuickSight 管理员用户和 AWS CLI：

1. 首先，打开安装了 AWS CLI 的终端。

2. 使用 list-dashboard 操作列出仪表板。键入以下命令，将高亮显示的值替换为你环境中的值：

   ```bash
   $aws quicksight list-dashboards --aws-account-id <aws-account-id>
   ```

   关注 DashboardSummaryList 数组，并捕获 DashboardId 值，我们将在下一步中使用它：

   ```json
   "DashboardSummaryList": [
       {
           "Arn": "arn:aws:quicksight:us-east-1:xxxxxxxxxx:dashboard/xxxx-xxxx-xxxx-xxxx-xxxxxx",
           "DashboardId": "xxxx-xxxx-xxxx-xxxx-xxxx",
           "Name": "New York Taxi Dashboard",
           ...
       }
   ]
   ```

3. 要更新仪表板权限，我们将使用 update-dashboard-permissions CLI 命令。键入以下命令，将高亮显示的值替换为你环境中的值：

   ```bash
   $aws quicksight update-dashboard-permissions --aws-account-id <aws-account-id> --dashboard-id <dashboard-id> --grant-permissions Actions=quicksight:DescribeDashboard,quicksight:ListDashboardVersions,quicksight:UpdateDashboardPermissions,quicksight:QueryDashboard,quicksight:UpdateDashboard,quicksight:DeleteDashboard,quicksight:DescribeDashboardPermissions,quicksight:UpdateDashboardPublishedVersion, Principal=arn:aws:quicksight:us-east-1:<aws-account-id>:user/companyA/author-a
   ```

现在我们已为 author-a 用户提供了访问权限，我们将登录 QuickSight 控制台并查看可用仪表板。

4. 以 author-a 用户身份登录 QuickSight 控制台。然后，导航到仪表板以查看可用仪表板。纽约出租车仪表板现在应可供你查看，如下图所示：

