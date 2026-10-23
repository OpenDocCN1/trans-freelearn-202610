

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

