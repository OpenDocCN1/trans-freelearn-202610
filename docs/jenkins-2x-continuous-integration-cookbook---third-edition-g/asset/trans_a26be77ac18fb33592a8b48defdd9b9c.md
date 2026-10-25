
```text
[ INFO] Backup started at [05/27/17 15:01:27]
[ INFO] Setting hudson in shutdown mode to avoid files corruptions.
[ INFO] Waiting all jobs end...
[ INFO] Number of running jobs detected : 0
[ INFO] All jobs finished.
[ INFO] Full backup file name : d:\backup\YYYYDDMM_hhmm
[ INFO] Saved files : 12482
[ INFO] Number of errors : 0
[ INFO] Cancel hudson shutdown mode
[ INFO] Backup end at [05/27/17 15:03:51]
[ INFO] [143.935s]
```

6. 前往 Backup Manager，点击 Restore Hudson configuration：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_146_0.png)

### 另请参阅

目前，备份插件不止一个。我选择了 `thinBackup` 插件（[https://wiki.jenkins-ci.org/display/JENKINS/thinBackup](https://wiki.jenkins-ci.org/display/JENKINS/thinBackup)），因为它支持计划调度。

## 通过 Jenkins CLI 使用命令行选项

你可以通过命令行工具访问 Jenkins 中的各种功能。

### 准备工作

从 Jenkins 仪表盘上的链接下载 `Jenkins-cli.jar` 文件。

### 操作步骤...

1. 进入 `Jenkins-cli.jar` 的下载位置，执行 `java -jar jenkins-cli.jar -s http://localhost:8080/ help` 命令查看 CLI 命令的帮助信息：

```text
C:\Users\Mitesh\Downloads>java -jar jenkins-cli.jar -s http://localhost:8080/ help
  add-job-to-view
    Adds jobs to view.
  build
    Builds a job, and optionally waits until its completion.
  cancel-quiet-down
    Cancel the effect of the "quiet-down" command.
  clear-queue
    Clears the build queue.
  connect-node
    Reconnect to a node(s)
  console
    Retrieves console output of a build.
  copy-job
    Copies a job.
  create-credentials-by-xml
    Create Credential by XML
  create-credentials-domain-by-xml
    Create Credentials Domain by XML
  create-job
    Creates a new job by reading stdin as a configuration XML file.
  create-node
    Creates a new node by reading stdin as a XML configuration.
  create-view
    Creates a new view by reading stdin as a XML configuration.
  declarative-linter
    Validate a Jenkinsfile containing a Declarative Pipeline
  delete-builds
    Deletes build record(s).
  delete-credentials
    Delete a Credential
  delete-credentials-domain
    Delete a Credentials Domain
  delete-job
    Deletes job(s).
  delete-node
    Deletes node(s)
  delete-view
    Deletes view(s).
```

2. 执行以下命令：

```text
java -jar jenkins-cli.jar -s http://localhost:8080/ version
java -jar jenkins-cli.jar -s http://localhost:8080/ who-am-i
java -jar jenkins-cli.jar -s http://localhost:8080/ list-jobs
```

3. 验证输出，如下所示：

```text
C:\Users\Mitesh\Downloads>java -jar jenkins-cli.jar -s http://localhost:8080/ version
2.73

C:\Users\Mitesh\Downloads>java -jar jenkins-cli.jar -s http://localhost:8080/ who-am-i
Authenticated as: anonymous
Authorities:
anonymous

C:\Users\Mitesh\Downloads>java -jar jenkins-cli.jar -s http://localhost:8080/ list-jobs
AzureAppDeploy
CucumberProject
CucumberProject2
FirstAndroidProject
FirstAntExample
FirstJob
FirstPipeline
Main-PetClinic
Maven-Sample
mitesh51
PetClinic-Code
PetClinic-Deploy
PetClinic-FuncTest
PetClinic-LoadTest
PetClinic-Package
PetClinic-Prod
PetClinicWebHook
SonarHTMLCSSJS
SpringBoot
Test
TestPipeline
ZapTest
ZapTestBackup

C:\Users\Mitesh\Downloads>
```

4. 请自行尝试其他命令。

## 从命令行修改 Jenkins 配置

你可能对 Jenkins 工作区顶层的 XML 文件感到好奇。这些是配置文件。`config.xml` 是处理默认服务器值的主要文件，但任何通过 GUI 设置值的插件也有对应的特定配置文件。

工作区下还有一个 `jobs` 子目录。每个单独的任务配置都包含在与任务同名的子目录中。任务特有的配置存储在子目录内的 `config.xml` 中。`users` 目录的情况类似：每个用户都有一个子目录，个人信息存储在 `config.xml` 中。

在受控情况下，如果你基础设施中的所有 Jenkins 服务器具有相同的插件和版本级别，你可以在一台测试机上测试，然后将配置文件推送到所有其他机器。然后你可以使用**命令行界面（CLI）**重启 Jenkins 服务器。

本教程将帮助你熟悉主要的 XML 配置结构，然后根据 XML 的细节提供有关插件 API 的提示。

### 准备工作

你需要一台启用了安全性的 Jenkins 服务器，并且能够编辑文件，可以通过登录后使用命令行操作，也可以使用文本编辑器进行编辑。

### 操作步骤...

让我们看看如何修改 Jenkins 配置。

1. 在 Jenkins 的顶层目录中，找到 `config.xml` 文件。编辑包含 `numExecutors` 的那一行，将数字 2 改为 3：

   ```text
   <numExecutors>3</numExecutors>
   ```

2. 重启服务器。你会看到执行器数量从默认的两个增加到三个。

3. 插件通过 XML 文件持久化其配置。为了证明这一点，请查找 `thinBackup.xml` 文件。除非你已安装 `thinBackup` 插件，否则你找不到它。

### 工作原理...

Jenkins 使用 **XStream**（[http://xstream.codehaus.org/](http://xstream.codehaus.org/)）将其配置持久化为可读的 XML 格式。工作区中的 XML 文件是插件、任务以及其他各种持久化信息的配置文件。`config.xml` 是主配置文件。安全设置和全局配置都在此处设置，并反映通过 GUI 所做的更改。插件使用相同的结构，XML 值对应于底层插件类中的成员值。GUI 本身是通过 Jelly 框架（[http://commons.apache.org/jelly/](http://commons.apache.org/jelly/)）从 XML 创建的。

通过重启服务器，你可以确保任何配置更改都会在初始化阶段被加载。

也可以使用“管理 Jenkins”页面中的**从磁盘重新加载配置**，无需重启即可加载更新后的配置。

## 管理磁盘使用情况

Jenkins 存储多个任务，并且创建的多个构建也存储在机器上。跟踪磁盘使用情况非常重要，因为随着时间的推移，多个项目的构建会占用大量空间。

### 准备工作

转到 Dashboard | Manage Jenkins | Manage Plugins | Available | 安装 Disk Usage 插件。

### 操作步骤...

该插件提供 Jenkins 安装所在系统的磁盘使用情况详细信息。

1. 转到 Dashboard | Manage Jenkins | Disk Usage：

2. 转到 Manage Jenkins，点击 Configure System。转到 Disk usage 部分，勾选 Show disk usage trend graph on the project page：

   **磁盘使用**

   - 启用构建计算
   - 计算时间间隔
   - 启用任务计算
   - 计算时间间隔
   - 启用工作区计算
   - 计算时间间隔
   - 当某些大小超限时发出警告
   - 在项目页面显示磁盘使用趋势图
   - 在全局图表中显示任务目录的可用空间
   - 全局磁盘使用历史的长度
   - 计算从节点工作区的超时时间（分钟）
   - 也从从节点端控制工作区
   - 排除在磁盘使用计算之外的任务

3. 转到一个特定项目，验证 Disk Usage Trend 图表是否可用：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_160_0.png)

## 安全关闭 Jenkins

是否有可能停止执行新的构建，以便最终可以安全地关闭系统？让我们在下面的教程中了解。
