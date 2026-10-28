

> **注意**
>
> 请注意，前面的命令并不属于我们稍后将要演示的动手练习。不过，如果你有一个想尝试运行这些命令的 Azure 环境，可以在 Azure Cloud Shell 中运行它们。

需要注意的是，动态分配的地址可能会发生变化；如果稍后再次进行测试，这些地址可能已经不再分配给你正在参与测试的组织。在这些情况下，针对 DNS 主机名可能更为合适，因为 IP 地址发生变化时，DNS 主机名会自动更新。这有助于我们避免扫描其他 Azure 客户的 IP 地址。此外，我们建议在收集到公共 IP 后立即执行任何公共 IP 扫描。

出于学习目的，我们将向你展示外部攻击者如何利用公开可用的信息匿名发现 Azure 目标。Microsoft Azure 网络团队发布了一个 JSON 文件，其中包含所有 Azure 区域和公共服务的公共 IP 地址范围。

该文件也会随着新地址范围的添加而每周更新。攻击者获取此列表并使用 **JQ** 等 JSON 工具对其进行解析是很常见的。随后，可以使用 **Nmap** 等工具扫描这些 IP 地址范围，以确定攻击面。我们将在下一组练习中模拟这一过程。

> **重要注意**
>
> Microsoft 强烈不建议扫描未分配给你的 Azure 订阅的公共 IP 地址范围，或扫描未经所有者授权的地址范围。下一项练习仅用于教育目的。**未经分配所有者的书面法律许可，请勿扫描这些地址！**

## 动手练习 – 使用 PowerShell 解析 Azure 公共 IP 地址

本练习将引导你了解外部攻击者如何利用已发布的 Azure 公共 IP 地址范围，识别潜在目标并扫描其漏洞。

以下是本练习中将完成的任务：

- **任务 1：** 在我们的渗透测试虚拟机上下载已发布的 Azure 公共 IP 地址范围 JSON 文件。
- **任务 2：** 使用 PowerShell 解析下载的文件，以识别潜在目标。

让我们开始：

1. 在渗透测试虚拟机中打开 Web 浏览器，并访问 `https://www.microsoft.com/en-us/download/details.aspx?id=56519`（也可以使用这个短网址：`http://bit.ly/azureipranges`）。

2. 单击 **Download** 按钮，以获取当前的 Azure 公共 IP 地址范围列表。这将下载一个 JSON 文件，其中包含按区域和服务划分的公共 IP 地址范围列表。

3. 为了演示方便，我们不会使用下载的 JSON 文件，而是使用命令行会话中显示的链接来下载该文件：

   ![](img/4fa338e84e0a590967a142dd798ba4e2_97_0.png)

   图 3.2 – 下载 JSON 文件

4. 在显示的窗口中，右键单击 **click here to download manually**，然后单击 **Copy link address**，以获取 JSON 文件的 URL：

   ![](img/4fa338e84e0a590967a142dd798ba4e2_97_1.png)

   图 3.3 – 获取 JSON 文件的 URL

> **注意**
>
> 我们在此处下载的 JSON 文件适用于 Azure 公有云。Azure 美国政府云有一个单独的文件，可从 `https://www.microsoft.com/en-us/download/details.aspx?id=57063` 下载。

5. 在渗透测试虚拟机的 PowerShell 控制台中，使用以下命令下载 JSON 文件。将 `<json_url>` 替换为你在*步骤 3*中获取的 URL：

   ```
   cd C:\Users\$env:USERNAME\
   Invoke-WebRequest <json_url> -OutFile azure_ip_range.json
   ```

6. 在本示例中，我们将使用 PowerShell 筛选 Azure 英国南部区域中所有服务的公共 IP 地址范围。这样会返回一个 IP 地址范围列表，攻击者可以扫描这些地址，以确定开放端口以及这些端口上正在监听的服务：

   ```
   $jsonData = gc .\azure_ip_range.json | ConvertFrom-Json
   ($jsonData | select -ExpandProperty values | where name -EQ AzureCloud.uksouth).properties.addressPrefixes
   ```

7. 接下来，尝试筛选英国南部区域中特定服务（Azure App Service 实例）所使用的公共 IP 地址范围。攻击者可以获取这些 IP 地址范围并扫描其中的 Web 应用程序漏洞，因为 Azure App Service 资源通常用于托管 Web 应用程序和 API：

   ```
   ($jsonData | select -ExpandProperty values | where name -EQ AppService.UKSouth).properties.addressPrefixes
   ```

