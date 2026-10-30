

```
You have not yet set up your credentials or your credentials are incorrect.
You must provide your credentials.
(aws-access-id): AKIAJOUAASEXAMPLE
(aws-secret-key): 5ZRIrtTM4ciIAvd4EXAMPLEDtm+PiPSzpoK
```

请记住不要与任何人分享 AWS 凭证。如果它们落入错误的人手中，他们可以接管你的整个 AWS 基础设施。

### 4. 接下来，我们需要为我们的应用程序命名：

```
Enter Application Name
(default is "eb"): django-demo-app
Application django-demo-app has been created.
```

### 5. Elastic Beanstalk 可以在多个平台上运行。我们的应用程序是一个在 Docker 中运行的 Django 应用程序，因此我们必须选择 Docker 平台：

```
Select a platform.
1) .NET Core on Linux
2) .NET on Windows Server
3) Docker
4) Go
5) Java
6) Node.js
7) PHP
8) Packer
9) Python
10) Ruby
11) Tomcat
(make a selection): 3
```

### 6. 选择你希望 Beanstalk 运行的 Docker 平台。我们将选择 `Docker running on 64-bit Amazon Linus 2023`：

```
Select a platform branch.
1) Docker running on 64bit Amazon Linux 2023
2) Docker running on 64bit Amazon Linux 2
3) ECS running on 64bit Amazon Linux 2
(default is 1): 1
```

### 7. 接下来，Beanstalk 会自动为我们的 AWS 实例设置 SSH 访问。我们需要选择 yes 来分配 SSH 密钥对。这将允许我们直接 SSH 到我们的 Beanstalk 应用程序：

```
Do you want to set up SSH for your instances?
(y/n): y
```

### 8. Beanstalk 创建一个新的 SSH 密钥对，它将用于为 EC2 实例提供访问权限：

```
Type a keypair name.
(Default is aws-eb):
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/argo/.ssh/aws-eb
Your public key has been saved in /Users/argo/.ssh/aws-eb.pub
```

至此，我们的 Beanstalk 应用程序已创建。现在，我们可以进入 AWS 控制台，找到我们的 Beanstalk 应用程序已准备就绪。*图 13.7* 显示了我们新创建的 Beanstalk 应用程序：

![Figure 13.7: The AWS console showing the newly created Beanstalk application, django-demo-app](image/Figure_13.7_B18867.jpg)

图 13.7：AWS 控制台显示新创建的 Beanstalk 应用程序，django-demo-app

当我们进入 AWS 控制台中的 `django-demo-app` 应用程序时，我们不会看到环境，如 *图 13.8* 所示。这是因为我们已创建了 Beanstalk 应用程序，但尚未创建任何将在 Beanstalk 上运行 Django 应用程序的环境：

![Figure 13.8: The AWS console showing the empty Beanstalk application](image/Figure_13.8_B18867.jpg)

图 13.8：AWS 控制台显示空的 Beanstalk 应用程序

1. 要创建环境，请进入 Django 应用程序文件夹并运行 `eb create <environment-name>` 命令。例如，如果我们将环境命名为 `prod-env`，则应使用以下命令创建 Beanstalk 环境：

```
eb create prod-env
```

现在，我们可以进入 AWS Elastic Beanstalk 仪表板，查看我们的 Beanstalk 环境正在创建。此步骤通常需要 10 分钟，因为 Elastic Beanstalk 将创建所有必需的 AWS 组件，例如 EC2 实例、`Application Load Balancer` (`ALB`)、自动扩展组、CloudWatch 警报等：

![Figure 13.9: The AWS Elastic Beanstalk dashboard showing the prod-env environment](image/Figure_13.9_B18867.jpg)

图 13.9：AWS Elastic Beanstalk 仪表板显示 prod-env 环境

一旦 Elastic Beanstalk 环境准备就绪，我们将看到 `Health` 状态从 `Pending` 变为 `Ok`。这表明应用程序已准备好提供流量。在 *图 13.10* 中，我们可以看到 `prod-env` 的状态，它表明它已准备好提供流量：

