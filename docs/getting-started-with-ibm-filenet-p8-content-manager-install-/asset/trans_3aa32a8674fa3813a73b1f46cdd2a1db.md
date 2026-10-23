
修复包将包含一组安装说明。我们不会详细讨论这些说明，但在安装特定修复包时，你应通读一遍。我们接下来演示的过程相当通用，而且修复包说明有时会包含重要的特殊情况或限制。修复包通常无法卸载，因此务必做好备份或准备其他恢复计划，以防遇到问题或决定放弃安装某个修复包。

## CE 4.5.1.4 服务器

我们的第一步是升级 CE 服务器和客户端组件。CE 服务器运行在我们 Linux 服务器上的一个 WAS profile 中。CE 客户端更新必须针对以下环境执行：位于独立 WAS profile 中的 CE 安装、我们 Windows 服务器上运行的 PE 服务器，以及任何安装了 FEM 的 Windows 机器。如果你搭建了 Java 或 .NET 开发环境，那些机器也需要进行客户端更新。我们将先更新 CE 服务器，然后应用客户端更新。这是 CE 更新支持的操作顺序。

> 在对 CE 服务器进行任何更改之前，请确保你已安全保存了引导后的 CE EAR 文件的副本。如果你在第 3 章“安装 Content Engine”中还没有这样做，那么现在请完成。CE EAR 文件将命名为 Engine-ws.ear、Engine-wl.ear、Engine-jb.ear 或 Engine-jbc.ear，具体名称取决于你的应用服务器品牌（分别是 WebSphere、WebLogic 或 JBoss）。如果你正从部署位置复制副本，你的应用服务器可能已将 CE EAR 重命名为其他名称。不要跳过此步骤。如果你丢失了原始的 bootstrap 设置，许多本可恢复的错误将变得无法恢复。

下载并解压 CE 服务器修复包更新。在解压后的目录中，你会找到的文件之一是 version.txt。这个简短的文件精确描述了所包含组件的确切构建版本。以下是我们 CE 修复包中 version.txt 的内容：

```
P8 Content Engine 4.5.1.4 build dap451.004.002
Using pui451.057
Using pch455.004
Using cm451.002.008
Using dep451.074
This build includes cambridge14.mdb revision
```

了解这些内容各自的含义并不是很重要。它们只是子组件的内部构建编号标识，并非所有这些标识都会在已安装的产品中实际可见。稍后我们将利用这些信息来复核修复包安装是否符合预期。

在 CE 服务器关闭的状态下，运行 CE 服务器修复包安装程序。在我们的案例中，可执行文件名为 4.5.1.4-P8CE-FP004.BIN。虽然这是一个图形化安装程序，但我们建议你从 shell 窗口启动它，以防有任何重要的控制台消息被打印出来。有几个安装程序面板仅适用于 Windows 环境，而且你可能希望为 CE 安装使用默认位置。这意味着你可以直接点击浏览大部分面板。

在最后一个面板上，安装程序建议：如果你正在使用 PE 和 CSE 这些组件并且有可用更新，请运行它们的客户端更新程序。换句话说，如果 CE 服务器使用了这些组件，安装程序会要求你更新它们。我们两者都在使用，但只有 CSE 有更新，因此我们接下来将处理这个问题。确保取消选中“启动 Configuration Manager”复选框，然后点击“完成”按钮。

你应该查看安装日志文件（位于目录 /opt/ibm/FileNet/ContentEngine/ 中），以确认是否发生了任何问题。

## CSE 4.5.1.1 客户端

解压 CSE 修复包 4.5.1.1 的 Linux 客户端包。此情况下的安装程序可执行文件名为 4.5.1.1-P8CSE-CLIENT-LINUX-FP001.BIN。同样，我们建议你从 shell 窗口运行该程序以启动图形化安装程序。此安装程序将更新 CE 服务器 EAR 文件中的 CSE 客户端文件。假设你使用默认路径，你可以直接点击浏览安装程序面板，它就会顺利完成。

> 在 CSE 修复包的发行说明中，你应该留意的一点是它是否需要对你的内容重新建立索引。通常不需要，但也有可能需要。对于我们这个极小的多合一站点来说，这不算什么问题。但对于一个拥有数百万甚至更多文档的生产站点，重新建立索引所需的时间将相当可观。

