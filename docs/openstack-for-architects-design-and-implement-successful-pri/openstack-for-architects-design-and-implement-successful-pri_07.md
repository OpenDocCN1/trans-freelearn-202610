## 集成平台

我们合作过的一位 OpenStack 架构师喜欢说：“没有任何 OpenStack 实现是一座孤岛”。每个部署都会与其周围的传统 IT 基础设施集成，从身份管理系统到审计系统再到计费系统。OpenStack 标准化的 API 使这些集成相对简单。本章将介绍 OpenStack 实现中可用的集成点以及许多常见的集成模式。

在本章中，我们将详细探讨以下几点：

- **身份管理（IdM）** 集成
- 置备工作流
- 计量和计费

这三种不同的平台集成展示了 OpenStack 的三种主要集成模式。将 Keystone 与 Active Directory 等 IdM 系统集成是相对规范化的——正如 Cinder 设计用于与存储阵列集成一样，Keystone 设计用于与 IdM 系统集成。Keystone 与 IdM 系统之间的契约主要隐藏在项目代码中。

另外两种集成模式展示了与 OpenStack 系统进行外部交互的两种不同方式。外部置备工作流通过其 REST API 驱动 OpenStack 系统。它们要么直接与 Neutron、Cinder、Nova 和 Glance API 交互，要么利用 Heat 编排 API 在 OpenStack 部署中创建、修改和删除对象。相比之下，计量、计费和审计系统通过与系统的输出交互来工作，它们要么监听 OpenStack 消息总线上发生的事件，要么从 Event API 消费事件。

## IdM 集成

在 2014 年的一篇博客文章中，OpenStack 开发者*内森·金德（Nathan Kinder）*著名地（且令人信服地）论证了 **Keystone 不是身份认证服务**。在这篇文章中，Kinder 描述了将 `Keystone` 服务部署在 Apache HTTP 服务器之后、使用原生模块执行身份认证的部署模式。Kinder 在文中提出了两个论点：

- 大多数人在使用 OpenStack 时会使用某种外部身份认证系统
- 参考身份认证系统（SQL 插件）不具备我们从身份认证服务所期望的任何功能

我们自身的经验证明了这两个论点；我们合作的大多数组织所着手处理的第一个集成就是 IdM 服务的集成。

## OpenStack 中的身份认证与授权

了解 OpenStack 中的身份认证和授权机制是很有帮助的。每次对 OpenStack API 服务的调用都通过一个 bearer token 进行授权，该 token 由 `Keystone` 服务获取并验证。下图总结了这一交互过程：

![](img/2f752555f69992669defa8b25e6dcee9_141_0.png)




当客户端向 Keystone 请求令牌时，会同时发送所请求的令牌范围：租户、项目或域。如果凭据能够授权客户端，Keystone 返回的令牌将特定于所请求的范围。一旦客户端获得令牌，向其他 OpenStack 服务发送的每个 API 请求都会在 HTTP 标头中包含该令牌。其他服务使用该令牌来授权客户端的请求。

目前 OpenStack 中广泛使用的 Keystone API 有两个版本：版本 2 和版本 3。在 API 版本 2 中，用户被授权访问特定的项目或**租户**。一个用户可能拥有多个租户的权限，且权限因租户而异。例如，用户 **msolberg** 在 **msolberg** 租户中可能拥有管理权限，但在 **architect** 租户中只有用户权限。API 版本 3 引入了授权域的概念，在基本租户之上提供了一层层级结构。Keystone API 版本 3 已经发布数个版本，但各个服务支持新的授权模型都需要一定时间。因此，大多数 OpenStack 部署仍使用 Keystone v2 API。

Keystone 在根据一组凭据决定是否颁发令牌时，会结合使用多个后端驱动。第一个后端驱动是身份驱动。身份驱动可以是 SQL（参考实现）或**轻量目录访问协议（LDAP）**。如果驱动设置为 LDAP，则可以使用外部目录服务对用户进行身份验证。第二个驱动是**分配（assignment）**驱动。该驱动允许用户使用 LDAP 进行身份验证，并使用 SQL 数据库分配角色。这种配置是 Active Directory 最常用的集成模式。

## 使用分离分配和身份配置 Keystone

在配置 Keystone 使用 Active Directory（或其他 LDAP 服务）进行身份验证之前，需要先在 Active Directory 端做一些更改。首先，决定如何将 Active Directory 中的对象映射到 OpenStack 中的账户。例如，您可能希望目录中拥有账户的任何用户都能访问 OpenStack 中的租户，或者您可能希望将使用权限限制在特定的用户组。如果打算限制用户，您需要创建一个组或属性来筛选这些用户。其次，需要创建一个能够查找其他用户的用户。请使用 `ldapsearch` 等工具执行测试，验证该用户是否有读取相应账户对象子树的能力。有关 Active Directory 端先决条件的更多信息，请参阅 `https://wiki.openstack.org/wiki/HowtoIntegrateKeystonewithAD`。

接下来，我们将配置 Keystone 使用 LDAP 身份后端和 SQL 分配后端。Packstack 开箱即用地支持此配置，我们只需更新 Puppet master 上的 Hiera 数据，即可让我们的安装反映新设置。下表列出了所需的设置：

| Hiera 变量 | keystone.conf 设置 | 描述 |
|---|---|---|
| CONFIG_KEYSTONE_IDENTITY_BACKEND | identity.driver | 要使用的后端（ldap） |
| CONFIG_KEYSTONE_LDAP_URL | ldap.url | AD LDAP 服务的 URI |
| CONFIG_KEYSTONE_LDAP_USER_DN | ldap.user | 用于查找账户的服务用户 |
| CONFIG_KEYSTONE_LDAP_USER_PASSWORD | ldap.password | 服务用户的密码 |
| CONFIG_KEYSTONE_LDAP_SUFFIX | ldap.suffix | 服务账户的后缀 |
| CONFIG_KEYSTONE_LDAP_USER_SUBTREE | ldap.user_tree_dn | AD 中用户账户所在的树 |
| CONFIG_KEYSTONE_LDAP_USER_OBJECTCLASS | ldap.user_objectclass | 用户账户的对象类（person） |
| CONFIG_KEYSTONE_LDAP_USER_FILTER | ldap.user_filter | 可选过滤器，用于仅选择树中的特定用户 |
| CONFIG_KEYSTONE_LDAP_USER_ID_ATTRIBUTE | ldap.user_id_attribute | 用作 Keystone 用户 ID 的属性（cn） |
| CONFIG_KEYSTONE_LDAP_USER_NAME_ATTRIBUTE | ldap.user_name_attribute | 用作 Keystone 用户名的属性（cn） |
| CONFIG_KEYSTONE_LDAP_USER_MAIL_ATTRIBUTE | ldap.user_mail_attribute | 用作 Keystone 用户电子邮件的属性（mail） |
| CONFIG_KEYSTONE_LDAP_USER_ENABLED_ATTRIBUTE | ldap.user_enabled_attribute | 确定用户是否启用的属性（userAccountControl） |
| CONFIG_KEYSTONE_LDAP_USER_ENABLED_MASK | ldap.user_enabled_mask | 用于确定账户是否启用的位掩码（2） |
| CONFIG_KEYSTONE_LDAP_USER_ENABLED_DEFAULT | ldap.user_enabled_default | 用于确定账户是否启用的默认值（512） |
| CONFIG_KEYSTONE_LDAP_USER_ATTRIBUTE_IGNORE | ldap.user_attribute_ignore | 对象上要忽略的属性（password、tenant_id、tenants） |
| CONFIG_KEYSTONE_LDAP_USER_ALLOW_CREATE | ldap.user_allow_create | 是否允许创建用户（False） |
| CONFIG_KEYSTONE_LDAP_USER_ALLOW_UPDATE | ldap.user_allow_update | 是否允许更新用户（False） |
| CONFIG_KEYSTONE_LDAP_USER_ALLOW_DELETE | ldap.user_allow_delete | 是否允许删除用户（False） |

一旦确定了适用于您的环境的相应设置，请使用相应设置更新 `/etc/puppet/hieradata/defaults.yaml`。在下次运行 Puppet 时，Keystone 将重新配置为使用 Active Directory 进行身份验证。

如前所述，让 Keystone 针对 LDAP 对用户进行身份验证的方法正在逐步淘汰，取而代之的是让 Apache HTTP 服务器自行执行身份验证，然后通过 `REMOTE_USER` 环境变量传递。默认情况下，Packstack 会将 Keystone 部署为 Apache 中的 **Web 服务器网关接口（WSGI）** 服务。有关配置 Keystone 虚拟主机对用户进行身份验证的说明，请参见 `http://docs.openstack.org/developer/keystone/external-auth.html`。

## 调配工作流

本书几乎整本都在讨论如何实施 OpenStack，而很少讨论大多数组织实际如何使用它。在本节中，我们将探讨 OpenStack 采用者通常如何调配虚拟基础设施的不同方式。首先，我们将介绍如何让用户访问 Horizon 用户界面。然后，我们将介绍通过 Nova、Cinder 和 Neutron API 进行调配。最后，我们将介绍如何使用 Heat 通过模板编排 OpenStack 基础设施。

### Horizon 用户界面

Horizon 仪表板 UI 为已经理解云计算概念并希望完全控制其已调配基础设施的用户提供了直观且标准化的界面。在期望最终用户提供一定程度支持的情况下（例如，由他们负责调度自己的备份），仪表板尤其适用。我们已在公共网站托管公司以及以向开发团队提供低成本计算环境为主要目标的私有云用例中看到过这种方式。

### 集成平台

向其最终用户提供仪表板访问权限的组织通常会在一定程度上对其进行定制。定制仪表板的外观相对简单。OpenStack 的 Liberty 版本引入了 Horizon **主题** 的概念，提供了一种更改颜色、图片和布局的标准化方式。此外，还有一种简单的机制可以启用或禁用仪表板上的各种面板。最后，还可以通过**自定义模块**覆盖与面板对应的 Python 类。我们看到一些组织在调配基础设施时，希望从用户那里收集更多信息，就会采用这种做法。有关各种选项的信息，请参阅 `http://docs.openstack.org/developer/horizon/topics/customizing.html`。

