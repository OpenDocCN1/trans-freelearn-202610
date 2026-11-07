
## 创建用户

描述：
创建新用户并设置相关属性，包括主项目和角色

- 4 域 ID
- 5 域名称
- 6 用户名
- 7 描述
- 8 电子邮件
- 9 密码 *
- 10 确认密码 *
- 11 主项目
- 12 角色
- 13 已启用

![](img/da864e0670c65edf6f5ab861ef9906b3_106_0.png)

图 3.22：在 Horizon 仪表板上添加有关新 OpenStack 用户的详细信息

参见图 3.23。你现在应该在 Horizon 仪表板的用户列表中看到 johnnyc-dashboard。

![](img/da864e0670c65edf6f5ab861ef9906b3_107_0.png)

图 3.23：新创建的用户将出现在用户列表中

你可以通过选择图 3.23 中显示的编辑按钮轻松更新用户的姓名、描述、电子邮件和项目。要删除或禁用帐户，请使用编辑按钮旁边的下拉菜单。

### 命令行界面

我们可以使用 python-openstackclient 通过 CLI 添加用户。确保已加载管理员用户凭据：

```
$ source openrc
```

我们将创建一个名为 johnnyc-cli 的新用户。

在运行命令之前，请探索 user create 命令的帮助：

```
$ openstack help user create
```

现在让我们通过 python-openstackclient 创建用户：

```
$ openstack user create --domain default --project trade \
--description "Developer" --email johnnyc@example.com \
--password openstack johnnyc-cli
```

![](img/da864e0670c65edf6f5ab861ef9906b3_107_1.png)

由于用户默认已启用，因此无需使用 enable 标志。要在创建时禁用用户，请使用 --disable 标志。

确认用户已成功创建：

```
$ openstack user list
```

johnnyc-cli 用户在项目中拥有角色之前将无法创建资源。

运行以下命令。你不会收到任何输出。

```
$ openstack role add --user johnnyc-cli --project trade _member_
```

通过运行以下命令验证 _member_ 角色是否已分配给 trade 项目上的用户：

```
$ openstack role assignment list --names
```

## 考试目标 - 创建域、项目、组和角色

请记住，只要在禁用多域的情况下登录 Horizon 仪表板，我们就会登录到默认域。我们可以通过 Horizon 仪表板在默认域内轻松创建新的项目、组和角色，但除非使用 python-openstackclient，否则无法在默认域之外的域下创建它们。

让我们深入了解该过程。

### 命令行界面

我们将仅关注考试目标，而不涉及多域环境的复杂用例。

要通过 python-openstackclient 创建名为 acme-corp 的新域，请运行以下命令：

```
$ openstack domain create acme-corp
```

现在我们可以确认域已创建：

```
$ openstack domain list
```

让我们在这个域中创建一个名为 edgarp 的新用户：

```
$ openstack user create --domain acme-corp edgarp --password openstack
```

现在我们可以继续在 acme-corp 域内创建一个新项目。让我们创建一个名为 development 的项目：

```
$ openstack project create --domain acme-corp development
```

现在让我们在 acme-corp 域中创建一个名为 interns 的新组：

```
$ openstack group create --domain acme-corp interns
```

现在让我们将 edgarp 添加到该组：

```
$ openstack group add user --group-domain acme-corp interns edgarp
```

验证 edgarp 是否在 interns 组中：

```
$ openstack group contains user --group-domain acme-corp interns edgarp
```

为了让 edgarp 访问 acme-corp 域内的 development 项目，我们必须将角色应用于 development 项目上的 interns 组：

```
$ openstack role add --project-domain acme-corp --project development --group interns _member_
```

此命令不会产生任何输出。我们可以通过首先验证 interns 组在 acme-corp 域内的 development 项目上是否具有 _member_ 角色来确认它是否正常工作：

```
$ openstack role assignment list --group interns --names
```

图 3.24 显示了我们刚刚创建内容的视觉表示：

![](img/da864e0670c65edf6f5ab861ef9906b3_110_0.png)

图 3.24：acme-corp 域内名为 development 的项目。edgarp 用户在 development 项目上具有 _member_ 角色

![](img/da864e0670c65edf6f5ab861ef9906b3_110_1.png)

由于 Horizon 仪表板的多域已关闭，edgarp 需要依赖 CLI 进行 acme-corp 域的身份验证。

## 考试目标 - 管理服务目录 - 服务和端点

对于 OpenStack 新手来说，服务目录的概念令人困惑，因为大多数初级 OpenStack 用户和管理员通过仪表板或 CLI 间接与 OpenStack 服务 API 交互。

你可以将目录视为存储在 Keystone 中的 URL 列表。该列表显示了 OpenStack 环境中当前安装的所有服务的 API URL（也称为 *端点*）。虽然不是考试目标的一部分，但让我们从宏观角度看看直接与 API 交互的用户如何使用服务目录：

- 1. OpenStack 操作员/开发人员决定直接与 API 交互，因此他们直接从 OpenStack 管理员处接收 Keystone 身份验证 URL、用户名、密码、域和项目。他们使用工具（如 `curl` 或 `wget`）通过 API 请求将此信息发送到 Keystone。
- 2. Keystone 回复两件事：一个限定到特定项目的令牌和一个服务目录。
- 3. 用户浏览服务目录的方式类似于在最喜欢的餐厅浏览菜单，决定购买哪种餐点。在这种情况下，他们决定要使用哪种服务。服务目录显示 OpenStack 环境中所有服务 API 的位置。如果他们决定要创建一个新实例，他们会从 `http://developer.openstack.org` 提供的 Nova API 文档中查找如何进行 API 调用。
- 4. 然后 OpenStack 操作员/开发人员将他们在 **步骤 2** 中收到的令牌连同创建服务器的 API 请求一起直接发送到 Nova API 公共 URL 端点。
- 5. 如果用户的角色已被授予创建服务器的授权，Nova 服务将履行创建服务器的请求，并且用户将从 Nova API 收到 `HTTP 200 OK`——确认请求已成功完成。

某些服务使用目录查找如何向其他服务发送 API 调用。

直接通过 API 完成此操作很有趣。仪表板、CLI、SDK 和 Heat 的优点是它们不需要显式处理令牌、服务目录或直接 API 调用。

作为 OpenStack 管理员，您可能负责向环境用户宣传新安装的 OpenStack 服务。一旦 OpenStack 基础设施工程师安装了该服务，您需要将其添加到服务目录中，以便用户知道该服务可用。可以通过 python-openstackclient 轻松地将新服务和端点添加到目录中。

服务目录中每个服务有三个端点：

- **Public**：用户用于外部访问 API 的 URL。这通常是外部 IP 地址（或解析为外部 IP 地址的名称）。可以想象在咖啡店或 OpenStack 环境外部的某个地方。甚至智能手机应用程序也可以使用公共端点向服务发出 API 调用。
- **Internal**：服务通常用于内部通信的 URL。这通常是只能从 OpenStack 集群内部访问的内部 IP 地址。如果用户或服务位于 OpenStack 环境内部，则无需使用公共端点。已通过 SSH 登录 OpenStack 服务器的 OpenStack 管理员或基础设施工程师也可以使用此端点。
- **Admin**：该 URL 用于公开服务的特殊仅限管理员的功能，尽管它通常与内部 URL/端口相同。事实上，Keystone 是包含的虚拟设备中唯一为管理员 URL 广告不同端口的服务（端口 35357）。Keystone 的早期版本在此端口上解锁了特殊功能，但从 Newton 版本开始，如果使用 v3 Keystone API，则 5000 和 35357 之间没有功能差异。
