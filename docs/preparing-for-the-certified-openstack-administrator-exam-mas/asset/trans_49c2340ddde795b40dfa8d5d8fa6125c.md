
## Horizon 仪表盘

请参阅图 8.4。让我们在 Horizon 仪表盘上创建一个新的容器。

![](img/da864e0670c65edf6f5ab861ef9906b3_274_0.png)

图 8.4：在 Horizon 仪表盘上创建 Swift 容器

- 1. 从 OpenStack 概览页面，通过选择顶部下拉菜单将自己限定到 **operations** 项目。
- 2. 从 **项目-对象存储** 面板组中选择 **容器** 面板。
- 3. 选择 **容器**。
- 4. 请参阅图 8.5。**容器名称（必填）**：为您的容器命名。输入 `container-dashboard`。
- 5. **容器公开访问（必填）**：使此容器的内容对公众可访问。这意味着世界上的任何人都可以浏览容器内的任何对象（前提是 swift API 端点是公开可访问的）。让我们保持此项 **未选中**，从而使容器保持私有。默认情况下，私有容器的对象仅可供具有 **admin** 或 **swiftoperator** 角色的用户访问。我们将在下一节中了解更多关于 ACL 的内容。
- 6. 选择 **创建**。

![](img/da864e0670c65edf6f5ab861ef9906b3_275_0.png)

图 8.5：在 Horizon 仪表盘上设置容器名称和访问权限

创建容器后，我们现在将向其上传一个对象。

在本地系统上创建一个名为 `object-dashboard.txt` 的文本文件。在文本文件中，只需写入 `object-dashboard` 并保存文件。

选择 `object-dashboard.txt` 文件并选择 **上传文件** 图标。如果文件成功上传，您应该会看到类似于图 8.6 的内容。图 8.7 显示了我们的对象在其容器、项目和域上下文中的可视化表示。

![](img/da864e0670c65edf6f5ab861ef9906b3_275_1.png)

如果您想上传大于 5 GB 的文件，必须使用 `python-swiftclient`。

如果我们想下载此文件，可以通过选择 **下载** 轻松完成：

![](img/da864e0670c65edf6f5ab861ef9906b3_276_0.png)

图 8.6：Horizon 仪表盘上包含一个对象的私有 Swift 容器

小心！Swift 会自动覆盖您上传的任何文件名和文件扩展名完全相同的文件。我们可以通过启用对象版本控制来更改此行为。这超出了 COA 考试准备的范围，但您可以在 https://docs.openstack.org/swift 了解更多信息。

![](img/da864e0670c65edf6f5ab861ef9906b3_276_1.png)

图 8.7：在项目和域上下文中位于容器内的 Swift 对象

## CLI

令人惊讶的是，python-openstackclient 目前没有设置容器 ACL 所需的功能，尽管这将在未来版本中提供。为了保持一致并简化操作，我将向您展示如何使用 python-swiftclient，它包含在 python-openstackclient 中。请记住，提供的 openrc 文件包含以变量形式提供的凭据，用于正确地向 Keystone 进行身份验证并设置项目范围。在本章中，我们将在 operations 项目中执行所有与 Swift 相关的任务。让我们将原始的 openrc 文件复制到一个名为 openrc-operations 的新文件中：

```
$ cp openrc openrc-operations
```

现在我们已经制作了副本，让我们编辑 openrc-operations 文件并更新项目和租户名称变量，以反映我们希望限定到 logistics 项目的范围。如果您更习惯使用其他编辑器，请在此处随意使用。

```
$ nano openrc-operations
```

将以下变量更改为 operations：

```
export OS_PROJECT_NAME=operations
export OS_TENANT_NAME=operations
```

我们更改 OS_TENANT_NAME 变量，因为某些较旧的基于服务的客户端可能依赖于它。

![](img/da864e0670c65edf6f5ab861ef9906b3_277_0.png)

导入新创建的 openrc-operations 文件：

```
$ source openrc-operations
```

验证是否已正确设置变量：

```
$ export | grep OS
```

现在我们可以继续列出 operations 项目拥有的容器：

```
$ swift list
```

我们应该会看到 container-dashboard 容器。如果我们想列出此容器内的所有对象，可以运行以下命令：

```
$ swift list container-dashboard
```

我们应该会看到上一个练习中的 object-dashboard.txt 文件。要将文件下载到当前工作目录，可以运行以下命令：

```
$ swift download container-dashboard object-dashboard.txt
```

