

## 在 Windows 上安装 Jenkins 2

让我们在 Windows 操作系统上安装 Jenkins 2。它可以是物理机，也可以是云上可用的虚拟机。

### 准备工作

要执行本菜谱，你需要下载 Jenkins。

对于业务部门，建议具备以下要求：

- Java 8
- 4 GB+ RAM
- 500 GB+ 可用磁盘空间

### 如何操作...

让我们按照以下步骤安装 Jenkins：

1. 访问 https://jenkins.io。

2. 点击 Download，如以下屏幕截图所示：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_34_0.png)

3. 从 Jenkins 下载最新的 Windows 安装包，地址为：http://mirrors.jenkins.io/windows/latest。

4. 点击该安装包并按照分步说明进行安装。

## 在 CentOS 上安装 Jenkins 2

让我们查看在 CentOS 上安装 Jenkins 所需的步骤。

### 准备工作

对于业务部门，建议具备以下要求：

- Java 8
- 4 GB+ RAM
- 500 GB+ 可用磁盘空间

Jenkins 需要 Java。要安装 Java，请执行：

```
sudo yum install java
```

### 如何操作...

1. Jenkins 的稳定版和最新版本可在 YUM 仓库中使用。
2. 将 Jenkins 仓库添加到 yum 源：

```
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
```

3. 对于稳定版，执行：

```
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
```

4. 导入以下密钥：

```
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
```

5. 通过执行以下命令安装 Jenkins：

```
sudo yum install jenkins
```

### 还有更多...

- 要启动 Jenkins 服务，执行：

  ```
  sudo service jenkins start
  ```

- 要停止 Jenkins 服务，执行：

  ```
  sudo service jenkins stop
  ```

- 要重启 Jenkins 服务，执行：

  ```
  sudo service jenkins restart
  ```

## 在 Azure 上安装 Jenkins 2

Microsoft Azure 是近年来最受欢迎的云服务提供商之一。让我们尝试在 Azure 上安装 Jenkins。

### 准备工作

你需要在 Azure 上安装 Jenkins 时拥有 Microsoft Azure 订阅。还提供一个月的免费试用：

1. 访问 https://jenkins.io。
2. 点击 Download。

### 如何操作...

让我们看看如何在 Azure 上安装 Jenkins：

1. 在 Jenkins 下载页面点击 Deploy to Azure 链接：

### 开始使用 Jenkins

Jenkins 项目提供两个发布分支，LTS 和 weekly。根据组织的需求，其中一个可能更受青睐。

两个发布分支均以 .war 文件、原生包、安装程序和 Docker 容器形式分发。

### 长期支持 LTS

LTS 长期支持 版本每 12 周从常规发布流中挑选一次，作为该时期的稳定版本。了解更多...

Changelog | Upgrade Guide | Past Releases

**部署 Jenkins 2.60.1**

![Deploy to Azure](https://jenkins.io/images/deploy-to-azure.png)

**下载 Jenkins 2.60.1 for：**

- Docker
- FreeBSD
- Gentoo
- Mac OS X
- OpenBSD
- openSUSE

### Weekly

每周都会发布新版本，以向用户和插件开发者提供错误修复和功能。

Changelog | Past Releases

**下载 Jenkins 2.70 for：**

- Arch Linux
- Docker
- FreeBSD
- Gentoo
- Mac OS X
- OpenBSD
- openSUSE
- Red Hat/Fedora/CentOS

2. 它会将你重定向到 Azure Marketplace。
3. 在同一页面上核实涉及的定价计划和类别。
4. 点击 GET IT NOW。这些步骤在以下屏幕截图中演示：

### Jenkins

Microsoft

Microsoft 在 Linux Ubuntu VM 上发布了 Jenkins Master

我们很高兴在 Microsoft Azure 上推出 Jenkins 支持的下一阶段，发布了安全、稳定且可用于生产环境的 Jenkins 版本。

**注意：** 要在部署后连接到此 Jenkins 实例，请浏览到该实例的 URL 或公共 IP。URL 是你在设置中输入的域名标签和该字段下方显示的后缀。

此解决方案模板将在 Linux Ubuntu 14.04 LTS VM 上安装最新的稳定 Jenkins 版本，并附带与 Azure 配合使用的工具和插件配置。这包括 –

- git 用于源代码控制
- Azure Credentials 插件用于安全连接
- Azure VM Agents 插件用于弹性构建、测试和持续集成
- Azure Storage 插件用于存储构建产物
- Azure CLI 用于使用脚本部署应用

有关此解决方案模板为你自动化的步骤的详细演练，请访问我们的博客文章。

5. 在 Create this app in Azure 选项卡中，点击 Continue：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_43_0.png)

