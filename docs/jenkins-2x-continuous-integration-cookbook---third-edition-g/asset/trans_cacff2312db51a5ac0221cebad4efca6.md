

## 还有更多...

在 Configure System 部分，这里有一些重要的配置需要你进行，例如 SCM Polling、E-mail Notification、Extended E-mail Notification、Git plugin、Disk usage、Quality Gates - SonarQube、Build Pipeline Plugin、Copyartifact: Upstream build that triggered this job、Audit Trail、SonarQube servers 等等。

## 配置 JENKINS_HOME

`.jenkins` 是包含 Jenkins 安装文件、配置、插件、构建作业配置等所有详细信息的核心目录。

## 准备工作

`.jenkins` 目录应保存在有充足可用空间的位置。默认情况下，Jenkins 会根据操作系统在特定位置创建 `.jenkins`（或 `JENKINS_HOME`）。

例如，在 Windows 中它位于 `c:\Users\<USER_NAME>\.jenkins`。

## 如何操作...

- 1. 在 Windows 中，要更改 `JENKINS_HOME`，请前往 控制面板 | 所有控制面板项 | 系统。
- 2. 点击 高级系统设置。
- 3. 点击 环境变量。
- 4. 创建一个新变量 JENKINS_HOME 并给出路径：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_86_0.png)

- 5. 在 Ubuntu 中，前往 `/etc/default/Jenkins`。
- 6. 更改 `JENKINS_HOME` 位置：

```
mitesh51@Jenkins:~$ vi /etc/default/jenkins
defaults for Jenkins automation server

# pulled in from the init script; makes things easier.
NAME=jenkins

# location of java
JAVA=/usr/bin/java

# arguments to pass to java
# Allow graphs etc. to work even when an X server is present
JAVA_ARGS="-Djava.awt.headless=true"

#JAVA_ARGS="-Xmx256m"

# make jenkins listen on IPv4 address
#JAVA_ARGS="-Djava.net.preferIPv4Stack=true"

PIDFILE=/var/run/$NAME/$NAME.pid

# user and group to be invoked as (default to jenkins)
JENKINS_USER=$NAME
JENKINS_GROUP=$NAME

# location of the jenkins war file
JENKINS_WAR=/usr/share/$NAME/$NAME.war

# jenkins home location
JENKINS_HOME=/var/lib/$NAME

# set this to false if you don't want Jenkins to run by itself
# in this set up, you are expected to provide a servlet container
# to host jenkins.
RUN_STANDALONE=true

# log location. this may be a syslog facility.priority
JENKINS_LOG=/var/log/$NAME/$NAME.log
#JENKINS_LOG=daemon.info
```

7. 完成更改后，保存更改。

## 还有更多...

下表描述了不同操作系统中 `JENKINS_HOME` 的默认位置：

| 操作系统 | $JENKINS_HOME 位置 |
|---|---|
| Windows | C:\Program Files (x86)\jenkins or c:\Users\<USER>\.jenkins |
| Mac OSX | Macintosh HD/Users/Shared/Jenkins |
| Ubuntu/Debian | /var/lib/jenkins |
| Red Hat/CentOS/Fedora | /var/lib/jenkins |
| OpenSUSE | /var/lib/jenkins |
| FreeBSD | /usr/local/etc/jenkins |
| OpenBSD | /usr/local/etc/jenkins |

## 理解 JENKINS_HOME 目录

在本教程中，我们将讨论 `JENKINS_HOME` 目录中可用的目录。

## 准备工作

从环境变量中定位 `JENKINS_HOME` 目录，或前往不同操作系统中 `JENKINS_HOME` 可用的默认位置。

## 如何操作...

- 1. `JENKINS_HOME` 目录中有许多文件和目录。我们将对一些重要文件和目录进行简要说明：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_93_0.png)

2. `JENKINS_HOME` 目录中一些重要的文件和目录如下：