以下截图展示了 Horizon 仪表板 UI：

![](img/2f752555f69992669defa8b25e6dcee9_145_0.png)

欧洲核子研究组织（CERN）定制的 OpenStack 仪表板

虽然为 Horizon 仪表板创建自定义主题相对简单，并且对于将向最终用户提供访问权限的组织来说可能是先决条件，但覆盖组成仪表板的类却是一项成本相对较高的选择。虽然初始开发可能并不困难，但长期来看会带来支持负担。它还会增加难以自动化的额外测试要求。因此，想要根据自己的特定目标定制资源调配流程的组织通常会使用外部服务目录，由该目录代表最终用户与 API 交互。




## 使用 REST API

OpenStack REST API 的设计模式借鉴自 Amazon EC2 API，使用起来非常简单。根据编排系统所使用的语言，通常有两种交互方式。客户端可以直接使用该语言的 HTTP 或 REST 库调用 API，也可以利用过去几年中涌现的众多 **云库**。这些库可以通过处理身份验证和序列化工作来大大简化开发，但也可能限制可用的 API 操作。我们在日常中常见的使用云库包括 Java 的 Apache jclouds、Python 的 OpenStack Shade 以及 Ruby 的 Fog。

在本章中，我们将直接与 REST API 交互。让我们分解一下使用 REST API 启动虚拟实例的过程：

- 1. 客户端向 Keystone 请求令牌。
- 2. 客户端从 Nova 中检索密钥对列表。
- 3. 客户端从 Nova 中检索镜像列表。
- 4. 客户端从 Nova 中检索规格列表。
- 5. 客户端从 Neutron 中检索网络列表。
- 6. 客户端可选择在 Cinder 中创建卷。
- 7. 客户端要求 Nova 使用所选的镜像、规格、网络以及可选的卷创建实例。

实例创建后，客户端可以对其执行各种操作，包括终止。让我们使用 Python requests 库来演示此工作流程：

- 1. 首先，我们向 Keystone 请求令牌（实际的 Python 代码如下）：

```
import requests #http://docs.python-requests.org/en/latest
import json

KEYSTONE_URL = 'http://controller:5000/v2.0/'
TENANT       = 'demo'
USERNAME     = 'demo'
PASSWORD     = 'secret'

r = requests.post("%s/tokens" % (KEYSTONE_URL,), json={
    "auth": {
        "tenantName": TENANT,
        "passwordCredentials": {
            "username": USERNAME,
            "password": PASSWORD,
        }
    }
})

if r.ok:
    token = r.json()['access']['token']['id']
else:
    raise Exception(r.text)
```

- 2. requests 库通过 `json()` 函数将 Keystone 返回的 JSON 反序列化为 Python 字典，从而简化了与 REST 接口的工作。Keystone 的响应包含令牌 ID，我们将把它作为头部包含在后续请求中：

```
# Place the token in the X-Auth-Token header
headers = {'X-Auth-Token': token}
```

- 3. Keystone 的响应还包含服务目录，即指向我们想要交互的其他服务的链接集。根据配置，Keystone 可能会为多个区域提供链接。在我们的实验环境中，我们只需使用每个服务的第一个链接。以下代码将搜索目录中的计算和网络服务：

```
# Find the link to the Nova API in the service catalog:
for service in r.json()['access']['serviceCatalog']:
    if service['type'] == 'compute':
        nova_endpoint = service['endpoints'][0]['publicURL']
    if service['type'] == 'network':
        neutron_endpoint = service['endpoints'][0]['publicURL']
```

请注意，目录中的端点是针对我们进行身份验证的租户进行范围限定的。例如，如果我们的租户 ID 为 `8ce08c9fa6c54ba094be743a55dc5b9a`，我们的 Nova 端点将为 `http://10.3.71.50:8774/v2/8ce08c9fa6c54ba094be743a55dc5b9a`。因此，唯一不应动态确定的端点是 Keystone 端点。所有其他需要交互的端点都应从服务目录中获取。

- 4. 接下来，我们将查找在启动镜像时要使用的密钥对、镜像、规格和网络：

```
keypair_ref = None
# Get the list of available keypairs:
keypairs = requests.get("%s/os-keypairs" % nova_endpoint,
headers=headers)
# take the first one
```

![](img/2f752555f69992669defa8b25e6dcee9_147_0.png)

```
keypair_ref = keypairs.json()['keypairs'][0]['keypair']['name']

image_ref = None
# Get the list of available images:
images = requests.get("%s/images" % nova_endpoint,
headers=headers)
# Select the "cirros" image:
for image in images.json()['images']:
    if image['name'] == 'cirros':
        image_ref = image['id']

flavor_ref = None
# Get the list of available flavors:
flavors = requests.get("%s/flavors" % nova_endpoint,
headers=headers)
# Select the "m1.small" flavor
for flavor in flavors.json()['flavors']:
    if flavor['name'] == 'm1.small':
        flavor_ref = flavor['id']

network_ref = None
# Get the list of available networks:
networks = requests.get("%s/v2.0/networks" % neutron_endpoint,
headers=headers)
# Select the "private" network
for network in networks.json()['networks']:
    if network['name'] == 'private':
        network_ref = network['id']
```

这些 GET 请求都包含从 Keystone 检索到的令牌作为头部。密钥对和规格从 Nova API 检索，镜像也是如此，尽管它实际上存储在 Glance 中。此示例中网络引用也可在 Nova API 中获取，但我们从 Neutron API 中检索它。Neutron API 提供了有关网络对象的更多信息和控制。

> 请注意，虽然 Nova 端点包含版本号（v2），但 Neutron 端点不包含。此外，请注意 Neutron 端点不包含租户 ID。每个 OpenStack API 都是由不同的团队独立开发的，它们之间几乎没有一致性。

- 5. 以下代码将使用我们在上一节中检索到的四个引用启动实例：

```
# Launch the instance:
server = requests.post("%s/servers" % (nova_endpoint,), json={
    "server": {
        "name": "new_server",
        "imageRef": image_ref,
        "flavorRef": flavor_ref,
        "key_name": keypair_ref,
        "networks": [{
            "uuid": network_ref
        }]
    }
}, headers=headers)
server_ref = server.json()['server']['id']
```

上述行为对于 REST API 来说是正常的；这是一个带有 JSON 的 POST 请求。关于此调用有几点需要注意。首先，调用仅在 Nova 调度程序接受请求所需的时间内阻塞。成功的返回码并不意味着实例已成功启动。其次，API 返回的 JSON 并不是服务器对象的序列化表示。相反，它提供了一组指向对象表示的链接。指向对象的链接由 API 版本、租户 ID、字符串 servers 和实例 ID 组成。例如，http://192.168.0.10:8774/v2/8ce08c9fa6c54ba094be743a55dc5b9a/servers/d87f6e9d-347e-48f3-acfb-6f2e85c43629。

以下请求提供实例的状态：

```
requests.get(server.json()['server']['links'][0]['href'],
headers=headers)
```

- 6. 一旦实例创建完成，操作可以 POST 到实例的 URL 并附加字符串 action。例如，以下代码将停止一个实例：

```
server_url = server.json()['server']['links'][0]['href']
stop = requests.post("%s/action"% (server_url,), json={
  "os-stop": None
}, headers=headers)
```

- 7. 最后，要删除实例，请向服务器的 URL 发出 HTTP DELETE 请求：

```
delete = requests.delete(server_url, headers=headers)
```

与本示例中的任何请求响应一样，HTTP 状态码表示请求成功或失败。删除请求的成功状态码为 204，操作的成功状态码通常为 202。有关预期状态码、请求参数和响应参数的更多信息，请参阅 `http://developer.openstack.org/api-ref-compute-v2.1.html` 中的计算 API 文档。

## 使用模板进行编排

驱动软件定义基础设施编排工作流的服务目录的终端用户通常是应用程序开发人员。允许开发人员通过 API 部署网络、卷和实例，对于自动化的持续集成构建和部署非常有用。虽然这种方法有许多优点，但许多组织更愿意让其开发人员部署由这些元素组成的环境。首先，无论是在 API 还是 Horizon 仪表板中公开的选项数量都相当令人印象深刻。典型的部署至少会有两三个规格，四五个镜像、私有和公有网络、任意大小的卷等等。仅为启动一个实例而从所有这些选项中进行选择非常耗时。它也可能给需要支持它们的团队带来负担。一旦引入软件定义网络，终端用户可能需要了解网络、路由和负载均衡才能部署一个可用的系统。在大多数企业环境中，这是不现实的。

管理软件定义基础设施复杂性的解决方案是组合一个由模板组成的服务目录。这些模板可以由单个实例或多个实例组成，具有丰富或简单的网络拓扑，以及高度定制或标准的镜像。

编排 API 也更容易编程。让我们通过一个示例来说明：

- 1. 我们与前一个示例相同的方式开始，通过向 Keystone 请求令牌：

```
import requests #http://docs.python-requests.org/en/latest
import json

KEYSTONE_URL = 'http://controller01:5000/v2.0/'
TENANT       = 'demo'
USERNAME     = 'demo'
PASSWORD     = 'secret'

r = requests.post("%s/tokens" % (KEYSTONE_URL,), json={
    "auth": {
        "tenantName": TENANT,
```




```
"passwordCredentials": {
    "username": USERNAME,
    "password": PASSWORD,
}
})

if r.ok:
    token = r.json()['access']['token']['id']
else:
    raise Exception(r.text)

# Place the token in the X-Auth-Token header
headers = {'X-Auth-Token': token}
```

2. 然后，我们不再搜索 Compute API，而是搜索编排服务的端点：

```
# Find the link to the Heat API in the service catalog:
for service in r.json()['access']['serviceCatalog']:
    if service['type'] == 'orchestration':
        heat_endpoint = service['endpoints'][0]['publicURL']
```

