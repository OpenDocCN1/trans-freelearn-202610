

## 从 Jenkins 部署 WAR 文件到 Tomcat

对于应用程序部署，我们可以使用多种方法将应用部署到 Web 服务器或应用服务器。我们可以使用批处理脚本或 shell 脚本复制持续集成过程后创建的包文件，也可以使用 Jenkins 插件部署应用。

## 准备工作

转到“管理 Jenkins | 管理插件”并安装“Deploy to container Plugin”：

| 安装 ↓ | 名称 | 版本 |
|---|---|---|
| Deploy to container Plugin | 1.10 |
| Deploy to Websphere container Plugin | 1.0 |

筛选器：Deploy to  
选项卡：更新、可用、已安装、高级  
按钮：不重启安装、立即下载并在重启后安装、立即检查  
更新信息获取时间：1天2小时前

等待插件成功安装。

5. 使用 Jenkins 插件指定要部署的 WAR 文件路径。从“构建后操作”中选择**将 war/ear 部署到容器**。点击**添加容器**并选择最新版本的 Tomcat。输入 Tomcat URL。输入我们在 `tomcat-users.xml` 中定义的用户名和密码：

6. 通过点击**立即构建**执行构建。验证应用程序部署的日志：

```
Results :

Tests run: 59, Failures: 0, Errors: 0, Skipped: 0

[INFO]
[INFO] --- maven-war-plugin:2.3.1:war (default-war) @ spring-petclinic ---
[INFO] Packaging webapp
[INFO] Assembling webapp [spring-petclinic] in [d:\jenkins\workspace\PetClinic-Test\target\spring-petclinic-4.2.5-SNAPSHOT]
[INFO] Processing war project
[INFO] Copying webapp resources [d:\jenkins\workspace\PetClinic-Test\src\main\webapp]
[INFO] Webapp assembled in [1669 msecs]
[INFO] Building war: d:\jenkins\workspace\PetClinic-Test\target\spring-petclinic-4.2.5-SNAPSHOT.war
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 28.772 s
[INFO] Finished at: 2016-07-06T22:59:37+05:30
[INFO] Final Memory: 29M/261M
[INFO] ------------------------------------------------------------------------
Deploying d:\jenkins\workspace\PetClinic-Test\target\spring-petclinic-4.2.5-SNAPSHOT.war to container Tomcat 7.x Remote
[d:\jenkins\workspace\PetClinic-Test\target\spring-petclinic-4.2.5-SNAPSHOT.war] is not deployed.
Doing a fresh deployment.
  Deploying [d:\jenkins\workspace\PetClinic-Test\target\spring-petclinic-4.2.5-SNAPSHOT.war]
Finished: SUCCESS
```

7. 打开浏览器，使用 Tomcat URL 和应用程序上下文访问该应用：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_410_0.png)

8. 在下一节中，我们将把 PetClinic 应用部署到位于 AWS EC2 实例中的 Tomcat。

## 从 Jenkins 部署 WAR 文件到 AWS Beanstalk

AWS Elastic Beanstalk 是一种**平台即服务（PaaS）**。我们将使用它部署 PetClinic 应用。以下是在 AWS Elastic Beanstalk 上部署应用需要遵循的步骤：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_411_0.png)

## 准备工作

让我们在 Elastic Beanstalk 中创建一个示例应用，以了解 Elastic Beanstalk 的工作原理。然后，我们将使用 Jenkins 插件将应用部署到其中。

在 AWS 管理控制台中点击**服务**，选择**AWS Elastic Beanstalk**。创建一个名为 `petclinic` 的新应用。选择**Tomcat**作为平台，并选择**示例应用**单选按钮：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_412_0.png)

验证创建示例应用的事件顺序：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_412_1.png)

这需要一些时间，一旦环境创建完成，它将以绿色高亮显示：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_413_0.png)

点击 petclinic 环境，并在仪表板中验证运行状况和运行版本：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_413_1.png)

验证环境 ID 和 URL。点击该 URL 并验证默认页面：

## 恭喜

您的第一个 AWS Elastic Beanstalk 应用现在已在 AWS 云中您自己的专属环境中运行。

### 下一步是什么？

