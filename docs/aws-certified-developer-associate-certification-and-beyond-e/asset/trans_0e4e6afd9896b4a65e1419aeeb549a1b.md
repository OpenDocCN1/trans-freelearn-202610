

+   4. **AWS Secrets Manager：** 你将在 AWS Secrets Manager 中配置一个机密，以存储数据库凭据详细信息，这些详细信息将通过环境变量访问你的应用程序 Elastic Beanstalk 环境。这是一种比在应用程序中硬编码凭据更加安全的方法。
+   5. **IAM 角色和策略：** 通常，你还需要配置 IAM 角色，以启用 Elastic Beanstalk 服务接收机密并在你的 VPC 中置备 EC2 实例。作为 Elastic Beanstalk 配置的一部分，你将置备一个自动伸缩组和启动配置——同样，需要相应的 IAM 角色。
+   6. **Elastic Beanstalk 应用程序：** 在所有基础设施组件配置完成后，你可以构建你的 Elastic Beanstalk 应用程序和环境。在这里，你将提供配置详细信息，以便 Elastic Beanstalk 能够置备你的自动伸缩组和自动伸缩策略，置备负载均衡器，并部署你的应用程序。

## 技术要求

要完成本章的项目任务，你需要执行以下操作：

- 你必须以 IAM 用户“Alice”的身份登录到 AWS 开发账户。请确保你位于伦敦区域（eu-west-2）。
- 你必须下载应用程序源代码并将其保存在桌面上以便轻松访问。源代码可在 https://packt.link/pdy6D 获取。

## 项目任务 10.1 – 创建自定义 VPC

在这个项目任务中，你将构建一个自定义 VPC 来托管你的应用程序，并配置 Elastic Beanstalk 以部署它。虽然可以获取让 Elastic Beanstalk 为你构建 VPC，但请记住，你想先在你的 VPC 中部署 RDS 数据库，因此需要将 VPC 的构建作为配置 Elastic Beanstalk 应用程序的先决条件。

+   1. 以 IAM 用户“Alice”的身份登录到 AWS 开发账户。
+   2. 从主控制台网页使用页面顶部的搜索栏搜索并导航到 VPC 管理控制台。
+   3. 接下来，通过从控制台左上角的区域下拉列表中选择伦敦区域，确保你位于伦敦区域。
+   4. 从左侧菜单中，点击“Your VPCs”，然后点击控制台右上角的“Create VPC”按钮。
+   5. 在“VPC settings”下的“Resources to create”部分选择“VPC and more options”。
+   6. 取消勾选 **Auto-generate** 旁边的复选框。
+   7. 对于 **IPv4 CIDR block** 范围，提供 `10.5.0.0/16` 的 CIDR 块。
+   8. 接下来，根据以下截图确保选中 **No IPv6 CIDR block**：

![](img/34ff9b5b5a734dae3450ab701e223c73_415_0.png)

图 10.9：VPC 设置

+   9. 展开“Number of Availability Zones (AZs)”部分下的 **Customize AZs**。
+   10. 确保选中 `eu-west-2a` 和 `eu-west-2b` 作为此 VPC 使用的可用区。
+   11. 确保“Number of public subnets”选项选中 2，“Number of private subnets”选项选中 4。
+   12. 展开 **Customize subnets CIDR blocks** 子部分，并提供以下 IP CIDR 块：

+   I. `eu-west-2a` 中的公共子网 CIDR 块 - `10.5.1.0/24`。这将标记为 `PublicSubnet01`。
+   II. `eu-west-2b` 中的公共子网 CIDR 块 - `10.5.2.0/24`。这将标记为 `PublicSubnet02`。
+   III. `eu-west-2a` 中的私有子网 CIDR 块 - `10.5.10.0/24`。这将标记为 `AppSubnet01`。
+   IV. `eu-west-2b` 中的私有子网 CIDR 块 - `10.5.11.0/24`。这将标记为 `AppSubnet02`。
+   V. `eu-west-2a` 中的私有子网 CIDR 块 - `10.5.20.0/24`。这将标记为 `DataSubnet01`。
+   VI. `eu-west-2b` 中的私有子网 CIDR 块 - `10.5.21.0/24`。这将标记为 `DataSubnet02`。
+   VII. 查看以下截图，确保你已正确配置子网 CIDR 块：

# 自定义子网 CIDR 块

eu-west-2a 中的公共子网 CIDR 块

10.5.1.0/24
256 个 IP

eu-west-2b 中的公共子网 CIDR 块

10.5.2.0/24
256 个 IP

eu-west-2a 中的私有子网 CIDR 块

10.5.10.0/24
256 个 IP

eu-west-2b 中的私有子网 CIDR 块

10.5.11.0/24
256 个 IP

eu-west-2a 中的私有子网 CIDR 块

10.5.20.0/24
256 个 IP

eu-west-2b 中的私有子网 CIDR 块

10.5.21.0/24
256 个 IP

图 10.10：VPC 子网 CIDR 块