8. 攻击者可以获取这些 IP 地址范围，并扫描开放端口和正在监听的服务，以了解攻击面。使用*步骤 6 和 7*中的示例，尝试执行其他查询，以识别其他 Azure 服务和区域的公共 IP 地址范围。

> **重要注意**
>
> 为了“让一切正常工作”，一些 Azure 管理员或工程师可能会开放分配给不同 Azure 区域的较大 IP 地址范围，以允许虚拟机相互通信。这也可能使你能够从正确区域中的虚拟机访问相同的系统。如果你已通过身份验证访问 Azure 环境，请留意网络安全组规则中的 Azure IP 地址范围，因为你可能可以加以利用。

此外，也可以使用此列表验证你已经在定位的主机是否为 Azure 资源。只需将目标 IP 地址与此列表中的地址范围进行比较，即可识别托管在 Azure 上的资源。本章稍后将使用 Cloud IP Checker 工具演示如何执行此操作。

## Azure 平台 DNS 后缀

如*第 1 章“Azure 平台和体系结构概述”*所述，许多 Azure 平台服务使用由 Microsoft 所有和管理的公共 DNS 后缀。当 Azure 客户创建资源实例时，Azure 会按照 `<resource instance name>.<service dns suffix name>` 的格式，为其分配关联 DNS 后缀的一个子域。

例如，Azure Blob 存储服务的公共 DNS 后缀是 `blob.core.windows.net`。如果 Azure 客户创建了一个名为 `azurepentesting` 的存储帐户实例，则该服务实例的**完全限定域名（FQDN）**将是 `azurepentesting.blob.core.windows.net`。

还需要注意的是，有些 DNS 后缀是区域性的，而另一些则是全局性的。例如，如果客户在 Azure 英国南部区域创建一个带有关联 DNS 标签的公共 IP 资源，则该资源的 FQDN 将是 `<dns label>.<region>.cloudapp.azure.com`。

> **重要注意**
>
> Microsoft 确实维护了一份*不完整的* Azure 域名列表。除了本章提到的域名外，该列表还包含一些其他域名。有关更多信息，请参阅 https://docs.microsoft.com/en-us/azure/security/fundamentals/azure-domains。

虽然该列表会持续更新且并不完整，但下面列出了一些常见的 DNS 域名及其关联服务：

| DNS 后缀 | 关联的 Azure 服务 |
| --- | --- |
| file.core.windows.net | 存储帐户 – 文件 |
| blob.core.windows.net | 存储帐户 – Blob |
| azurewebsites.net | App Services 和 Functions 应用 |
| scm.azurewebsites.net | App Services – 管理 |
| database.windows.net | 数据库 – MSSQL |
| documents.azure.com | 数据库 – Cosmos DB |
| cloudapp.azure.com | 客户分配的公共 IP DNS |
| vault.azure.net | Key Vault |
| azurecontainer.io | 容器实例 |
| azurecr.io | 容器注册表 |

那么，这些信息与匿名枚举 Azure 服务有什么关系？作为渗透测试人员，你可以使用 DNS 解析或暴力破解技术枚举这些子域上的主机，从而发现 Azure 客户部署的活动资源实例。

> **重要注意**
>
> 需要牢记的是，在进行匿名测试时，*名称并不一定能证明资源的所有权*。例如，你可能会看到一个类似 `https://aws.blob.core.windows.net` 的 Azure Blob 存储 URL，并认为它属于**亚马逊云科技（AWS）**。但在本文撰写时，它实际上是一个注册在作者 Azure 租户中的存储帐户。我们使用它来存放 Azure 写作示例，因此它与亚马逊没有任何关系。除了一些与 Microsoft 相关的关键词外，Azure 平台不会执行资源名称完整性验证。如果某个名称当前尚未被其他人使用，任何拥有有效 Azure 订阅的人都可以创建一个名为 `departmentofdefense.azurecr.io` 的容器注册表，没有任何机制可以阻止这一点。

要匿名枚举 Azure 中的平台服务，渗透测试人员可以采用以下方法：

1. 确定要使用的基础词搜索词。这些搜索词通常与正在参与测试的 Azure 客户名称，或与该组织相关的已知术语有关；例如，`packt`、`azurepentesting`、`azurept` 等。
2. 根据基础词创建排列组合，以识别潜在的子域名；例如，`packt-prod`、`packt-dev`、`azurepentesting-stage`、`azurept-qa` 等。

