
7. 在**数据库**部分，按照以下截图选择您创建的 MySQL 数据库，例如 `tododb`：

![](img/34ff9b5b5a734dae3450ab701e223c73_426_0.png)

图 10.19：在 AWS Secrets Manager 中为 MySQL 数据库配置密钥

8. 点击页面底部的 **Next**。

9. 在 **Configure secret** 页面，为您的密钥提供一个名称，例如 `tododbsecret`。

10. 提供一个可选的描述（由您决定），然后点击 **Next**。

11. 在 **Configure rotation – optional** 部分，启用 **Automatic rotation** 切换开关。

12. 将 **Time unit** 设置为 **Days**，并将 **Days** 设置为 7。

13. 在 **Rotation function** 部分，确保已选择 **Create a rotation function**，然后为您的 Lambda 轮换函数提供一个名称。请注意，AWS Secrets Manager 会在您选择的名称前添加 `SecretsManager` 作为前缀，参见以下截图：

![](img/34ff9b5b5a734dae3450ab701e223c73_427_0.png)

图 10.20：配置密钥轮换并创建轮换函数

14. 对于 **Rotation strategy**，确保已选择 **Single user**。点击 **Next**。

15. 查看配置，然后点击页面底部的 **Store** 按钮。

您现在已在 AWS Secrets Manager 中创建了一个名为 `tododbsecret` 的密钥。Lambda 轮换函数的创建需要几分钟时间，由 AWS CloudFormation 完成。Secrets Manager 现在正在管理您的数据库凭据，每隔七天轮换一次，以确保定期生成新的凭据集以增强安全性。

在下一节中，您将创建一个具有必要策略的 IAM 角色，以允许 Elastic Beanstalk 服务与您的环境（例如 EC2 实例）进行交互，并查询 Secrets Manager 以动态检索凭据数据并将其存储在环境变量中。

## 项目任务 10.5 – 为您的 Elastic Beanstalk 应用配置 IAM 角色

在此项目任务中，您需要创建一个 IAM 角色，该角色需包含以下由 AWS 管理的策略：

- `AWSElasticBeanstalkWebTier`
- `AWSElasticBeanstalkMulticontainerDocker`
- `AWSElasticBeanstalkWorkerTier`

此外，您还需要一个内联策略，该策略授予访问 AWS Secrets Manager 中密钥的权限。然后，该角色将被配置为由 Elastic Beanstalk 服务部署的 EC2 实例假定（assumed）。

1. 在 AWS 管理控制台中，搜索并导航至 **AWS Identity and Access Management (IAM)** 服务。
2. 从左侧菜单中，点击 **Roles**。
3. 在右侧窗格中，点击 **Create role** 按钮。
4. 对于 **Trusted entity type**，选择 **AWS service**。
5. 接下来，从 **Service or use case** 下拉列表中选择 **EC2**，然后选择 **EC2** 作为 **Use case**。点击 **Next**。
6. 在 **Add Permissions** 页面，搜索并选择前面提到的三个 AWS Elastic Beanstalk 策略，然后点击 **Next**。
7. 为角色提供一个名称，例如 `Todo-EB-SM-IAM-Role`。
8. 滚动到页面底部并点击 **Create role**。
9. 您的角色将被创建。接下来，在可用的角色列表中的搜索框中搜索该角色，参见以下截图（*图 10.21*）：

![](img/34ff9b5b5a734dae3450ab701e223c73_429_0.png)

图 10.21：用于 Elastic Beanstalk 和 Secrets Manager 的 IAM 角色

10. 点击该角色以选中它。
11. 在 Permissions 选项卡中，点击 Add permissions 下拉列表并选择 Create inline policy。
12. 在 Specify permissions 页面中，在 Service 部分的下拉列表中选择 **Secrets Manager**（从 Choose a service 下拉列表中选择）。
13. 展开 **Read** 访问级别选项，并选择 **GetSecretValue** 操作。
14. 点击 Resources 下方的 **Add ARNs** 链接。在出现的对话框中，您需要提供您在 Secrets Manager 中创建的密钥的 Amazon Resource Name (ARN)。要获取 ARN，请执行以下操作：
    1. 在单独的浏览器标签页中打开 Secrets Manager。
    2. 从左侧菜单中点击 **Secrets**，然后在右侧窗格中点击您创建的密钥。
    3. 从 Secret details 部分中，将密钥的 ARN 复制到您的剪贴板。
    4. 在您的 IAM 浏览器标签页对话框中的 **Specify ARN(s)** 部分，将您从 Secrets Manager 控制台中复制的 ARN 粘贴到 Resource ARN 旁边的文本框中。对话框中的其他字段将自动填充。
    5. 点击 **Add ARNs**。

15. 您将被带回 Specify permissions 页面，该页面应与图 10.22 类似：

![](img/34ff9b5b5a734dae3450ab701e223c73_430_0.png)

