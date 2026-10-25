

## 解锁 Jenkins

为确保 Jenkins 由管理员安全设置，已将密码写入日志（不确定在哪里找到？）以及服务器上的此文件：
/var/lib/jenkins/secrets/initialAdminPassword
请从上述任一位置复制密码并粘贴到下方。
管理员密码

29. 在 PuTTY 窗口中，执行 cat 命令以获取“管理员密码”部分提到的文件的详细信息。
30. 将该密码替换到“管理员密码”框中，然后点击“继续”：

```
mitesh51@Jenkins:~$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
e0a0e477a2234e7bb45061173512eeff
mitesh51@Jenkins:~$
```

31. 点击“安装建议的插件”：

## 自定义 Jenkins

插件为 Jenkins 扩展了额外功能，以支持许多不同的需求。

### 安装建议的插件
安装 Jenkins 社区认为最有用的插件。

### 选择要安装的插件
选择并安装最适合您需求的插件。

Jenkins 2.60.1

32. 所有插件成功安装后，创建第一个管理员用户。点击“保存并完成”，如下一个截图所示：

## 创建第一个管理员用户

用户名：mitesh51
密码：··········
确认密码：··········
全名：Mitesh P
电子邮件地址：

Jenkins 2.60.1 以管理员身份继续 保存并完成

33. 现在，Jenkins 设置已完成。点击“开始使用 Jenkins”：

[24]

![](img/2a4d2e09d9af2c678ea8f43ea344059d_54_0.png)

34. 最后，我们来到了仪表盘界面：

## 欢迎使用 Jenkins！

[登录以创建新任务。](https://mjjjenkins.centralindia.cloudapp.azure.com/login?from=%2F)

+   1 空闲
+   2 空闲

页面生成时间：2017年7月18日 下午6:06:58 UTC [REST API](/api/) Jenkins 版本 2.60.1

## 工作原理...

在幕后，该模板基于最佳实践创建了托管 Jenkins 所需的全部资源，包括虚拟网络、网络安全组、虚拟机等；然后 Jenkins 会在 Microsoft Azure 的安全环境中安装和配置。

## 在 Windows 中将 Jenkins 安装为服务

将 Jenkins 安装为 Windows 服务后，您可以在机器启动时立即启动 Jenkins，并且不受交互使用 Jenkins 的用户身份影响。

## 准备工作

在 Windows 上安装并配置 Jenkins。

## 操作步骤...

按照以下步骤在 Windows 中将 Jenkins 安装为服务：

+   1. 转到 Jenkins 仪表盘。
+   2. 点击“管理 Jenkins”。
+   3. 点击“安装为 Windows 服务”，如下图所示：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_59_0.png)

+   4. 将“安装目录”保留为默认值，然后点击“安装”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_59_1.png)

+   5. “Windows 服务”安装成功完成后，点击“是”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_59_2.png)

+   6. 现在，Jenkins 将作为 Windows 系统中的一个服务运行。

## 工作原理...

转到“控制面板”并搜索“服务”。

点击“服务（本地）”进行查看：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_60_0.png)

一旦 Jenkins 服务可用，您可以根据需要右键单击它并启动、停止或重启服务。

## 在 Jenkins 中安装插件

Jenkins 是一个开源自动化服务器，由于拥有 400 多个插件，它被广泛使用且具有高度扩展性。插件使 Jenkins 与其他工具的集成变得非常容易。您也可以创建自己的插件。

## 准备工作

可用的插件有各种类别，例如源代码管理、从属启动器和控制器、构建触发器、构建工具、构建通知、构建报告、其他构建后操作、外部站点/工具集成、UI 插件、身份验证和用户管理、Android 开发、iOS 开发、.NET 开发、Ruby 开发、库插件等等。

## 操作步骤...

+   1. 转到 Jenkins 仪表盘。点击“管理 Jenkins”，然后点击“管理插件”：

## 管理 Jenkins