CSE 客户端文件更新将放置到 /opt/ibm/FileNet/ContentEngine/CSEClient/ 中。你应该快速浏览一下该目录中的安装日志文件，以确认没有出现问题。

## CSE 4.5.1.1 服务器

由于我们刚刚安装了 CSE 客户端更新，为了保持同步，我们将安装 CSE 服务器更新。解压 CSE 修复包 4.5.1.1 的 Linux 服务器包。此情况下的安装程序可执行文件名为 4.5.1.1-P8CSE-LINUX-FP001.BIN。同样，我们建议你从 shell 窗口运行该程序以启动图形化安装程序。如果你此前没有为此设置环境变量，则需要在运行安装程序之前为 JAVA_HOME 设置一个值。以下是我们在第 5 章“安装其他组件”中使用的设置：

```
# export JAVA_HOME=/usr/lib/jvm/java-1.5.0-ibm-1.5.0_11.2.x86_64
```

> 在更新到 CSE 服务器软件期间，你必须关闭所有 CE 和 CSE 服务器。否则，CSE 可能会在更新期间尝试处理 CE 请求，从而导致文本索引损坏。

安装程序将更新 CSE 服务器本身中的软件，因此我们将首先停止 CSE 服务器。如果你正在使用类似于我们在第 2 章中使用的 up 和 down 脚本，则只需运行 down 脚本即可停止所有内容。否则，请转到 CSE 软件位置（默认是 /opt/ibm/FileNet/ContentEngine/verity/k2/_ilnx21/bin/），并运行 k2adminstop 命令。

> 快速检查 Unix 或 Linux 服务器上是否有任何 CSE 软件正在运行的方法是执行命令 ps -ef | grep k2。如果没有显示任何内容（可能除了 grep 命令本身外），则说明没有 CSE 软件正在运行。

修复包文件中包含用于备份和恢复 CSE 配置文件的脚本。你需要一个目标目录来存储这些文件（它们总计将达到 30-50 MB）。只有当你的系统在重启时不会自动清理 /tmp/ 目录时，你才可以安全地将它们放在 /tmp/ 下的某个目录中。更安全的做法是将它们存储在你解压修复包文件所在位置的一个新建子目录中。你应该以我们在第 5 章中指定的用户 k2_os_user 身份运行备份（以及之后的恢复）。

```
# su - k2_os_user -c "/some/path/backup_k2.sh /opt/ibm/FileNet/ContentEngine/verity /some/path/k2save"
```

在生产环境中，你可能会将各种 CSE 功能分散到多台服务器上。如果是这种情况，你应该在每台独立的服务器上运行备份过程和 CSE 安装程序。

重新启动 CE 的 WAS profile。（如果你通过关闭所有内容而停止了 WAS profile，那么还必须重新启动目录服务器。）在 WAS 控制台中，导航到 Applications | Application Types | WebSphere enterprise applications。选择 CE 服务器（通常名为 FileNetEngine），然后点击 Uninstall 按钮。按照提示操作，并将更改保存到主配置。

如果你的 CE 服务器应用程序名为 FileNetEngine，那么它的 WAS 缓存文件将位于 /opt/ibm/WebSphere/AppServer/profiles/AppSrv02/temp/localhostNode02/server1/FileNetEngine/。最可能的情况是，你会发现在该位置只有空目录。停止 WAS profile，然后删除该 FileNetEngine 目录及其以下的所有内容。

重新启动 WAS profile。至此，CE 服务器应用程序的所有残留痕迹都已清除。

在第 3 章中，我们最初是以 root 用户身份安装 CE 服务器的。我们也将以 root 用户身份运行 P8 Configuration Manager。稍后，我们将像在第 3 章中那样修正文件和目录的所有权。使用以下命令运行 P8 Configuration Manager：

```
# /opt/ibm/FileNet/ContentEngine/tools/configure/configmgr
```

导航到并打开初始安装时保存的配置配置文件（wjc-rhel）。你可能还记得，在第 3 章中，我们将 WAS 控制台管理员登录名从 administrator 更改为了 was_admin。该配置配置文件可能仍然反映的是较早期的 administrator 用户。点击那个看起来像电脑的图标，打开应用服务器配置设置面板。提供 was_admin 用户名和密码。使用 Test Connection 按钮以确保你的设置正确。

![](img/2295191c450a0c86b2d82df6cec04526_419_0.png)