图 10.22：配置内联策略以访问 AWS Secrets Manager 中的密钥

16. 滚动到页面底部并点击 **Next**。
17. 提供一个策略名称，例如 `Todo-EB-Secret-IAM-Policy`。
18. 点击页面底部的 **Create policy**。
19. 您的内联策略已创建并添加到您刚才定义的 IAM 角色中。

既然 IAM 角色和策略现已定义，您可以继续创建您的 Elastic Beanstalk 应用和环境。

## 项目任务 10.6 – 创建 Elastic Beanstalk 应用

在此项目任务中，您将创建一个 Elastic Beanstalk 应用和环境。然后，您将从 GitHub 仓库下载的应用代码进行上传，并将该应用程序部署到一组 EC2 实例中。Amazon Elastic Beanstalk 将在自动扩展组中预置 EC2 实例，部署应用程序负载均衡器，并通过您定义的环境变量配置到后端数据库的连接。

1. 使用 AWS 管理控制台顶部的搜索栏搜索并导航至 Elastic Beanstalk 服务。
2. 确保您仍处于 London 区域。
3. 从左侧菜单中点击 **Applications**，然后在右侧窗格中点击 **Create application** 按钮。
4. 为您的应用程序提供一个名称，例如 `Todo-List-EB-App`。
5. 提供您的应用程序描述。
6. 点击 **Create** 按钮。
7. 在 **Application Todo-List-EB-App environments** 页面上，点击右侧窗格中的 **Create new environment** 按钮。
8. 您将被带到 Configure environment 页面。确保从 Environment tier 中选择了 **Web server environment**。
9. 您将注意到，**Environment name** 已预填为您的应用程序名称后跟 `-env`。例如，`Todo-List-EB-App-env`。您可以保持原样。
10. 向下滚动到页面的 **Platform** 部分。从 **Platform** 下拉列表中选择 **PHP**。接受默认的平台分支，应该是运行在 64 位 Amazon Linux 2023 上的 PHP 8.3。
11. 在 **Application code** 部分中，点击 **Upload your code** 单选按钮。
12. 提供一个版本标签，例如 `v1`。
13. 在 Source code origin 下选择 **Local file**。最大大小 500 MB。
14. 点击 **Choose file** 按钮，导航到您保存从我们的 GitHub 仓库下载的应用程序源代码的位置。选择要上传的 `.zip` 文件，参见以下截图：

![](img/34ff9b5b5a734dae3450ab701e223c73_432_0.png)

图 10.23：将您的源代码上传到 Elastic Beanstalk

15. 在 **Presets** 下，选择 **High availability**。
16. 点击 **Next**。
17. 在 **Configure service access** 部分中，在 Service role 下选择 **Create and use new service role** 选项。
18. 从 **EC2 instance profile** 下拉列表中选择您在项目任务 10.5 中创建的 IAM 角色。点击 **Next**。
19. 在 **Set up networking, databases, and tags** 部分中，从 **VPC** 下拉列表中选择您在项目任务 10.1 中创建的 VPC。
20. 接下来，选择您希望部署 EC2 实例的可用区（AZs）和子网。这些将是应用子网（app subnets）。具体来说，这些将是 `AppSubnet01` 和 `AppSubnet02`，参见以下截图：

### 实例子网

筛选实例子网

|  | 可用区 | 子网 | CIDR | 名称 |
|---|---|---|---|---|
| ☑ | eu-west-2a | subnet-00c162... | 10.5.10.0/24 | AppSubnet01 |
| ☐ | eu-west-2a | subnet-0107e5... | 10.5.1.0/24 | PublicSubnet01 |
| ☐ | eu-west-2a | subnet-063fba... | 10.5.20.0/24 | DataSubnet01 |
| ☐ | eu-west-2b | subnet-0985f4... | 10.5.21.0/24 | DataSubnet02 |
| ☐ | eu-west-2b | subnet-0a577b... | 10.5.2.0/24 | PublicSubnet02 |
| ☑ | eu-west-2b | subnet-0ed45b... | 10.5.11.0/24 | AppSubnet02 |

图 10.24：选择您的 EC2 实例的子网

21. 保持 **Database** 部分不变，因为您的数据库解决方案是在 Elastic Beanstalk 配置之外构建的。
22. 继续向下滚动并点击 **Next**。
23. 在 **Configure instance traffic and scaling – optional** 部分中，再向下滚动一点，并从 **EC2 security groups** 列表中选择 `App-Security-Group`。
24. 在 **Capacity** 子部分中，确保从 **Environment type** 下拉列表中选择了 **Load Balanced**。在 **Instances** 下，将最小实例大小设置为 1，最大实例大小设置为 2。
25. 再向下滚动一点，确保在 **Instance types** 下拉列表中仅选择了 `t3.micro`。
26. 在 **Load balancer network settings** 部分中，确保 **Visibility** 设置为 **Public**。