现在让我们创建一个新的容器：

```
$ swift post container-cli
```

创建一个名为 object-cli.txt 的新对象：

```
$ touch object-cli.txt
```

让我们在文件中添加文件名：

```
$ echo "object-cli" > object-cli.txt
```

现在我们可以将对象上传到容器：

```
$ swift upload container-cli object-cli.txt
```

我们可以通过运行以下命令来确认容器和文件是否已成功上传：

```
$ swift list container-cli
```

让我们创建并上传另一个对象：

```
$ touch object-deleteme.txt
```

让我们将此文件上传到我们的容器：

```
$ swift upload container-cli object-deleteme.txt
```

确认文件是否已成功上传：

```
$ swift list container-cli
```

现在让我们删除此文件：

```
$ swift delete container-cli object-deleteme.txt
```

创建容器、向这些容器上传对象以及删除对象就是这么简单！非常容易！

## 考试目标：管理容器 ACL

请记住，默认情况下，容器的内容只能由具有 admin 或 swiftoperator 角色的人访问。如果某人拥有其中任一角色，他们就有权限设置 ACL。ACL 在容器级别工作，并适用于容器内的所有对象。

- Read ACL 控制谁可以读取/下载容器内的文件。您还可以将读取/下载访问权限限制为特定项目内的所有用户或一个特定用户。
- Write ACL 允许您为容器设置上传权限。例如，您可能希望允许特定用户或项目内的所有用户向容器上传文件。您甚至可以将容器设置为匿名写入，从而允许任何人，无论项目如何，都可以向容器上传对象。

![](img/da864e0670c65edf6f5ab861ef9906b3_279_0.png)

只能使用原始 API 或通过 python-swiftclient 设置 ACL。唯一可以通过 Horizon 仪表盘设置的 ACL 是通过图 8.6 中的 Public 复选框允许匿名访问容器。

## CLI

![](img/da864e0670c65edf6f5ab861ef9906b3_279_1.png)

小心！python-swiftclient 帮助（swift post --help）关于 ACL 语法的信息不正确！错误修复已经提交，但在考试期间不要依赖它！请确保遵循此处或官方社区文档 https://docs.openstack.org/ocata/ 中的 ACL 语法！

让我们导入我们的 openrc-operations 文件：

```
$ source openrc-operations
```

验证是否已正确设置变量并且您已限定到 operations 项目：

```
$ export | grep OS
```

让我们首先列出我们当前的容器。我们应该能够看到我们在上一节中创建的 container-cli 容器：

```
$ swift list
```

要查看此容器的 ACL，请运行以下命令：

```
$ swift stat container-cli
```

我们应该会看到类似于图 8.8 的输出。

![](img/da864e0670c65edf6f5ab861ef9906b3_280_0.png)

图 8.8：'swift stat' 命令的输出显示容器上未设置读取或写入 ACL

如您所见，Read ACL 和 Write ACL 为空。这意味着容器当前为私有，仅可供具有 admin 或 swiftoperator 角色的人访问。

让我们开始使容器公开可访问，以便互联网上的任何人都可以从中下载文件。让我们分解 Read ACL 的语法。

看看以下语法：

```
$ swift post container-cli --read-acl ".r:*,.r:listings"
```

.r 指的是 referrer 头。referrer 头是链接到所请求资源的网页的地址。换句话说，如果有人在 https://www.openstack.org/ 上链接到此容器中的对象，则向 Swift 请求该对象的请求将包含 referrer 头中的 openstack.org。通配符星号 (*) 表示 referrer 头可以包含任何值，因此可以来自 Swift 链接可能发布的任何网站。语法中的 .r:listings 使容器能够列出容器中的所有对象。现在让我们运行此命令！

设置 ACL 时您不会得到任何确认输出，因此我们可以运行以下命令来验证 read ACL 是否已应用：

```
$ swift stat container-cli
```

我们应该会看到类似于图 8.9 的输出。

![](img/da864e0670c65edf6f5ab861ef9906b3_281_0.png)

图 8.9：设置公共读取 ACL 后 'swift stat' 命令的输出

为了验证容器是否公开，我们需要从服务目录中获取公共 URL。

运行以下命令：

```
$ openstack catalog list
```

找到 Swift 公共端点并将其复制并粘贴到浏览器中。确保在末尾附加容器名称，使其看起来类似于这样：

```
http://192.168.56.56:8080/v1/AUTH_c03401d3e99f4d4ea1aede321b7a61ff/container-cli
```