Heat URL 与 Nova URL 一样，包含租户的 UUID。

3. 最后，我们将表示 Heat 模板的 JSON 字符串 POST 到该服务：

```
stack = requests.post("%s/stacks"% (heat_endpoint,), json={
    "stack_name": "new_server",
    "template": {
        "heat_template_version": "2013-05-23",
        "resources": {
            "new_server_port": {
                "type": "OS::Neutron::Port",
                "properties": {
                    "network": "private"
                }
            },
            "new_server": {
                "type": "OS::Nova::Server",
                "properties": {
                    "key_name": "demo",
                    "flavor": "m1.small",
                    "image": "cirros",
                    "networks": [ {
                        "port": {
                            "get_resource": "new_server_port"
                        }
} ]
}
}
}
}
}, headers=headers)
```

Heat 为我们执行名称到 ID 的转换，因此我们无需查找规格、镜像、密钥或网络的参考信息。调用的响应包含状态码（201 表示成功）以及指向 stack 对象的链接（stack.json()['stack']['links'][0]['href']）。Heat API 公开了一个与 Compute API 类似的 actions 链接。通过该机制，堆栈可以挂起和恢复。

不过，Heat API 最重要的特性也许是能够引用外部模板。要使用此特性，请使用 template_url 属性，而不是指定模板。请看以下示例：

```
stack = requests.post("%s/stacks" % (heat_endpoint,), json={
  "stack_name": "new_server",
  "template_url": "http://www.example.com/heat-templates/new_server.yaml"
}, headers=headers)
```

这样，模板就可以与供应代码分离。这允许模板存放在单独的版本控制仓库中，并允许不同团队分别处理目录和供应逻辑，而不必同步进行。

几乎可以通过 OpenStack API 创建的每个对象都可以使用 Heat 进行编排。有关 heat 模板的更多信息，请参阅 http://docs.openstack.org/developer/heat/template_guide/ 上的 Heat 模板指南。

## 计量和计费

无论您是在构建公有云还是私有云，系统最关键的能力之一就是跟踪用户或租户供应的虚拟对象的使用情况。OpenStack 中的使用情况跟踪方式与 Amazon Web Services 基本一致。对于计算资源，系统会跟踪特定实例的启动和终止时间。计算资源的成本与实例关联的规格中的 CPU 核心数或内存量有关。一些组织可能还会根据镜像来关联成本。

例如，Red Hat Enterprise Linux 镜像的实例化可能每小时花费一定的美分数，而 Microsoft Windows 镜像的实例化可能花费不同。其他资源可能根据供应时间计费，而不管是否使用。例如，租户可能需要为浮动 IP 分配给项目的时间长度付费，无论它是否绑定到实例。Cinder 卷通常也按这种方式处理。

无论您的组织打算按消费量向客户收费，还是仅向客户展示其消费量，都有几种不同的策略来跟踪和报告使用数据。有些组织通过服务目录调用 API 在 OpenStack 上供应资源，他们选择将服务目录实现作为记录系统。这有助于为用户保持一致性。例如，如果他们供应了一个三层应用，他们可能希望按三层应用计费，而不是按三台虚拟机、一个 Cinder 卷和一个浮动 IP 计费。不过，对于 PaaS 或持续集成用例，在一段时间内使用的计算和存储总量可能更为重要。

在 OpenStack 中跟踪使用情况有几种基本方法。系统可以监听消息总线上的事件，或通过查询 Ceilometer 获取这些事件，然后推断使用数据。系统还可以直接查询 Ceilometer 获取使用数据。除了计量和计费之外，这些方法也可用于其他用例，而且对于每种用例，有些方法比其他方法更合适。例如，监听事件并记录时间可以深入了解系统性能。此外，OpenStack 系统中的某些事件可能会驱动合规性工作流。

## 监听 OpenStack

OpenStack 系统中有两种内部通信模式。第一种是对 REST API 进行 GET 或 POST，第二种是向消息队列拉取和推送消息。每个服务都同时使用这两种模式；当 Nova API 通过 REST API 收到供应资源的请求时，它会通过消息总线将该请求传递给计算节点。它们通过总线发送回使用数据，但使用 API 与其他服务通信。这两种通信模式也可用于集成。正如我们可以使用 REST API 供应资源一样，我们可以使用消息总线跟踪它们的进度。

以下代码示例使用 Python 中的 Kombu 库连接到消息总线上的 Nova 队列，以监听各种 Nova 服务之间的这些 RPC 调用。首先，我们定义一个回调方法来处理总线上的事件。这个简单的方法只是打印消息正文：

```
from kombu import Connection, Exchange, Queue

def process_message(body, message):
    print body
    message.ack()
```

然后我们创建一个名为 listener 的队列，并将其绑定到 nova 交换机。在以下示例中，我们将订阅 nova 队列上的所有消息：

```
nova_exchange = Exchange('nova', 'topic', durable=False)
nova_queue   = Queue('listener', exchange = nova_exchange,
routing_key='#')
conn = Connection('amqp://guest:guest@192.168.0.10//')
consumer = conn.Consumer(nova_queue, callbacks=[process_message])
consumer.consume()
```

请注意，连接字符串特定于所讨论的部署。消息总线的用户名和密码应与 /etc/nova/nova.conf 中的设置保持一致。

最后，我们将检查队列中的消息并处理它们：

```
while True:
    conn.drain_events()
```

运行此示例将显示出 Nova 服务之间的通信有多么频繁。这些 RPC 消息的消息格式定义良好，但消息内容与版本相关。出于这些原因，大多数开发者会选择改用通知子系统。

## 使用通知子系统

每个 OpenStack 服务都可以配置为在消息总线上发出通知。这些通知随后可以由消息客户端或 Ceilometer 服务接收。通知最初由每个服务分别实现，但该功能已合并到 Oslo Messaging 库中，作为公共接口。

## 集成平台

要在 Nova 中启用通知，需要在 `/etc/nova/nova.conf` 中更改以下设置：

| nova.conf 设置 | 示例值 | 描述 |
|---|---|---|
| `instance_usage_audit` | `true` | 开启使用情况审计 |
| `instance_usage_audit_period` | `hour` | 将使用情况审计设置为每小时 |
| `notify_on_state_change` | `vm_and_task_state` | 告知 Nova 在虚拟机状态更改时发送通知 |
| `notification_driver` | `messagingv2` | 告知 Nova 将通知发送到消息总线 |

还有其他可用的通知驱动程序。例如，通知可以发送到 syslog。有关更多信息，请参阅 `http://docs.openstack.org/developer/oslo.messaging/FAQ.html` 上的 Oslo Messaging 常见问题解答。

下面提供了一个可以监听这些通知事件的简单客户端：

```
```python
#!/usr/bin/env python

from kombu import Connection, Exchange, Queue

def process_message(body, message):
    print body
    message.ack()

nova_exchange = Exchange('nova', 'topic', durable=False)
notifications_queue = Queue('notification-listener', exchange = nova_exchange, routing_key='notifications.info')
conn = Connection('amqp://guest:guest@192.168.0.10/')
consumer = conn.Consumer(notifications_queue, callbacks=[process_message])
consumer.consume()

while True:
    conn.drain_events()
```
```

此示例创建了一个名为 `notification-listener` 的队列，该队列绑定到 `nova` 交换机。不过，我们不监听交换机上的所有事件，而是订阅 `notifications.info` 主题。这确保我们只接收 Nova 服务指定为通知的消息。



在oslo.messaging库中可以找到一种更高级的抽象，供与OpenStack交互的Python程序员使用。有关此方法的文档可在 http://docs.openstack.org/developer/oslo.messaging/notification_listener.html 获取。另一个选择是使用Yagi库（https://github.com/rackerlabs/yagi），而Rackspace的StackTach（https://github.com/openstack/stacktach）又使用了该库。

## 从Ceilometer消费事件

从消息队列消费事件允许在OpenStack环境中实现推送式集成模型。然而，对于许多组织来说，编写和维护一个守护进程来从队列消费事件并对其执行操作可能非常繁重。一个更简单的选择是通过REST API轮询Ceilometer接口以获取事件。鉴于Nova（以及其他服务）被配置为通过消息总线发出通知，Ceilometer服务可以存储这些事件并使其可供轮询。要启用此功能，请在/etc/ceilometer/ceilometer.conf中将store_events设置为True。

以下示例展示了如何使用Python requests库从Ceilometer检索事件：

1. 首先，我们从Keystone获取一个令牌：

```python
import requests # http://docs.python-requests.org/en/latest
import json

KEYSTONE_URL = 'http://controller01:5000/v2.0/'
TENANT       = 'demo'
USERNAME     = 'demo'
PASSWORD     = 'secret'

r = requests.post("%s/tokens" % (KEYSTONE_URL,), json={
    "auth": {
        "tenantName": TENANT,
        "passwordCredentials": {
            "username": USERNAME,
            "password": PASSWORD,
        }
    }
})
if r.ok:
    token = r.json()['access']['token']['id']
else:
    raise Exception(r.text)

# Place the token in the X-Auth-Token header
headers = {'X-Auth-Token': token}
```

2. 然后，我们在Keystone的服务目录中找到计量服务：

```python
# Find the link to the Ceilometer API in the service catalog:
for service in r.json()['access']['serviceCatalog']:
    if service['type'] == 'metering':
        metering_endpoint = service['endpoints'][0]['publicURL']
```

3. 最后，我们使用GET请求获取所有可用事件，并打印出类型：

```python
events = requests.get("%s/v2/events" % (metering_endpoint,), headers=headers)

for e in events.json():
    print e['event_type']
```

GET请求可以包含针对特定事件类型的过滤器，并且可以包含对检索事件数量的限制。有关使用Ceilometer事件API的更多信息，请参阅 http://docs.openstack.org/developer/ceilometer/webapi/v2.html。

## 在Ceilometer中读取计量数据

