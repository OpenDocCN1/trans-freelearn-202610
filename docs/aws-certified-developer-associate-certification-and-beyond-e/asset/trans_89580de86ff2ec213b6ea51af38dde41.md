

配置目标组的步骤如下：

- 1. 从 AWS 管理控制台搜索 EC2 并进入 EC2 控制台。请确保你位于 **us-east-1 (N. Virginia)** 区域。
- 2. 从左侧菜单的 **Load Balancing**（负载均衡）部分选择 **Target Groups**（目标组）。
- 3. 在右侧窗格中，点击 **Create target group**（创建目标组）按钮。
- 4. 从 **Choose a target type**（选择目标类型）部分选择 **Instances**（实例）。
- 5. 接下来，在 *Target group name*（目标组名称）下方的文本框中提供一个描述性名称，例如 `todo-list-ec2-target-group`。
- 6. 按照以下截图所示，确保将 **Protocol**（协议）和 **Port**（端口）分别设置为 **HTTP** 和 **80**：

![](img/34ff9b5b5a734dae3450ab701e223c73_295_0.png)

图 7.20 – 为你的负载均衡器配置目标组

- 7. 将 **IP address type**（IP 地址类型）设置为 **IPv4**。
- 8. 从 VPC 下拉框中选择你之前创建的 **todolist-vpc**。
- 9. 确保从 *Protocol*（协议）版本中选择了 **HTTP1**。
- 10. 保持其余所有设置为默认值，并点击页面底部的 **Next**（下一步）。
- 11. 你会注意到目前没有任何可用实例可以添加到该目标组中。这是没问题的，因为你稍后配置自动扩缩组时，会自动将新启动的实例与该目标组关联。现在，请点击 **Create target group**（创建目标组）按钮。
- 12. 你应该会收到一条通知，指示目标组已成功创建。

接下来，你将创建自动扩缩服务的启动模板。**启动模板**允许你指定配置详情，如 AMI ID、实例类型和族、用于连接实例的密钥对，以及需要与实例关联的任何安全组。AWS 强烈建议使用**启动模板**来定义这些配置项，而不是使用以前的**启动配置**。这是因为模板提供了额外功能，并且你可以拥有启动模板的多个版本。托管多个版本可以让你快速为不同的用例配置你的机群；例如，你在一套 AMI 用于开发，另一套用于生产。

让我们继续设置启动模板。

## 项目任务 7.5 – 创建启动模板

在此步骤中，你将创建一个启动模板。作为配置的一部分，你将引用在*第 5 章，使用 Amazon EC2 了解 AWS 计算服务及存储选项*中创建的 **Amazon Machine Image (AMI)**。该 AMI 已经嵌入了某些配置，例如 Node.js 和 PM2 的安装。虽然就本项目任务而言，安装这些组件所花的时间很少，但你可以想象，拥有预配置的 AMI 如何能显著加快你的 EC2 实例的部署过程：

- 1. 确保你仍在 EC2 管理控制台中，并且位于 **us-east-1 (N. Virginia)** 区域。
- 2. 从左侧菜单的 **Instances**（实例）部分选择 **Launch Templates**（启动模板）。
- 3. 在右侧窗格中，点击 **Create launch template**（创建启动模板）按钮。
- 4. 在出现的启动模板向导中，为你的模板提供一个合适的名称，例如 `todo-list-launch-template`。
- 5. 接下来，为你的启动模板提供描述。
- 6. 在 **Application and OS Images (Amazon Machine Image)**（应用程序和操作系统镜像（Amazon 机器映像））下，选择 **My AMIs**（我的 AMI）选项卡，并从 **Amazon Machine Image (AMI)**（Amazon 机器映像）下拉列表中选择你在*第 5 章，使用 Amazon EC2 了解 AWS 计算服务及存储选项*中创建的 AMI，如下图所示：

### 应用程序和操作系统镜像 (Amazon Machine Image) 信息

AMI 是一个模板，包含启动你的实例所需的软件配置（操作系统、应用程序服务器和应用程序）。如果你在下文中没有找到正在寻找的内容，请搜索或浏览 AMI。

搜索我们的完整目录，包括数千个应用程序和操作系统镜像

- 最近的
- 我的 AMI
- 快速启动

不要包含在启动模板中

我拥有的

分享给我

浏览更多 AMI
包括来自 AWS、市场（Marketplace）和社区的 AMI

Amazon Machine Image (AMI)

todolist-worker-ami
ami-01a82b0668b883e1
2024-03-21T06:35:32.000Z 虚拟化: hvm ENA 已启用: true 根设备类型: ebs

图 7.21 – 启动模板配置：选择你的企业 AMI