![](img/2a4d2e09d9af2c678ea8f43ea344059d_43_1.png)

6. 如果你已有 Azure 订阅，则使用用户名和密码登录。
7. 部署模型已选择。
8. 点击 Create：

我们很高兴在 Microsoft Azure 上推出 Jenkins 支持的下一阶段，发布了安全、稳定且可用于生产环境的 Jenkins 版本。

**注意：** 要在部署后连接到此 Jenkins 实例，请浏览到该实例的 URL 或公共 IP。URL 是你在设置中输入的域名标签和该字段下方显示的后缀。

此解决方案模板将在 Linux Ubuntu 14.04 LTS VM 上安装最新的稳定 Jenkins 版本，并附带与 Azure 配合使用的工具和插件配置。这包括 –

- git 用于源代码控制
- Azure Credentials 插件用于安全连接
- Azure VM Agents 插件用于弹性构建、测试和持续集成
- Azure Storage 插件用于存储构建产物
- Azure CLI 用于使用脚本部署应用

有关此解决方案模板为你自动化的步骤的详细演练，请访问我们的博客文章。

此解决方案模板旨在按照最佳实践配置 Jenkins 实例，且 Azure 知识要求最少。通过少量用户输入和通过 Azure 门户的简单一键部署，你可以在几分钟内配置一个完全配置好的 Jenkins 实例，该实例可以

- 选择部署模型 🛈
- Resource Manager

9. 提供密码、Jenkins 发布类型和订阅详情，如下图所示：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_46_0.png)

10. 选择 Create new 并提供资源组名称。
11. 根据你的偏好选择位置。
12. 点击 OK：

### Create Jenkins

13. 根据需求选择大小。
14. 提供唯一的域名标签。
15. 点击 OK：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_48_0.png)

16. 查看选择。
17. 将根据你的选择执行自动验证。
18. 点击 OK：

19. 在点击 Purchase 按钮之前查看可用的模板链接：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_49_0.png)

20. 准备就绪后，点击 Purchase：

21. 在 Azure 门户中，点击你创建的资源组。逐一查看正在创建的资源。在概览部分查看部署。

22. 点击 JenkinsIP 并访问与其关联的域名：

### Jenkins On Azure

此 Jenkins 实例不支持 https，因此通过公共 IP 地址登录已被禁用（这会将你的密码和其他信息暴露给窃听）。要安全登录，你需要使用 SSH 端口转发连接到 Jenkins 实例。

```
ssh -L 127.0.0.1:8080:localhost:8080 username@msbjenkins.centralindia.cloudapp.azure.com
```

如果你不想公开此 Jenkins 实例，你需要移除 nginx 反向代理。

```
sudo apt-get purge nginx nginx-common
```

23. 下载 PuTTY 或其他 ssh 客户端，以便我们可以连接到 Jenkins 页面中提到的 URL。
24. 前往 putty.exe 可用的位置。
25. 在命令提示符中执行 Jenkins 页面上给出的命令。
26. 在 PuTTY 安全警告中选择 Yes：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_51_0.png)

27. 打开浏览器并导航到 URL，http://localhost:8080。
28. 它会要求输入管理员密码：