在前面的章节中，我们介绍了从OpenStack云检索事件数据的不同方法。然后，这些事件数据可用于跟踪使用情况并为系统的最终用户生成计费信息。Ceilometer服务正是为此而设计的；基于事件数据跟踪使用情况并生成计费信息。Ceilometer通过REST API提供这些数据。

以下示例使用Python requests库请求用户租户内运行的实例的时长：

1. 首先，我们从Keystone请求一个令牌：

```python
#!/usr/bin/env python

import requests # http://docs.python-requests.org/en/latest
import json

KEYSTONE_URL = 'http://controller01:5000/v2.0/'
TENANT       = 'demo'
USERNAME     = 'demo'
PASSWORD     = 'secret'

r = requests.post("%s/tokens" % (KEYSTONE_URL,), json={
    "auth": {
        "tenantName": TENANT,
        "passwordCredentials": {
            "username": USERNAME,
            "password": PASSWORD,
        }
    }
})

if r.ok:
    token = r.json()['access']['token']['id']
else:
    raise Exception(r.text)

# Place the token in the X-Auth-Token header
headers = {'X-Auth-Token': token}
```

2. 然后，我们从服务目录中获取Ceilometer服务的链接：

```python
# Find the link to the Ceilometer API in the service catalog:
for service in r.json()['access']['serviceCatalog']:
    if service['type'] == 'metering':
        metering_endpoint = service['endpoints'][0]['publicURL']
```

3. 接下来，我们针对Meter实例发起一个GET请求以获取统计信息。Ceilometer附带一组预定义的计量器，用于实例运行时间、CPU使用率、磁盘使用率等：

```python
tenant_id = r.json()['access']['token']['tenant']['id']
filter = {'q.field': 'project', 'q.value': tenant_id}
statistics = requests.get("%s/v2/meters/instance/statistics" % (metering_endpoint,), headers=headers)
```

有关可用计量器的完整列表，请参阅 http://docs.openstack.org/admin-guide-cloud/telemetry-measurements.html。

![](img/2f752555f69992669defa8b25e6dcee9_158_0.png)

请注意，我们使用搜索过滤器将统计请求的范围限定为租户自身。为每个租户生成账单的调用将遍历租户列表并收集每个租户的使用情况。还可以通过附加过滤器将时间范围限定在某个时间段内。有关过滤器的更多信息，请参阅 http://docs.openstack.org/developer/ceilometer/webapi/v2.html。

调用返回的JSON包含统计信息列表。我们关注duration值：

```python
print statistics.json()[0]['duration']
```

其他统计信息也可用于计量器，例如总和、平均值、最小值和最大值。

## 更新设计文档

在本章中，我们确定了使用可用API与OpenStack交互的几种不同方式。现在是重新审视设计文档并填写需求部分的好时机。一个理想的OpenStack云需求集应具有以下特征：

- 每个需求都应该是原子的，以限制需求的范围
- 每个需求都应包含上下文；应包含有关谁将使用该功能以及在什么情况下使用的信息
- 每个需求都应该是可测试的，理想情况下是自动化的

如果满足这些条件，我们就可以生成策略文档，规定哪些角色应有权访问哪些功能，并且我们可以生成自动化测试套件来验证平台的需求是否得到满足。这些测试套件又可用于监控套件，以确保运行中的系统按照规范执行。

## 编写需求

在本节中，我们将看一些示例需求。正如我们本章前面看到的，与OpenStack REST API的每次交互都以向Keystone请求令牌开始。以下用户故事很好地代表了这种交互：

> “作为系统的最终用户，我应该能够向Keystone服务提供我的Active Directory用户名和密码，并收到一个令牌，授权我使用其他OpenStack服务。该令牌将限定在我被OpenStack管理员授予访问权限的项目范围内。”

可以将以下元数据与此需求关联起来：

- **用户**：最终用户
- **输入**：Active Directory认证凭据
- **输出**：授权令牌和项目范围

然后，我们还可以使用REST API描述该需求的实现：

```
POST /v2.0/tokens
{
  "auth": {
    "tenantName": "demo",
    "passwordCredentials": {
      "username": "demo",
      "password": "secret"
    }
  }
}

RESPONSE 200
{
  "access": {
    "token": {
      "issued_at": "2014-01-30T15:30:58.819584",
      "expires": "2014-01-31T15:30:58Z",
      "id": "aaaa-bbbbb-ccccc-dddd",
      "tenant": {
        "description": null,
        "enabled": true,
        "id": "fc394f2ab2df4114bde39905f800dc57",
        "name": "demo"
      }
    }
  }
}
```

如果需要，我们还可以为此需求确定服务水平协议。例如，我们可以指定Keystone服务应在10秒内响应并返回有效的令牌。这可用于为系统建立可接受的性能阈值。




让我们通过一个更复杂的示例来说明，即启动实例：

> “作为系统的终端用户，我应该能够出示我的 Keystone 令牌，并通过 Nova API 将给定的镜像实例化为虚拟机。我应该能够指定镜像、规格、SSH 密钥、私有网络以及实例的元数据。”

以下元数据可以与此需求关联：

- **用户：** 终端用户
- **输入：** Keystone 令牌、计算服务 API、租户 ID、镜像 ID、规格 ID、SSH 密钥名称、网络 ID 以及元数据键和值
- **输出：** 实例描述

此需求的一个示例如下：

```
POST /v2.1/{tenant_id}/servers
{
  "server": {
    "name": "example-server",
    "imageRef": "{image_id}",
    "flavorRef": "{flavor_id}",
    "key_name": "{key_name}"
    "networks": "{\"uuid\": \"{network_id}\"}"
    "metadata": {
      "{metadata_key}": "{metadata_value}"
    }
  }
}
RESPONSE 202
{
  "server": {
    "id": "{server_id}",
    "links": [
      {
        "href": "/v2.1/{tenant_id}/servers/{server_id}",
        "rel": "self"
      },
      {
        "href": "/servers/{server_id}",
        "rel": "bookmark"
      }
    ],
  }
}
```

请注意，我们保持需求相对简单。我们本可以逐步介绍整个调用和响应脚本直到最终调用，但这更适合在测试实现中进行。我们将在下一节介绍如何将需求转换为测试。

## 测试需求

在第 4 章《构建部署流水线》中，我们使用 shell 工具为 OpenStack 部署编写了一些基本测试。在本节中，我们将结合本章前面讨论的代码示例与 Python unittest 库，生成一组可从 Jenkins 运行以验证部署的自动化测试。以下示例使用 Python unittest 和 requests 库，非常简单，但可作为起点：

```
#!/usr/bin/env python

import unittest
import requests #http://docs.python-requests.org/en/latest
import json

KEYSTONE_URL = 'http://controller01:5000/v2.0/'
TENANT       = 'demo'
USERNAME     = 'demo'
PASSWORD     = 'secret'
```

导入必要的库并设置全局变量后，我们将为测试创建一个 TestCase 对象：

```
class TestKeystone(unittest.TestCase):
    """Set of Keystone-specific tests"""
```

任何以 test 开头的函数都将被 unittest 库视为要运行的测试。我们将定义一个函数来执行第一个用户故事中描述的 REST 调用：

```
def test_001_get_token(self):
    """Requirement 001: Authorization Token"""
    r = requests.post("%s/tokens"% (KEYSTONE_URL,), json={
        "auth": {
            "tenantName": TENANT,
            "passwordCredentials": {
                "username": USERNAME,
                "password": PASSWORD,
            }
        }
    }
```

```
})
if r.ok:
    self.assertEqual(r.status_code, 200)
    token = r.json()['access']['token']['id']
    self.assertTrue(token)
else:
    raise Exception(r.text)
```

此测试基于本章前面用于从 Keystone 服务检索令牌的示例。我们添加了两个断言语句来验证服务是否按我们设计的方式响应。第一个断言响应代码为 200，第二个断言 JSON 响应中包含令牌的值。

为了使测试能够从命令行运行，我们在脚本末尾添加了对 `unittest` 库的调用：

```
if __name__ == '__main__':
    unittest.main()
```

如果我们将此脚本检入我们在 *第 4 章《构建部署流水线》* 中创建的代码仓库的测试目录，我们可以从 Jenkins 运行脚本来调用此自动化测试。设计文档中的每个需求都应以这种方式关联一个自动化测试。随着需求被添加到设计文档并在 Puppet 中实现，表示它们的新的测试可以添加到套件中。

## 总结

在本章中，我们研究了 OpenStack 平台内的三种集成模式。第一种模式是使用 Keystone 提供的内置集成功能与 Active Directory 集成。第二种是使用 REST API 将基础设施配置到 OpenStack 环境中。第三种是监听消息总线以获取通知。在探索了与系统交互的各种方式之后，我们在设计文档中考虑了它们并为部署的系统编写了需求。最后，这些需求被转换为可插入部署流水线的自动化测试。

我们合作的一些组织对将 OpenStack 平台与环境中的其他服务集成所带来的复杂性表示沮丧。对于这些组织而言，Cloud Management Platform（CMP）是一种值得考虑的选择，它可以模糊集成的一些技术细节。诸如 Red Hat 的 CloudForms 或 Dell 的 Cloud Manager 之类的 CMP 将接入 OpenStack 实现的 REST API 和消息总线，并允许对虚拟基础设施中的对象进行更高级别的访问。它们还提供与第三方身份验证、服务目录和计费系统的即用型集成。

我们喜欢将整个 OpenStack 系统视为一个庞大的集成平台。在 OpenStack 之前，组织花费了大量资源将存储、网络和计算基础设施与其组织基础设施集成。系统的开放设计是其最伟大的优势之一。在下一章中，我们将讨论 OpenStack 安全。我们将检查常见的安全实践，如日志记录、修补和加密，以及一些应用安全建议。

## 参考文献