+   13. 继续向下滚动，确保你为 NAT 网关（$）配置选择了 1 AZ 中的 1 个。你将只为该项目部署单个 NAT 网关。请注意，NAT 网关是收费服务，你应该在完成并测试该项目后终止这些资源。
+   14. 确保在 VPC endpoints 下选择 None。
+   15. 滚动到页面顶部，查看右侧窗格中的 Preview 部分。为了能够轻松识别各个子网，手动编辑各个文本框，根据以下截图标记它们。

![](img/34ff9b5b5a734dae3450ab701e223c73_417_0.png)

图 10.11：VPC 架构可视化预览

以下是用于你的配置的具体名称：

+   I. 如前面截图所示，在 VPC 部分下的文本框中为你的 VPC 提供一个名称。你可以使用诸如 Todo-List-EB-VPC 的名称。
+   II. 你会注意到前面截图中六个子网使用的名称如下：

+   i. eu-west-2a：

```
PublicSubnet01
AppSubnet01
DataSubnet01
```

+   ii. eu-west-2b：

```
PublicSubnet02
AppSubnet02
DataSubnet02
```

+   3. 确保根据截图为你的路由表命名，以便轻松识别。因此，在此示例中，路由表命名为：
+   1. `PublicRouteTable`：公共子网的路由表
+   2. `AppSubnet01RT`：`AppSubnet01` 的路由表
+   3. `AppSubnet02RT`：`AppSubnet02` 的路由表
+   4. `DataSubnet01RT`：`DataSubnet01` 的路由表
+   5. `DataSubnet02RT`：`DataSubnet02` 的路由表
+   4. 如果你将鼠标悬停在各个子网上，你可以识别它们连接到哪些路由表。
+   5. 为互联网网关提供名称，例如 `Todo-List-IGW`，并为 NAT 网关提供名称，例如 `Todo-List-NATGW`。

+   16. 在所有配置到位后，向下滚动到页面底部并点击 **Create VPC** 按钮。
+   17. 将显示 **Create VPC workflow** 屏幕，并使用所需的组件配置你的 VPC。
+   18. 一旦创建好 VPC，你可以导航回主 VPC 仪表板，然后点击 **Your VPCs** 来查看你新创建的 VPC。

在下一节中，你将配置 VPC 中的一些安全组，以确保流量可以路由到你的 RDS 数据库。

## 项目任务 10.2 – 创建安全组

在这个项目任务中，你将创建两个安全组，这些安全组是托管应用程序的 EC2 实例向后端数据库发送流量所必需的，这些数据库你将很快部署。Amazon Elastic Beanstalk 还将置备一组自己的安全组：一个允许来自互联网到 Application Load Balancers 的流量，另一个安全组将关联到它部署的 EC2 实例，启用 Application Load Balancer 将传入的 HTTP 请求分发到这些实例。

如下面图表所示，最终配置意味着你的 EC2 实例将附加两个安全组，一个用于端口 `80`（HTTP）的入站流量，另一个用于启用这些实例向你的 Amazon RDS 数据库发送数据库请求：

![](img/34ff9b5b5a734dae3450ab701e223c73_419_0.png)

图 10.12：安全组配置

虽然 Elastic Beanstalk 将为 Application Load Balancer 和它部署的 EC2 实例创建安全组，但你必须还配置另外两个安全组 – 一个用于 EC2 实例，另一个用于你即将部署的 RDS 数据库。

+   1. 确保你仍然登录到 AWS 开发账户并位于伦敦区域。
+   2. 如有必要，导航到 VPC 仪表板。
+   3. 从左侧菜单中，点击 Security 部分下的 Security groups。
+   4. 在右侧窗格中，点击 Create security group 按钮。
+   5. 为安全组提供一个名称，例如 App-Security-Group。
+   6. 提供你选择的适当描述。
+   7. 确保从 VPC 下拉列表中选择了正确的 VPC，在这种情况下为 Todo-List-EB-VPC，除非你为你的 VPC 选择了其他名称。
+   8. 滚动到页面底部，点击 Tags – optional 部分中的 Add new tag。创建一个新标签，将 Key 设置为 Name，将 Value – optional 设置为 App-Security-Group。这将确保安全组被标记且易于识别。
+   9. 点击 Create security group 按钮。安全组现在将被创建。你在配置 Elastic Beanstalk 服务和定义 EC2 实例配置时将引用此组。请注意，你未为此安全组创建入站规则。这是完全没问题的，因为当你配置 Elastic Beanstalk 时，该服务将为你的 EC2 实例置备另一个安全组，以从 Application Load Balancer 接受端口 80（HTTP）的入站流量。记住，一个 EC2 实例可以关联多个安全组。
+   10. 你现在将为 RDS 数据库配置安全组。从左侧菜单中，再次点击 Security groups。在右侧窗格中，点击 Create security group 按钮。
+   11. 为你的安全组提供一个合适的名称，例如 Data-Security-Group，以及适当的描述。