- 7. 接下来，在实例类型部分，选择 t2.micro，它符合免费套餐资格。
- 8. 从密钥对名称下拉列表中选择你在步骤 3 中创建的密钥对。
- 9. 接下来，在网络设置部分，确保在子网下拉列表中选择了 Don't include in launch template（不包含在启动模板中）。

10. 接下来，在防火墙（安全组）子部分中，确保选择了 Select existing security group（选择现有安全组），然后从 Security groups（安全组）下的下拉列表中选择你在*第 4 章，使用 Amazon VPC 在云中构建私有网络*中创建的应用程序安全组，如下图所示。请记住，应用程序安全组是你将关联到 EC2 实例的安全组，因为它允许负载均衡器向实例上运行的应用程序传输入站流量。

![](img/34ff9b5b5a734dae3450ab701e223c73_298_0.png)

图 7.22 – 配置你的启动模板的网络设置

11. 接下来，向下滚动并展开高级详细信息部分。

12. 选择 IAM 实例配置文件下拉箭头，并选择你在*第 5 章，使用 Amazon EC2 了解 AWS 计算服务及存储选项*中创建的 IAM 角色。请记住，该角色命名为 todo-ec2-iam-master-role，并与以下三个策略关联：

- I. AmazonSSMManagedInstanceCore - 这是一个 AWS 管理的策略
- II. todo-list-s3-sourcecode-access-policy - 这是一个客户管理的策略
- III. todo-list-secret-access-policy - 这是一个客户管理的策略

13. 接下来，滚动到用户数据 – 可选部分。在用户数据文本框中，你必须粘贴我们在 GitHub 仓库中提供给你的 Bash 脚本 (user-data.sh)。请参阅以下截图作为示例：

```
1  #!/bin/bash
2
3  AWS_REGION=us-east-1
4  SECRET_NAME=[Secret Name]
5  DB_HOST=[Database Endpoint]
6  DB_PORT=3306
7
8  yum update -y
9  aws s3 cp s3://[S3 Bucket Name]/todo-plus-nodejs.zip /var
10 unzip /var/todo-plus-nodejs.zip -d /var
11 cd /var/todo-plus-nodejs
12 npm install
13 npm run build
14
15 echo "export AWS_REGION=$AWS_REGION" >> ~/.bashrc
16 echo "export SECRET_NAME=$SECRET_NAME" >> ~/.bashrc
17 echo "export DB_HOST=$DB_HOST" >> ~/.bashrc
18 echo "export DB_PORT=$DB_PORT" >> ~/.bashrc
19 source ~/.bashrc
20 pm2 start dist/main.js+
```

图 7.23 – 示例用户数据脚本

14. 确保填写你在*第 6 章，使用 AWS 关系型和非关系型 (NoSQL) 数据库管理多个数据集*中创建的机密名称。

15. 用你的 RDS 数据库的端点地址填充 DB_HOST 占位符值。你可以在 RDS 仪表板下的“数据库”中找到它。点击你在*第 6 章，使用 AWS 关系型和非关系型 (NoSQL) 数据库管理多个数据集*中部署的数据库，以查看其连接性和安全性设置。你会发现提供了端点地址，你可以将其复制并粘贴到代码中，替换占位符值。

16. 最后，替换你之前在项目任务 7.1 中创建的 Amazon S3 存储桶名称的占位符。

17. 接下来，点击 Create launch template（创建启动模板）按钮。

18. 你应该会收到一条通知，表明启动模板已成功创建。

现在你已经创建了启动模板，是时候设置自动扩缩组了。自动扩缩组使用启动模板中提供的详细信息，并允许你定义组的最小和最大大小。你还可以指定自动扩缩服务旨在满足的期望容量。自动扩缩服务还将配置为使用目标组配置执行健康检查。在固定实例数量配置中，如果某个实例未通过健康检查，自动扩缩服务将终止该故障实例并用一个新实例替换它。

如本章前面所讨论的，你还可以定义扩缩策略，如目标跟踪扩缩策略或步骤扩缩。然而，就本项目而言，我们将配置自动扩缩服务以维护机群中固定数量的实例。

## 项目任务 7.6 – 创建自动扩缩组

在此任务中，你将继续配置你的自动扩缩组：

- 1. 确保你仍在 EC2 管理控制台中，并且位于 **us-east-1 (N. Virginia)** 区域。
- 2. 从左侧菜单的 **Auto Scaling**（自动扩缩）部分选择 **Auto Scaling Groups**（自动扩缩组）。
- 3. 接下来，在右侧窗格中，点击 **Create Auto Scaling group**（创建自动扩缩组）按钮。
- 4. 为你的自动扩缩组提供一个描述性名称，例如 `todo-list-auto-scaling-group`。
- 5. 从下拉框中选择你在上一步中创建的启动模板，如下图所示：

![](img/34ff9b5b5a734dae3450ab701e223c73_300_0.png)

图 7.24 – 配置自动扩缩组并为组选择启动模板