- `config.xml` — Jenkins 根配置文件
- `fingerprints` — 它存储指纹记录，如有
- `plugins` — 它是所有 Jenkins 插件的根目录
- `jobs` — 它是所有 Jenkins 作业的根目录
- `logs` — 它存储所有日志文件
- `secrets` — 它是用于凭据解密的密钥的根目录
- `users` — 它存储 Jenkins 中所有与用户相关的详细信息
- `war` — 它存储与 `JENKINS_WAR` 文件相关的所有详细信息
- `workspace` — 它存储与不同构建作业相关的所有文件和工件，并在归档元素时将内容移至 jobs 目录。

## 另请参阅

在 `JENKINS_HOME` 目录中，用任意文本编辑器打开 `config.xml` 并查看文件中可用的选项。

## 为 Jenkins 使用不同的端口

默认情况下，Jenkins 运行在端口号 8080。

## 如何操作...

让我们更改 Jenkins 运行的端口：

- 1. 如果你使用命令行运行 Jenkins，则可以执行类似 `java -jar -httpPort=9999 jenkins.war` 的命令，将现有端口从 `8080` 更改为 `9999`。
- 2. 另一种更改端口的方法是在使用 Windows 包安装 Jenkins 时如下：
  - 前往你安装 Jenkins 的 `Program Files/Jenkins` 目录
  - 在编辑器中打开 `Jenkins.xml`
  - 找到 `"--httpPort=8080"` 并将端口 `8080` 替换为新端口号

## 其工作原理...

首先，你安装了 Ubuntu 的虚拟镜像，更改了密码以使其更难被他人登录，并为安全补丁更新了客户机操作系统。

## 在 Jenkins 中配置 JAVA_HOME

Jenkins 是一个开源自动化服务器，可用于为多种编程语言编写的项目配置持续集成。让我们考虑一个基于 Java 的应用程序的情况。

我们需要告诉 Jenkins Java 安装在何处。

## 如何操作...

- 1. 打开 Jenkins 仪表板。
- 2. 前往 管理 Jenkins。
- 3. 前往 全局工具配置 以配置工具、其位置和自动安装程序。
- 4. 前往 JDK 部分。
- 5. 输入名称并勾选 自动安装 选项；提供 Oracle 账户的详细信息以成功下载 JDK。
- 6. 你可以给出 JDK 1.7 或 JDK 1.8 等逻辑名称，以便在配置构建作业时识别正确版本。
- 7. 你可以根据版本添加多个 JDK，因此如果不同应用程序需要不同的 JDK，则可以通过在 Jenkins 中添加 JDK 轻松管理该场景：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_101_0.png)

## 其工作原理...

当你在 Jenkins 中创建构建作业并配置它时，你需要指定构建执行将使用的 Java 版本。如果你不想自动安装，也可以使用系统上已有的 Java。

在构建作业的常规部分，我们可以从列表中选择一个 JDK。该列表包含我们在全局工具配置中配置的所有 JDK。

## 在 Jenkins 中配置 Git

Jenkins 可以与许多源代码存储库集成，Git 就是其中之一。让我们考虑一个基于 Java 的应用程序的情况，其源代码存储在 Git、Gitlab 和/或 GitHub 中。

我们需要告诉 Jenkins Git 在本地系统中的安装位置。

## 准备工作

根据操作系统下载所需的 Git 版本，或自动安装。

## 如何操作...

- 1. 打开 Jenkins 仪表板。
- 2. 前往 管理 Jenkins。
- 3. 前往 全局工具配置 以配置工具、其位置和自动安装程序。
- 4. 前往 Git 部分。
- 5. 输入名称并点击 自动安装，或提供 Git 的路径。
- 6. 你可以根据版本或特定代理添加多个 Git。你需要给出有意义的名称，以便在配置构建作业时易于识别：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_105_0.png)

## 其工作原理...

当你在 Jenkins 中创建构建作业并配置它时，你需要指定构建执行将使用的 Git 版本。如果你不想自动安装，也可以使用系统上已有的 Git 安装。

在源代码管理部分，从列表中选择在全局工具配置中配置的适当 Git。

前往 Git 可执行文件配置以更改 Git 安装程序。