- Keystone is not an authentication service, Nathan Kinder: https://blog-nkinder.rhcloud.com/?p=130
- How to Integrate Keystone with Active Directory: https://wiki.openstack.org/wiki/HowtoIntegrateKeystonewithAD
- Customizing Horizon: http://docs.openstack.org/developer/horizon/topics/customizing.html
- OpenStack API Documentation: http://developer.openstack.org/api-guide/quick-start/index.html

![](img/2f752555f69992669defa8b25e6dcee9_166_0.png)

## OpenStack 中的安全区域

在 OpenStack 部署中存在一系列逻辑安全区域。这些是 OpenStack 平台中可被应用程序、服务器、网络或用户利用的基本信任区域。这些区域的信任级别不断提高，可以分解为以下区域：

1. 公共
2. 访客
3. 管理
4. 数据

- **公共区域：** OpenStack 中的这些区域是任何云基础设施中完全不受信任的区域。按惯例，它们是最开放的，因此被称为公共。它们不一定对互联网开放，但该区域对不受信任的资源开放，并且在网络上不受运营商直接控制。该区域需要加密和其他补偿控制措施才能满足大多数组织的安全要求。
- **访客区域：** 这些区域用于在 OpenStack 云中配置的实例。它们包括跨租户网络实例流量（一个实例到另一个实例，跨隔离网络或同一租户网络）。此流量不是支持云基础设施的流量，也不是提供对 OpenStack 原生 RESTful API 端点的 API 访问的流量。将不受监管的公共 IP 地址提供给驻留在该域中的实例将导致该域不受信任。只有在实施补偿控制措施以使用外部网络控制来监管对该区域的公共 IP 访问时，该区域才可被视为受信任。





- **管理区域**：这些是 OpenStack 服务所在并交互的区域。这些特定区域通常被隔离，仅用于 OpenStack 控制平面流量。由于这种内部通信的敏感性，必须采取措施强化该特定网络，并且只有运行 OpenStack 控制平面的服务器才能访问该网络。一旦完成，该区域即被视为可信区域。如果该区域被路由或连接到可路由网络，实际上就相当于将其桥接到不可信区域，该网络会立即变成不可信网络，并且必须立即实施补偿性控制措施。
- **数据区域**：这些安全区域主要包含与 OpenStack 内部存储服务相关的服务和网络流量。这些数据通常需要高安全性和高保密性。该网络区域的信任级别高度依赖于云中运行的工作负载以及所执行的部署类型。因此，该区域可以是可信的，也可以是不可信的；如果不了解部署类型的具体细节，我们无法指定默认信任级别。在考虑 OpenStack 安全以及平台上运行的资源和服务时，理解这些区域非常重要。开发者有意将平台不同部分的访问权限分开，以区分特权区域和非特权区域，并减少跨区域攻击。

## 软件漏洞

OpenStack 是一个主要用 Python 编写的编排平台，运行在基于 Linux 的操作系统之上。该编排平台负责为租户配置实例或提供基础设施即服务（IaaS），以支持需要在云环境中运行的工作负载。因此，OpenStack 软件漏洞可分为两大类：第一类包括 OpenStack 配置和编排的实例或裸金属服务器；第二类包括 OpenStack 基础设施环境及其主机。

### 实例软件安全与补丁

在 OpenStack 中，虚拟机监视器（hypervisor）创建并运行独立的虚拟机或实例。这些实例需要与其所在的底层 OpenStack 基础设施分开进行软件更新和打补丁。对虚拟机监视器和底层服务器操作系统的更新不会传播到活动工作负载和实例；因此，必须存在两种策略——一种用于云上运行的实例，另一种用于云基础设施。

实例策略应与当前生效的、用于控制现有遗留系统补丁管理的组织和治理策略保持一致。由于 OpenStack 基于镜像和规格（flavor）启动实例，这些镜像和规格可能在实例启动时将可执行元数据注入实例，因此根据工作负载类型，有多种方法可以确保在启动实例之前使用最新加固镜像。

对于 OpenStack 云中常见的传统临时工作负载，应根据组织策略定期更新 Glance 镜像，并由公司内的可信组织进行控制。这些更新应向操作系统添加补丁和安全修复，并作为现有镜像导入 Glance 镜像仓库。在部署到生产环境之前，这些更新后的镜像应在较低级别的开发环境或沙盒环境中进行测试。然后，这些更新后的镜像将作为任何使用该镜像的新实例以及一直在使用该镜像的临时工作负载的基础操作系统。但是，正在运行的镜像不会改变。安全补丁只有在实例重新启动后才会生效。通常，临时工作负载往往能够在影响应用程序可用性的情况下重新启动，但云上运行的持久工作负载则需要制定基于应用程序的策略，以便在打补丁的同时保持应用程序的正常运行时间。一些组织对运行旧镜像版本的实例重启有严格规定，具体取决于应用于镜像的补丁是用于安全修复还是仅仅是一次更新。

应创建默认的 OpenStack 安全策略，以允许传统软件合规性工具扫描主机的软件更新合规性。对于持久工作负载以及不会频繁重启或需要保持一定可用性水平的实例，可以创建安全策略，允许这些实例使用基于传统软件仓库的补丁解决方案，并通过对正在运行的实例打补丁的传统方式实施补丁。然而，根据组织的 OpenStack 网络部署情况，可能需要进行额外的网络配置，才能使软件仓库在租户网络内可用。对于临时和持久工作负载，都可以使用 cloud-init 和 cloudbase-init，在启动时利用原生软件管理工具和本地软件仓库对镜像执行更新。但是，根据对镜像执行更新的数量，这些在启动时发生的更新可能会延迟实例配置。这就是为什么更新基础 Glance 镜像是确保实例安全和软件更新合规性的最佳实践。

### 基础设施主机安全与补丁

安全是 OpenStack 架构的基本组成部分，需要持续维护，以保护该技术栈中的各个安全区域。OpenStack 是一个由许多不同部分组成的复杂平台，多个不同的开发方正在积极持续地开发它。表面上，这似乎从根本上缺乏安全性，但实际上，OpenStack 不仅由成千上万的个人开发，还受到成千上万的用户和开发者的测试和审查。这些用户和开发者向其他开发者和测试者形成了一个有用的反馈循环。这几乎提供了对草率和不安代码的持续警惕。然而，与商业软件一样，安全问题仍时有发现。这就是为什么 OpenStack 基金会成立了 OpenStack 安全团队，发布有关已识别安全问题的公告、描述和补丁链接。

#### 为 OpenStack 代码打补丁

这些补丁针对 OpenStack 代码中的漏洞。当前的安全公告列表维护在 https://security.openstack.org/ossalist.html，其中也提供了补丁。这些补丁大多是升级后的 Python 文件，可以直接替换 OpenStack 控制节点或计算节点上的现有文件。一个针对在 Neutron 中发现的 bug、包含替换文件的补丁示例可在 https://review.openstack.org/#/c/299025/ 查看。OpenStack 漏洞管理团队负责安全和 OpenStack 的 bug 及修复流程，相关信息参见 https://wiki.openstack.org/wiki/Vulnerability_Management。

#### Red Hat Enterprise Linux 和 CentOS

使用任何 Linux 发行版时，及时更新所有受影响的软件以限制安全风险至关重要。如果该软件是 Red Hat 内某个包的一部分，则应在 Red Hat 发布修复后尽快修补该漏洞。Red Hat 通常会随其安全公告附带补丁，然后以安全勘误更新（security erratum update）的形式发布。同样，必须在补丁和更新发布后立即应用，以帮助消除攻击者利用该 bug 攻击您的 OpenStack 基础设施的风险。这可以通过 Yum 包管理器实现，并且仅从可信来源下载。这些可信来源可以是 Red Hat 网络（RHN）或由您的组织控制的本地仓库。从组织外部下载的所有包都应使用 GNU Privacy Guard 或 GNUPG（一种用于确保包文件真实性的免费软件）进行真实性验证。如果验证失败，则说明包已损坏或被篡改。

#### Canonical 基于 Ubuntu 的操作系统

Ubuntu 的安全和补丁生态系统与 Red Hat 非常相似。当 Canonical 的 Ubuntu 开发者发现并修补漏洞时，他们就会发布安全更新。安全通知可以通过电子邮件或 RSS 订阅获取。Ubuntu 允许自动安装安全更新——一旦配置完成，您无需再手动运行安全更新。Ubuntu 允许用户通过 `cron-apt` 使用 `unattended-upgrades` 配置自动安全更新。类似的行为也可应用于 Red Hat 主机，方法是使用 `yum-cron` 并设置 `update_cmd = minimal-security-severity:Important`。

#### 软件仓库管理

软件仓库管理对于为 OpenStack 基础设施提供安全更新至关重要。Red Hat 开发了一个名为 **Satellite** 的管理应用程序，Ubuntu 则有 **Landscape**。这两者都是系统管理应用程序，允许管理员部署、修补、管理和监控各自的系统。这些应用程序是一种本地部署（on-premise）方式，用于从远程分配门户下载和管理内容。它们减少了系统更新期间公司广域网（WAN）上的流量，因为所有包只需传输到本地仓库一次。这些应用程序还可在本地管理更新文件，以限制外部恶意内容带来的安全风险。




Satellite 甚至可以配置为单一系统，或配置为一系列远程系统，这些系统充当代理，并从中心服务器获取授权和订阅信息。这些商业应用还为用户提供包管理的单一管理界面，并允许为企业中不同类别的服务器创建更新配置文件。

另一种选择是使用 Puppet、Chef 和 Ansible 等开源工具，通过创建脚本执行复制和验证，手动创建和维护包仓库。不过，更完整的方案是使用 Pulp（http://www.pulpproject.org/）等社区项目。Pulp 是一个用于管理软件仓库的免费开源平台。它可配置支持 RPM 包类型（rpm、srpm、errata 等）、Puppet 模块、Docker 镜像、Atomic Trees、Python 包等。还可以通过插件支持 Debian 包。使用 Pulp 可以完成以下操作：