- 了解如何使用 AWS Elastic Beanstalk 构建、部署和管理您自己的应用
- AWS Elastic Beanstalk 概念
- 了解如何创建新的应用版本
- 了解如何管理您的应用环境

### 下载 AWS 参考应用

- 使用 AWS SDK for Java 探索一个功能完整的参考应用

### 适用于 Eclipse 的 AWS 工具包

- 开发人员可以直接从 Eclipse 构建和部署 AWS Elastic Beanstalk 应用
- 通过观看此视频开始使用 Eclipse 和 AWS Elastic Beanstalk
- 查看所有 AWS Elastic Beanstalk 文档

## 操作步骤...

1. 安装 AWS Elastic Beanstalk Publisher 插件。更多详细信息，请访问 https://wiki.jenkins-ci.org/display/JENKINS/AWS+Beanstalk+Publisher+Plugin：

2. 在“构建后操作”中会出现一个用于 Elastic Beanstalk 的新区域。

3. 点击 Jenkins 仪表板并选择“凭据”；添加您的 AWS 凭据。

4. 转到您的 Jenkins 构建，并选择在全局配置中设置的 AWS 凭据。

5. 从列表中选择 AWS 区域，然后点击**获取可用应用程序**。由于我们已经创建了一个示例应用，它将像下面这样显示。

6. 在**环境查找**中，于**按名称获取环境**框中提供环境 ID，然后点击**获取可用环境**：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_415_0.png)

### Elastic Beanstalk 应用程序

AWS 凭据

AWS：AKI

按名称查找 AWS 凭据

获取凭据名称

AWS 区域

us-east-1

AWS 区域文本

应用程序名称

petclinic

petclinic

获取可用应用程序

环境查找

#### 按名称获取环境

环境名称

e-y2fmvwri3n

petclinic

获取可用环境

7. 保存配置并点击**立即构建**。

8. 现在，让我们转到 AWS 管理控制台，检查 WAR 文件是否正在复制到 Amazon S3：

9. 转到 S3 服务并检查可用的存储桶：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_416_0.png)

10. 由于 WAR 文件较大，上传到 Amazon S3 需要一些时间。上传完成后，它将出现在 Amazon S3 存储桶中。

11. 在 Jenkins 中检查构建任务的执行状态。预期输出的部分内容如下：

- 测试用例执行和 WAR 文件创建成功
- 构建成功

12. 现在，检查 AWS 管理控制台：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_418_0.png)

16. 应用程序部署成功后，请终止环境。转到特定环境，点击**操作**中的**终止**来终止环境。至此，我们已成功在 Elastic Beanstalk 上部署了应用。

## 从 Jenkins 部署 WAR 文件到 Azure 应用服务

Microsoft Azure 应用服务是一种 PaaS。在本节中，我们将了解 Azure Web 应用以及如何部署我们的 PetClinic 应用。

## 准备工作

我们需要有一个 Microsoft Azure 订阅。转到“应用服务”并点击**+ 添加**：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_420_0.png)

然后，点击**创建**：

### Web 应用

Microsoft

在数秒内创建和部署网站，功能强大，满足您的需求。

利用现有工具创建和部署应用，无需管理基础架构的麻烦。Microsoft Azure Web Sites 为任意规模的 Web 应用提供安全、灵活的开发、部署和扩展选项。使用框架和模板，在数秒内创建网站。从 TFS、GitHub、BitBucket 等源代码控制选项中进行选择。使用任何工具或操作系统，通过 .NET、PHP、Node.js 或 Python 开发您的网站。

- 构建云应用的最快方式
- 快速预置和部署
- 可自动扩展的安全平台
- 为 Visual Studio 开发人员提供出色体验
- 对所有人开放且灵活
- 监控、警报和自动扩展（预览）

![](img/2a4d2e09d9af2c678ea8f43ea344059d_421_0.png)

![](img/2a4d2e09d9af2c678ea8f43ea344059d_421_1.png)

创建

转到 Microsoft Azure 门户 https://portal.azure.com。点击“应用服务”，然后点击**+ 添加**。为“应用名称”、“订阅”、“资源组”和“应用服务计划/位置”提供值。然后，点击**创建**。