+   - 配置系统
+   配置全局设置和路径。
+   - 配置全局安全性
+   保护 Jenkins：定义允许访问/使用系统的用户。
+   - 配置凭据
+   配置凭据提供程序和类型
+   - 全局工具配置
+   配置工具、它们的位置和自动安装程序。
+   - 从磁盘重新加载配置
+   丢弃内存中所有已加载的数据，并从文件系统重新加载所有内容。当您直接在磁盘上修改配置文件时很有用。
+   - 管理插件
+   添加、删除、禁用或启用可扩展 Jenkins 功能的插件。
+   - 系统信息
+   显示各种环境信息以协助故障排除。
+   - 系统日志
+   系统日志捕获与 Jenkins 相关的来自 `java.util.logging` 的输出。
+   - 负载统计
+   检查资源利用率，查看是否需要为构建添加更多计算机。

2. 转到“可选插件”选项卡，选择要安装的任何插件。点击“无重启安装”：

## 更多内容...

Jenkins 定义了用于建模构建系统某一方面的接口或抽象类。接口或抽象类定义了需要实现内容的协定；Jenkins 使用插件来扩展这些实现。

## 在 Jenkins 中上传插件

有些情况下，您已经从 [https://updates.jenkins-ci.org/download/plugins/](https://updates.jenkins-ci.org/download/plugins/) 下载了所有插件，或者您创建了自己的插件并希望在 Jenkins 中使用。本教程将说明如何在 Jenkins 中上传这些插件。

## 准备工作

下载所需插件 https://updates.jenkins-ci.org/download/plugins/ 或使用插件教程 https://wiki.jenkins.io/display/JENKINS/Plugin+tutorial 创建插件。

## 操作步骤...

在本教程中，我们将下载一个插件并从 Jenkins 仪表盘上传它：

+   1. 下载 copyartifact 插件（`.hpi`）文件：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_71_0.png)

+   2. 将最新版本下载到您的系统：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_71_1.png)

+   3. 转到 Jenkins 仪表盘。
+   4. 点击“管理 Jenkins”。
+   5. 点击“管理插件”。
+   6. 转到“高级”选项卡。在“上传插件”部分点击“选择文件”：

## 工作原理...

一旦插件成功上传，我们就可以利用上传到 Jenkins 的插件所提供的扩展性。

## 在 Jenkins 中配置代理

Jenkins 通常安装在使用代理服务器的组织中。在这种情况下，在 Jenkins 仪表盘中配置代理非常重要，这样插件更新才能成功安装。

## 准备工作

获取服务器名称、端口号以及允许访问代理服务器的凭据等所有详细信息。

## 操作步骤...

+   1. 转到 Jenkins 仪表盘。
+   2. 点击“管理 Jenkins”。
+   3. 点击“管理插件”。
+   4. 转到“高级”选项卡。
+   5. 提供“服务器”、“端口”、“用户名”、“密码”和“无代理主机”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_77_0.png)

+   6. 点击“提交”。

## 在 Jenkins 中配置全局设置

“配置系统”是进行初始设置的地方，并且非常有用。您可以配置 SMTP 服务器详细信息、SonarQube 服务器详细信息、环境变量等。

## 准备工作

要配置全局设置和路径，我们需要进入 Jenkins 仪表盘中的“配置系统”。

## 操作步骤...

+   1. 转到 Jenkins 仪表盘。
+   2. 点击“管理 Jenkins”。
+   3. 点击“配置系统”。
+   4. 验证“主目录”是否可用：

5. 在“配置系统”页面中，我们也可以定义环境变量，以便在构建作业执行期间使用它们。

6. 例如，我们可以在“环境变量”中配置 ANDROID SDK 路径，以便为 Android 应用设置持续集成。

# 全局属性

环境变量

变量列表

名称 PROP_LOC

值 <属性文件的路径>

删除

添加

7. 同一页面上另一个重要的配置是“Jenkins 位置”。您可以指定 Jenkins 安装的 HTTP 地址，因为 Jenkins 无法可靠地从自身检测到这样的 Jenkins URL：

# Jenkins 位置

Jenkins URL http://10.0.0.5:8080/

系统管理员电子邮件地址 尚未配置 <nobody@nowhere>