- 手动将发行版仓库中的内容拉取到 Pulp 服务器，可以是仅一次或定期进行
- 将你自己的内容上传到 Pulp 服务器（OpenStack 安全补丁）
- 将内容发布为基于 Web 的仓库、一系列 ISO 或其他各种方式

随着需要修补的主机数量增加，软件修补和仓库管理会变得越来越困难。在启动 OpenStack 部署时，制定正确的修补策略并借助工具来管理和启用修补，对于扩展 OpenStack 云至关重要。通过先在较低环境、再在生产环境中对 OpenStack 基础设施应用和测试安全补丁，组织将确保开发和生产环境都是安全的，并已通过运维测试。

## 加固 Hypervisor

Nova 服务是 OpenStack 最复杂的项目之一，在环境中提供计算功能。Nova 在 OpenStack 云中无处不在，并与大多数其他核心 IaaS 服务交互。正确配置这一特定服务是保障 OpenStack 部署安全的重要因素。

## 标准 Linux 加固实践与 Hypervisor

OpenStack 环境安全的关键在于虚拟化技术（也称为 hypervisor）的配置和加固。虽然 OpenStack 可以配置使用多种不同的 hypervisor，但目前最常用的 hypervisor 是 KVM。RHEL、Ubuntu 和 CentOS 等所有主流操作系统都支持 KVM hypervisor。Red Hat OpenStack Platform、Mirantis OpenStack 和 HP Helion 等所有主流 OpenStack 发行版都默认使用 KVM 作为 hypervisor。因此，我们将重点关注在 Linux 上运行、实现生产级安全的 KVM hypervisor。

此外，KVM hypervisor 已在商业发行版上通过美国政府的 Common Criteria 计划认证，并已验证可将各实例的运行时环境彼此隔离，从而提供充分的实例隔离。更多信息请参阅 https://www.niap-ccevs.org/NIAP_Evolution/faqs/nstissp-11/。

Red Hat OpenStack Platform 是已通过 Common Criteria 认证的发行版之一，它必须满足以下最低身份验证要求列表：

- 审计（能够详细审计大量事件）
- 自主访问控制
- 强制访问控制
- 基于角色的访问控制
- 对象重用（文件系统对象、内存和 IPC 对象）
- 管理用户的安全管理
- 安全通信（SSH）
- 存储加密（支持加密块设备）
- TSF 保护（通过硬件保护内核软件和数据）

## 保护云安全

虽然这些身份和认证功能是美国政府软件认证所必需的，但它们对于生产云环境同样至关重要，尤其是那些在公共区域运行并连接互联网的环境。这些工具的输出应输入到第 5 章 *构建可运维* 中讨论的更大日志、监控和告警框架中。在使用 ELK 等日志处理与审计工具时，一旦发生任何安全事件或超过阈值，即可生成安全告警。

除了身份和认证之外，OpenStack 还可以使用多种不同的密码标准来实现身份识别、认证、数据传输加密以及静态数据保护。其中一些如下：

- AES：传输中数据和静态数据
- TDES：传输中数据
- RSA：传输中数据以及身份识别/认证
- DSA：传输中数据以及身份识别/认证
- Serpent：静态数据
- Twofish：静态数据
- SHA-1：传输中数据和静态数据
- SHA-2：静态数据以及身份识别/授权

为了实现最佳安全性，不仅底层操作系统必须提供相应功能，运行计算主机的硬件（也因此运行 hypervisor）也应支持以下特性：

| 特性 | 硬件描述 | 安全增强 |
|---|---|---|
| VT-d / AMD-Vi | I/O MMU | PCI 直通保护 |
| Intel TXT / SEM | 英特尔可信执行技术 | 动态证明服务 |
| SR-IOV、MR-IOV、ATS | PCI-SIG I/O 虚拟化 | 多个实例安全共享 PCI Express 设备。 |
| VT-C | 网络虚拟化 | 改进 hypervisor 上的网络 I/O |

除了硬件和操作系统要求之外，KVM 还支持其他功能，如 sVirt（SELinux 和虚拟化）、Intel TXT 和 AppArmor。

## SELinux 与 sVirt 实战

以下步骤将帮助你检查 SELinux 是否处于 enforcing（强制启用并保护）状态。由于你的实验室运行的是 CentOS 7，你应该已经安装了 SELinux 并处于 enforcing 状态，同时所有已安装软件也应支持 sVirt 正常工作。我们将向你展示如何设置镜像文件权限以保护你的实例：

- 1. 在实验室服务器上以 root 身份执行以下命令：

通常，在 HA 环境中，你会在计算节点上执行此命令。

```
# getenforce
Enforcing
```

确认命令返回 Enforcing。

- 2. 如果由于某种原因输出其他内容，请执行以下操作并重复上一条命令：

```
# setenforce 1
```

- 3. 现在，检查你的 id 和上下文：

```
# id
uid=0(root)  gid=0(root)  groups=0(root)
context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

- 4. 现在，查找实验室服务器上运行实例的上下文：

你应该至少有一个实例在运行。如果不止一个，请选择第一个。如果没有实例在运行，请创建一个实例并重复该命令。

```
# ps axZ | grep qemu-kvm