![Figure 13.10: AWS Elastic Beanstalk prod-env showing a health status of Ok](image/Figure_13.10_B18867.jpg)

图 13.10：AWS Elastic Beanstalk prod-env 显示健康状态为 Ok

*图 13.10* 中显示的 `Domain` 值是可以用于从互联网访问我们的 Django 应用程序的公共域名。在我们的例子中，它是 `http://prod-env.eba-e5mff922.ap-south-1.elasticbeanstalk.com/admin`（示例 URL）；当我们在浏览器中打开它时，我们应该能够看到初始的 Django 网页。我们可以在 *图 13.11* 中看到浏览器的输出：

![Figure 13.11: Initial Django web page](image/Figure_13.11_B18867.jpg)

图 13.11：初始 Django 网页

`eb` CLI 仅适用于位于 Git 中并且至少有一个提交的项目。如果遇到如下所示的错误，请确保你的项目正在使用 Git 进行跟踪：

```
> eb create prod-env
WARNING: Git is in a detached head state. Using branch "default"
WARNING: Git is in a detached head state. Using branch "default"
WARNING: Git is in a detached head state. Using branch "default"
WARNING: Git is in a detached head state. Using branch "default"
ERROR: CommandError – git could not find the HEAD; most likely because there are no git commits present
```

使用我们的样板 Django 项目代码，我们已经了解了如何将应用程序部署到 Elastic Beanstalk。在我们的示例中，我们使用了基本的样板代码来部署 Django 应用程序，而不连接到数据库或运行 Web 服务所需的任何其他外部服务。现在，让我们学习如何将 Django 应用程序连接到 AWS RDS 和 ElastiCache。

## 创建 AWS RDS Postgres 实例并连接到 Django

Elastic Beanstalk 为我们提供了为给定的 Elastic Beanstalk 环境创建 RDS 的选项。然而，我们不会使用该选项，以确保我们的 RDS 实例与 Beanstalk 解耦。

现在，让我们学习如何创建一个可以连接到我们的 Beanstalk 环境的 RDS 实例并配置我们的 Django 代码。

### 创建新的 RDS 环境

以下是创建新 RDS 环境可以遵循的简单步骤：

1. 在 AWS 控制台中进入 RDS 仪表板：

![Figure 13.12: Navigating to RDS on the AWS dashboard](image/Figure_13.12_B18867.jpg)

图 13.12：在 AWS 仪表板上导航到 RDS

2. 在左侧边栏进入 `Databases` 部分，然后单击 `Create Database` 以打开向导，如 *图 13.13* 所示：

![Figure 13.13: The RDS dashboard in the AWS console](image/Figure_13.13_B18867.jpg)

图 13.13：AWS 控制台中的 RDS 仪表板

3. 选择 `Easy Create` 选项，然后选择以下选项（或者，你可以通过使用 `Standard Create` 选项以更多选项配置数据库。我们将在本教程中遵循 `Easy Create` 选项）：

   1. 在向导中选择 `Option`。然后，在 `Engine Type` 下选择 `PostgreSQL`。
   2. 选择 `Free tier`。
   3. 为数据库命名。我们将其命名为 `django-demo`。
   4. 指定主用户名。我们将使用 `django_db_user`。
   5. 指定主密码。我们将使用 `abc^A12a*12`。
   6. 然后，单击 `Create` 按钮。

4. RDS 需要 4-5 分钟来创建数据库。创建后，我们会看到数据库的状态为 `Creating`：

![Figure 13.14: A new RDS instance being created](image/Figure_13.14_B18867.jpg)

图 13.14：正在创建新的 RDS 实例

至此，我们已创建了数据库。接下来，我们需要将数据库连接到 Django 代码。默认情况下，RDS 不暴露给互联网，这意味着我们无法在 AWS VPC 网络之外连接到 RDS 实例。不将 RDS 服务器暴露给互联网是一个良好的实践。

然而，这并不意味着我们的 Elastic Beanstalk 服务器可以直接连接到 RDS 实例。要连接到 RDS 实例，我们必须配置我们的 VPC 安全组以启用连接。

### 将 RDS 连接到 Django 服务器