system_u:system_r:svirt_tcg_t:s0:c55,c122 7480  ?
Rl  0:10 /usr/libexec/qemu-kvm -name instance-
00000001
-S -machine pc-i440fx-rhel7.0.0,accel=t cg,usb=off
-m 512 -realtime mlock=off -smp
1,sockets=1,cores=1,threads=1 -uuid b803b3ce-1570-
```

```
4092=8765=4a9f76793842 -smbios\ntype=1,manufacturer=Fedora\nProject,product=OpenStack Nova,version=13.1.0-
1.el7,serial=8184f926-0263-4305-8d8d-
d50c1472697e,uuid=b803b3ce-1570-4092-8765-
4a9f76793842,family=Virtual Machine -no-user-
config -nodefaults -chardev\nsocket,id=charmonitor,path=/var/lib/libvirt/\nqemu/domain-instance-00000001/monitor.sock,\nserver,nowait -mon\nchardev=charmonitor,id=monitor,mode=control -rtc\nbase=utc -no-shutdown -boot strict=on -device\npiix3-usb-uhci,id=usb,bus=pci.0,addr=0x1,0x2 -\ndrive file=/var/lib/nova/instances/b803b3ce-1570-\n4092-8765-4a9f76793842/disk,if=none,id=drive-\nvirtio-disk0,format=qcow2,cache=none -device\nvirtio-blk-pci,scsi=off,bus=pci.0,addr=0x4,\ndrive=drive-virtio-disk0,id=virtio-disk0,\nbootindex=1 -netdev tap,fd=26,id=hostnet0 -device\nvirtio-net-pci,netdev=hostnet0,id=net0,\nmac=fa:16:3e:43:48:ad,bus=pci.0,addr=0x3 -chardev\nfile,id=charserial0,path=/var/lib/\nnova/instances/b803b3ce-1570-4092-8765-\n4a9f76793842/console.log -device isa-\nserial,chardev=charserial0,id=serial0 -chardev\npty,id=charserial1 -device isa-\nserial,chardev=charserial1,\nid=serial1 -device usb-tablet,id=input0 -vnc\n0.0.0.0:0 -k en-us -vga cirrus -device\nvirtio-balloon-pci,id=balloon0,bus=pci.0,addr=0x5\n-msg timestamp=on
```

如你所见，第一行给出了 qemu-kvm（实例进程）的上下文。`instance-00000001` 实例的 MCS 上下文为 `system_u:system_r:svirt_tcg_t:s0:c55,c122`。

- 5. 现在，查看 `/var/lib/nova/instances/b803b3ce-1570-4092-8765-4a9f76793842/` 中与该实例关联的文件，可以确认 sVirt 已正确地将它们标记为与实例进程相同的上下文（`s0:c55,c122`）：

```
# ls -alZ /var/lib/nova/instances/b803b3ce-1570-\n4092-8765-4a9f76793842/\n\ndrwxr-xr-x. nova nova\nsystem_u:object_r:nova_var_lib_t:s0 .
```



在本章开头，我们定义了安全区域。这些网络和资源区域，配合适当的物理或逻辑网络架构，为OpenStack云的不同区域提供了必要的网络和资源隔离。例如，可以通过物理LAN或VLAN分离来隔离区域。

## 评估风险

考虑到这一最佳实践，管理员首先需要确定其组织中的威胁向量在哪里，以及哪些地方最易受攻击。一个例子是那些将Horizon等公开安全区域服务暴露于互联网的云。由于这些端点暴露在组织无法控制的网络和用户面前，该端点显然是采取高安全性措施的优先选择。然而，保护公共端点可能并不是OpenStack端点所需的唯一加密。根据组织的网络架构，威胁可能来自内部网络，这些网络可能连接到更安全的OpenStack区域（管理区域或私有区域）。因此，在制定加密安全策略时，仅仅依赖OpenStack的默认安全区域隔离是不够的。虽然在所有位置添加加密无疑会为所有流量增加额外保护，但如果端点主机被攻破，仅添加SSL/TLS加密将无法提供保护。因此，端点加密应只是更大范围的网络和基于主机的安全策略的一部分。

## 端点安全最佳实践

在当今标准的OpenStack部署中，公共网络上的默认流量通过SSL进行保护。虽然这是一个良好的第一步，但最佳安全实践要求所有内部流量都应采用同样安全的实践。此外，由于最近的SSL漏洞，除非组织绝对需要遗留兼容性，否则SSL/TLS是唯一推荐的加密方法。

TLS通过公钥基础设施（PKI）框架保护端点。该框架是一组流程，通过验证相关方的身份来确保消息安全发送。PKI涉及私钥和公钥，它们协同工作以创建安全连接。这些密钥由证书颁发机构（CA）认证，大多数公司为自己的内部证书签发CA。面向公众的端点必须使用知名的公共证书颁发机构，以便大多数浏览器将连接识别为安全。因此，由于这些证书存储在单个主机上，主机安全对于保护私钥文件至关重要。如果私钥被泄露，那么使用该密钥的任何流量的安全性也会受到损害。

在OpenStack生态系统中，目前通过使用OpenSSL实现的库提供对TLS的支持。我们建议仅使用TLS 1.2，因为1.1和1.0容易受到攻击。由于许多不同的公开漏洞（Heartbleed、POODLE等），应完全避免使用SSL（v1-v3.0）。如今大多数实现实际上是TLS，因为TLS是SSL的演进版本。许多人将TLS 1.0称为SSL 3.1，但常见的做法是将最新的HTTPS实现称为SSL/TLS或TLS/SSL。在本文档中，我们将使用前一种说法（即SSL/TLS）。

建议所有区域中的所有API端点都配置为使用SSL/TLS。但是，在某些情况下，由于加密所需的处理，性能可能会受到影响。在高流量的使用场景中，加密确实会消耗大量资源。在这些情况下，我们建议使用硬件加速器作为可能的选项，以便将加密工作从主机本身卸载。

对于OpenStack端点使用SSL/TLS的最常见用例是使用可以建立和终止SSL/TLS会话的SSL/TLS代理。虽然有多种选择，例如Pound、Stud、nginx、HAproxy或Apache httpd，但将由管理员决定使用哪个工具。

## 示例

以下示例将展示如何通过Apache wsgi代理配置为Horizon的服务端点安装SSL/TLS，以及在Keystone中配置原生TLS支持。这些示例假设管理员已经从内部CA源获得了证书。如果您将公开发布Horizon或Keystone，则务必从全球CA获取证书。这些证书应能被所有浏览器识别，并且能够通过互联网进行验证。

让我们从Keystone开始：

既然我们已经有了证书，让我们将它们放在正确的位置并赋予适当的权限：

```
# chown keystone /etc/pki/tls/certs/keystone.crt
# chown keystone /etc/pki/tls/private/keystone.key
```

现在我们可以开始进行正式的安全工作了。我们首先为Keystone添加一个新的SSL端点，然后删除旧的端点。首先，找到当前的端点：

```
# keystone endpoint-list | grep 5000
```

现在创建新的端点：

```
# keystone endpoint-create --publicurl https://openstack.example.com:5000/v2.0 --internalurl https://openstack.example.com:5000/v2.0 --adminurl https://openstack.example.com:5000/v2.0 --service keystone
```

然后删除旧的端点：

```
# keystone endpoint-delete <endpoint-id-from-above-endpoint>
```

编辑您的 /etc/keystone/keystone.conf 文件，使其包含以下行：

```
[ssl]
enable = True
certfile = /etc/pki/tls/certs/keystone.crt
keyfile = /etc/pki/tls/private/keystone.key
```

现在重启Keystone服务并确保没有错误：

```
# service openstack-keystone restart
```

更改您的 keystonerc_admin 或任何其他项目环境文件中的环境变量，通过将 OS_AUTH_URL 更改为以下内容来包含新端点：

```
OS_AUTH_URL=https://openstack.example.com:35357/v2.0/
# source keystonerc_<user>
export OS_CACERT=/path/to/certificates
```

快速测试一下：

```
keystone endpoint-list
```

## 保护云安全

假设该命令没有显示任何错误，您现在可以配置其余的OpenStack服务，使其使用SSL/TLS连接到Keystone。这些配置都非常相似。以下以Nova为例。您需要编辑 `/etc/nova/nova.conf` 文件，使其看起来像这样：

```ini
[keystone_authtoken]
auth_protocol = https
auth_port = 35357
auth_host = openstack.example.com
auth_uri = https://openstack.example.com:5000/v2.0
cafile = /path/to/ca.crt
```

Cinder、Glance、Neutron、Swift、Ceilometer等也都是如此。以下是需要编辑的配置文件列表：

- `/etc/openstack-dashboard/local_settings` （Horizon 访问 Keystone）
- `/etc/ceilometer/ceilometer.conf`
- `/etc/glance/glance-api.conf`
- `/etc/neutron/neutron.conf`
- `/etc/neutron/api-paste.ini`
- `/etc/neutron/metadata_agent.ini`
- `/etc/glance/glance-registry.conf`
- `/etc/cinder/cinder.conf`
- `/etc/cinder/api-paste.ini`
- `/etc/swift/proxy-server.conf`

一旦这些或其他任何服务配置文件被修改，请重启所有服务并测试您的云环境：

```bash
# openstack-service restart
# service httpd restart
# openstack-status
```

您应该看到所有服务都在运行并且在线。现在您已经加密了OpenStack服务与Keystone之间的流量，这是保持云安全的一个非常重要的步骤。如果没有，请检查服务的日志文件以查找错误。

现在，我们配置Horizon，以使用SSL/TLS加密与客户端的连接。

在控制器节点上，在Ubuntu的 `/etc/apache2/apache2.conf` 和RHEL的 `/etc/httpd/conf/httpd.conf` 中添加以下内容（或修改现有配置以匹配）：

```
<VirtualHost <ip address>:80>
    ServerName <site FQDN>
    RedirectPermanent / https://<site FQDN>/
</VirtualHost>
<VirtualHost <ip address>:443>
    ServerName <site FQDN>
    SSLEngine On
    SSLProtocol +TLSv1 +TLSv1.1 +TLSv1.2
    SSLCipherSuite HIGH:!RC4:!MD5:!aNULL:!eNULL:!EXP:!LOW:!MEDIUM
    SSLCertificateFile      /path/<site FQDN>.crt
    SSLCACertificateFile    /path/<site FQDN>.crt
    SSLCertificateKeyFile /path/<site FQDN>.key
    WSGIScriptAlias / <WSGI script location>
    WSGIDaemonProcess horizon user=<user> group=<group> processes=3 threads=10
    Alias /static <static files location>
    <Directory <WSGI dir>>
        # For http server 2.2 and earlier:
        Order allow,deny
        Allow from all

        # Or, in Apache http server 2.4 and later:
        # Require all granted
    </Directory>
</VirtualHost>
```

在计算节点上，`libvirt` 守护进程的默认配置是不允许远程访问的。但是，在OpenStack计算节点之间进行实例的在线迁移需要计算节点之间的远程 `libvirt` 守护进程访问。显然，在大多数情况下，未经身份验证的远程访问是不允许的；因此，我们可以设置带有SSL/TLS加密的 `libvirtd` TCP套接字，并使用X.509客户端证书进行身份验证。





## 审计 OpenStack

随着越来越多的企业将生产工作负载迁移到 OpenStack，对审计合规的需求呈指数级增长。这些企业可能对 PCI、FedRAMP、SOX 和 HIPAA 合规有专门的审计要求，如果没有一种方式来审计其云中发生的情况，他们将不符合合规要求。这些企业习惯于在传统平台上拥有这种能力，因此，要求 OpenStack 也提供这种能力是合乎逻辑的。

这种审计需要以企业习惯的方式进行，不应期望他们必须以分散的方式从每个核心 OpenStack 项目中摄取不同的日志格式，正如他们今天有时必须做的那样。然而，好消息是，有一种格式可以提供规范化和联邦化的方式来收集和分析审计数据。这种格式是 **Cloud Auditing Data Federation (CADF)** 标准。CADF 标准定义了一个完整的事件模型，任何人都可以用来填写在云环境中认证、自主管理和自主审计应用安全所需的必要数据。这种功能对于建立对云平台的信任并增加采用率是必要的。企业需要感到能够跟踪和检测其云平台上的未经授权访问。

### CADF 详情

根据 CADF 的管理机构 Distributed Management Task Force，CADF 标准执行以下功能：

> “定义了一个规范的事件数据模型以及一组兼容的接口，用于在云提供商和客户之间联合事件、日志和报告。CADF 为云服务的客户提供了多项好处。审计事件数据可以用通用格式表示，以便在不同的云提供商之间一致地报告此数据。云客户还将能够从不同的云提供商聚合数据，以提供所有审计数据的更完整和一致的视图。此外，由于来自不同提供商的审计数据以相同格式提供，客户将能够对所有审计数据使用通用的审计工具和流程。能够从不同来源联合数据也将为 OpenStack 用户带来好处，提供一种审计数据格式，该格式将在由一些通用组件（如 Keystone 和 Oslo 库）组成的分散云（IaaS）服务集合中保持一致。这些组件需要共享审计数据。”

CADF 规范包含一个事件模型，该模型为任何可审计的事件指定了分类法，具体如下：

- **Resource:** 通过与事件相关的逻辑资源对事件进行分类。基本上，这是谁是预期目标或什么观察到事件。
- **Action:** 用于对导致事件的操作进行分类。
- **Outcome:** 描述操作的结果。

此外，前述分类法的强制属性进一步将事件分解为以下五个组件：

| Model Component | CADF Definition |
|---|---|
| **OBSERVER** | 基于其对 Actual Event 的观察（直接或间接）生成 CADF 事件记录的 RESOURCE。 |
| **INITIATOR** | 根据 OBSERVER，发起、起源或引发事件 ACTION 的 RESOURCE。 |
| **ACTION** | 根据 OBSERVER，INITIATOR 对事件的 TARGET 执行、尝试执行或待执行的运算或活动。 |
| **TARGET** | 根据 OBSERVER，CADF 事件记录的 ACTION 被执行、尝试执行或待执行的 RESOURCE。*注意：TARGET（在 CADF 事件模型中）可以代表多个目标资源。* |
| **OUTCOME** | 根据 OBSERVER，ACTION 对 TARGET 的结果或状态。 |

使用这些属性，CADF 数据模型旨在提供活动的“谁”、“什么”、“何时”、“何处”、“从何处”和“到何处”。安全领域的大多数人都听说过审计和合规的 5 个 W，然而，该模型是为云环境设计的，并额外增加了两个（从何处和到何处）。

为了进一步解释 CADF 如何基于 7 个 W 用于审计，请参考以下图表：

| "W" Component | CADF Mandatory Properties | CADF Optional Properties (where applicable) | Description |
|---|---|---|---|
| What | event.action<br>event.outcome<br>event.type | event.reason (e.g., severity, reason code, policy id) | 发生了“什么”活动；“什么”是结果 |
| When | event.eventTime | reporter.timestamp (detailed), for each reporter<br>event.duration | “何时”发生<br>• 任何粒度均可通过 ISO 8601 格式 |
| Who | initiator.id<br>initiator.type | initiator.id (id, name): (basic)<br>initiator.credential (token): (detailed)<br>initiator.credential.assertions (precise) | “谁”（人员或服务）发起操作 |
| FromWhere |  | initiator.addresses (basic)<br>initiator.host (agents, platforms, ...) (detailed)<br>initiator.geolocation (precise) | FromWhere 提供描述操作发起地的信息。<br><br>可能包括<br>• 逻辑/物理地址<br>• ISO-6709-2008，精确地理位置 |
| OnWhat | target.id<br>target.type |  | 活动针对的“onWhat”资源 |
| Where | observer.id<br>observer.type | reporterstep.role (detailed)<br>reporterstep.reporterTime (detailed) | “在何处”活动被观察到（报告），或以某种方式被修改。 |
| ToWhere |  | target.addresses (basic)<br>target.host (agents, platforms, ...) (detailed)<br>target.geolocation (precise) | ToWhere 提供描述受操作影响的目标资源所在位置的信息。<br><br>例如，这可以简单到一个 IP 地址或服务器名称。 |

CADF – the 7 W's of audit – CADF OpenStack

### 使用 CADF 与 OpenStack

除了少数特殊情况外，从 OpenStack 服务获取 CADF 信息相当直接。然而，根据发行版，这些说明可能会有所不同。
以下说明展示了如何为 Nova 启用 CADF 审计事件以发送到 Ceilometer（可选日志文件）。这是通过 Keystone 中间件完成的，该中间件提供了一个可选的 WSGI 中间件过滤器，允许对 OpenStack 的每个组件的 API 请求进行审计。

首先，登录到您的 OpenStack 部署。编辑 `/etc/nova/api-paste.ini` 文件。在文件末尾添加以下内容：

```
[filter:audit]
paste.filter_factory = pycadf.middleware.audit:AuditMiddleware.factory
audit_map_file = /etc/nova/api_audit_map.conf
```

检查 `[composite:openstack_compute_api_v2]` 设置并验证值是否与以下示例匹配：

```
[composite:openstack_compute_api_v2]
use = call:nova.api.auth:pipeline_factory
noauth = faultwrap sizelimit noauth ratelimit osapi_compute_app_v2
keystone = faultwrap sizelimit authtoken keystonecontext ratelimit audit osapi_compute_app_v2
keystone_nolimit = faultwrap sizelimit authtoken keystonecontext audit osapi_compute_app_v2
```

还提供其他选项，可将事件重定向到日志文件以及消息传递。只需添加以下内容：

```
[audit_middleware_notifications]
driver = log
```

现在，将 `api_audit_map.conf` 文件复制到 `/etc/nova/` 目录并重启 API 服务。

重启 API 服务的命令因操作系统而异。在 Red Hat Enterprise Linux 系统上，命令为 `service openstack-nova-api restart`。要了解更多信息，请参阅 https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/2/html/Getting_Started_Guide/chap-Deploying_Compute_Services.html

打开 OpenStack 安装目录的 `egg-info` 子目录中的 `entry_points.txt` 文件。

![](img/2f752555f69992669defa8b25e6dcee9_190_0.png)

对于 PackStack 安装，文件路径类似于 `/usr/lib/python2.7/site-packages/ceilometer-2014.2-py2.7.egg-info/entry_points.txt`。

虽然此示例仅针对 Nova，但其他服务（如 Glance、Ceilometer、Cinder、Neutron 等）的预配置 `api_audit_map.conf` 文件可在 https://github.com/openstack/pycadf/tree/master/etc/pycadf 获取。通过遵循上述步骤并使用这些 `api_audit_map.conf` 文件，您可以为所有核心 OpenStack 服务启用 CADF 事件。

由于事件存储到消息传递中，并且默认情况下事件发送到 Ceilometer，因此您将在此处找到事件存储的位置。为了查询 Ceilometer 中的 Keystone 事件，您应首先执行会导致事件的 Keystone 操作，例如：

```
$ openstack user create test_user --os-identity-api-version 3 --os-auth-url http://10.0.2.15:5000/v3 --os-default-domain default
```

为了从 Ceilometer 中获取此操作创建的事件，您应执行以下操作：

```
$ ceilometer event-list --query
event_type=identity.user.created
```

这将导致以下类型的输出：

| Event Type | Trait Name | Trait Type | Trait Value |
|---|---|---|---|
| identity.user.created | initiator_id | string | asd123j123312yf34212339e6e424a7f |
| identity.user.created | project_id | string | ae7e3k14k202042j22f19bc25c8c09f0 |
| identity.user.created | resource_id | string | 22a21766c478927349e3fda7ee0b712b |
| identity.user.created | service | string | identity.openstack.example.com |





正如我们在前面的输出中所看到的，我们拥有由前一个命令创建用户的审计轨迹，以及项目和资源 ID。 这些数据以及来自其他服务的所有数据组合起来，将为该用户在此云中的交易以及此云中的任何其他交易提供 7 W 审计轨迹。 可以使用 Ansible、Puppet 或 Chef 等开源工具为 Ceilometer、日志文件创建查询自动化，或者让 Ceilometer 将此事件数据发送到类似 IBM 的 QRadar http://www-03.ibm.com/software/products/en/qradar 的系统，该系统根据 IBM 的说法：

> “[A] 安全情报平台，它将传统的网络安全信息和事件管理（SIEM）和日志管理功能与网络行为异常检测（NBAD）、漏洞评估和管理、风险分析和模拟以及取证数据检查相结合。 它通过与其他产品/应用程序/服务/资产/端点集成来消耗事件、流、资产和漏洞信息以及网络拓扑（在客户端的环境中本地或在云中），使用户能够从许多不同的角度和视角查看、分析、理解和报告其环境中发生的一切。”

QRadar 作为 Ceilometer 数据的消费者与 OpenStack 集成。 然后它将平台中的 CADF 格式数据用于分析。

### 日志聚合和分析

正如我们在上一节中所看到的，我们拥有由演示命令创建用户的审计轨迹，以及项目和资源 ID。 这些数据以及来自其他服务的所有数据组合起来，将为该用户在此云中的交易以及此云中的任何其他交易提供 7 W 审计轨迹。 然而，由于我们现在拥有审计数据，我们可以开始寻找安全攻击。 其中一些攻击可能如下：

- 对 OpenStack 进行暴力攻击的实时安全警报
- 检测对虚拟机、卷或镜像的恶意或未经授权的访问
- 意外或故意的云服务中断
- 对 OpenStack 中所有用户活动的合规性报告

这些类型的攻击都可以通过日志内省来检测。 正如我们在 *第 5 章，构建以运营* 中讨论的那样，有一套 OpenStack 工具已成为日志传输、格式化和内省的事实标准。 构成这一强大组合的三个工具是 Elasticsearch（搜索模块）、Logstash（日志传输工具）和 Kibana（可视化引擎和仪表板）。 虽然我们在 *第 5 章，构建以运营* 中主要从运营角度讨论了这些工具，但这些相同的工具可用于实时检测前面列出的所有攻击。 该堆栈的主要优势如下：

- 用 Java 编写的开源搜索服务器
- 用于索引多种异构数据
- 通过索引进行实时搜索
- 输出 JSON 的 REST API Web 界面

在此领域的最佳架构实践是使用 Logstash 以分布式方式将所有包含 CADF 数据的日志文件集中到运行 Elasticsearch 和 Kibana 的安全主机上。 然后配置 Logstash 以索引高度规范化的 CADF 日志，并创建限制以在阈值被突破时发出警报。

例如，如果 CADF 日志检测到在短时间内来自同一 IP 地址的数千次 Keystone 身份验证失败事件，那么很可能是暴力攻击，并且值得向安全和网络团队发出警报。

使用 Kibana，团队可以搜索审计日志并确定攻击发生的时间有多长，是否有任何有效用户通过同一子网登录，或进行任何数量的其他分析。

还有相当多的其他 SIEM 平台可与 OpenStack 集成，并且可以摄取日志文件或 Ceilometer 馈送，其中一些列于此处：

- Symantec Security Information Manager
- Splunk
- HP/Arcsight
- Tripwire
- NetIQ
- Quest Software
- Enterprise Security Manager

虽然开源和商业解决方案都具有出色的功能集，但每个企业都需要自行评估 SIEM 解决方案。 然而，有了前面的信息，管理员可以感到有能力为 OpenStack 启用 CADF 事件日志记录，并为任何平台提供其满足企业审计要求所需的数据。

### 总结

在本章中，您学习了一些关于 OpenStack 安全的整体方法。 这不仅包括运行在 OpenStack 之上的应用程序，还包括构成平台的项目以及基础操作系统环境。 您了解到企业 OpenStack 安全不仅仅是加密、日志记录或修补，而是一种更以企业为基础的方法，其中所有解决方案必须与其他解决方案和谐工作。 有时这种互操作性可以通过配置来实现，有时则需要根据安全要求添加额外的开源或商业级企业软件。 然而，一个非常重要的观点是认识到 OpenStack 并没有比其竞争平台更不安全，并且有策略和工具可用于使 OpenStack 符合大多数企业安全策略。

### 参考

- https://security.openstack.org/ossalist.html
- https://review.openstack.org/#/c/299025/
- https://wiki.openstack.org/wiki/Vulnerability_Management
- https://www.openstack.org/assets/survey/April-2016-User-Survey-Report.pdf
- http://www.pulpproject.org/
- http://libvirt.org/remote.html#Remote_certificates
- http://www-03.ibm.com/software/products/en/qradar
- https://github.com/openstack/pycadf/tree/master/etc/pycadf
