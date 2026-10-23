

通过此设置，需要访问互联网的工业服务器/客户端/工作站或其他设备，其代理配置将被定义为指向 IDMZ Squid 代理服务。Squid 服务使用 **访问控制列表 (ACLs)** 来将系统列入白名单或黑名单以访问互联网，并采用类似的方法来允许或阻止访问互联网上的 URL 或 IP 地址。除允许围绕工业系统和 URL 创建 ACL 以将互联网访问列入白名单/黑名单外，前向 Web 代理代理服务还会将客户端隐藏在 Web 服务器之外，增加了一层保护。

Squid 设置的详细描述超出了本书的范围，但简单的网络搜索将得到大量关于该主题的教程和“如何操作”。

## 配置

在企业区，我们需要实施以下配置以支持此代理服务解决方案：

- 企业区系统的配置取决于具体情况。通常，由于 Squid 在企业侧通过 HTTP(S) 通信，系统无需（重新）配置。然而，防火墙、企业 DMZ 或其他系统中的 ACL 可能需要允许 Squid 与这些系统通信或通过这些系统通信。

IDMZ 防火墙使用以下配置来支持此代理服务解决方案：

- 添加一条防火墙规则，允许一组 **工业** IP 地址通过端口 3128（Squid 代理服务端口）访问 **IDMZ** Squid 服务器。
- 添加一条防火墙规则，允许 **IDMZ** Squid 服务器通过工业系统需要访问企业/互联网 Web 访问的任何服务/端口访问一组 **企业** IP 地址。这些可以是企业 Web 服务器/服务的 IP 地址/范围/子网或互联网上的 Web 服务器的 IP 地址/范围/子网。

IDMZ 交换机使用以下配置来支持此代理服务解决方案：

- 为 Squid 服务器创建一个专用 VLAN，并在与 IDMZ 防火墙的链路上创建相应的子接口。

IDMZ 代理服务/服务器使用以下配置来支持此代理服务解决方案：

- 创建一个专用 vSwitch，对应于在 IDMZ 交换机上创建的 VLAN。
- 在 IDMZ 中的 CentOS VM 上安装并配置 Squid 服务。
- 在 CentOS 机器上，向 `/etc/squid/squid.conf` 添加 ACL 以允许工业系统/子网访问互联网。以下代码片段为此提供示例：

```
acl localhost src 127.0.0.1/32
acl IndustrialClient src 192.168.0.107
...
http_access allow IndustrialClient
http_access allow localhost
```

- 在 CentOS 机器上，创建 `/etc/squid/allowed_domains` URL/域名白名单并添加你希望允许访问的 URL 或域名。以下代码片段为此提供示例：

```
.microsoft.com
https://rockwellautomation.custhelp.com/
...
```

- 在 CentOS 机器上，向 `/etc/squid/squid.conf` 添加 ACL 以允许访问白名单的 URL/域名。以下代码片段为此提供示例：

```
acl allowed_domains dstdomain "/etc/squid/allowed_domains"
...
http_access allow IndustrialClient allowed_domains
```

在工业区，我们需要实施以下配置以支持此代理服务解决方案：

- 任何需要访问互联网/企业 URL 的系统或设备都需要配置代理设置，使其指向 Squid IDMZ 服务，如下所示：

```
<squid-server-ip>:3128
```

设计和配置此代理服务解决方案时应考虑以下设计和安全注意事项：

- 对于你希望授予互联网访问权限的工业系统和设备，以及你希望允许访问的 URL/域名，均坚持采用白名单方法。

通过此设置，企业客户端、服务器或其他系统可以通过配置为反向 Web 代理代理服务的 nginx Web 服务器访问工业基于 Web 的资源。反向 Web 代理代理服务会将服务器隐藏在客户端之外，以保护工业基于 Web 的资源。

### 配置

在企业区，我们需要实施以下配置以支持此代理服务解决方案：

企业区无需特殊配置。反向 Web 代理将表现为一个 URL/Web 页面，允许企业用户通过 URL 访问工业服务。例如，`http://comp-xyz-rev-prx/ind-core` 将允许通过企业暴露的 `comp-xyz-prx` Web 服务器访问工业核心交换机的 Web 界面。

IDMZ 防火墙使用以下配置来支持此代理服务解决方案：

- 添加一条防火墙规则，允许（一组）**企业** IP 地址通过端口 `443`（SSL）和/或端口 `80`（HTTP）访问 **IDMZ** 反向 Web 代理服务器。
- 添加一条防火墙规则，允许 **IDMZ** 反向 Web 代理服务器通过端口 `443`（SSL）和/或端口 `80`（HTTP）访问一组 **工业** IP 地址。

IDMZ 交换机使用以下配置来支持此代理服务解决方案：

- 为 nginx 服务器创建一个专用 VLAN，并在与 IDMZ 防火墙的链路上创建相应的子接口。

IDMZ 代理服务/服务器使用以下配置来支持此代理服务解决方案：

- 创建一个专用 vSwitch，对应于在 IDMZ 交换机上创建的 VLAN。
- 在 IDMZ 中的 CentOS VM 上安装并配置 nginx 服务。例如，考虑以下配置片段，取自 IDMZ 中 CentOS VM 上的 `/etc/nginx/nginx.conf`。该片段显示了设置到工业核心交换机 Web 门户（`http://192.168.10.1`）的代理转发器的配置：

```
    ...
    server {
        listen   80 default_server;
        listen   [::]:80 default_server;
        server_name comp-xyz-rev-prx;
        proxy_redirect     off;
        proxy_set_header  X-Real-IP $remote_addr;
        proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header  Host $http_host;
        location /ind-core-switch {
            proxy_pass http://192.168.10.1/;
        }
    }
    ...
```

在工业区，我们需要实施以下配置以支持此代理服务解决方案：

- 工业网络上的目标系统无需 IDMZ 代理服务特定配置。（基于主机的防火墙可能需要配置以允许来自反向 Web 代理服务器的访问，或设备上的 ACL 需要更新以允许访问。）

## 更新工业区的 Microsoft Windows 系统

我们将要讨论的最后一个 IDMZ 代理服务解决方案是一种将 Microsoft Windows 更新引入工业环境的方法。补丁和更新是安全计划的关键方面，因此能够在工业区修补系统应位列你的愿望清单之首。

### 设计

本节介绍的 Microsoft Windows 更新代理服务解决方案基于 Microsoft 的 WSUS，该服务作为 IDMZ 中 Microsoft Windows Server 2019 VM 上的角色安装。这在以下图表中有所体现：

![](img/c8ffa9959721097701584fcc95101362_109_0.png)

图 3.11 – Microsoft Windows 更新 IDMZ 代理服务场景

通过此设置，工业 Microsoft Windows 客户端、服务器和工作站可以检查 IDMZ 中的 WSUS 服务器是否有新补丁和更新。IDMZ WSUS 可以配置为直接与 Microsoft 更新服务器检查新内容，或指向企业 WSUS 服务器。此外，还可以在生产区域内部添加下游服务器，以允许更新不在第 3 级站点运营中的 Microsoft Windows 系统。

## 配置

在企业区，我们需要实施以下配置以支持此代理服务解决方案：

- 如果使用企业 WSUS 服务器供 IDMZ WSUS 服务器从中拉取更新，则该服务器需要配置为拉取并存储工业网络上可能存在的任何 Microsoft 产品相关的更新。

IDMZ 防火墙使用以下配置来支持此代理服务解决方案：

- 添加一条防火墙规则，允许工业 IP 地址通过端口 8530（HTTP）或端口 8531（HTTPS）访问 IDMZ WSUS 服务器以检查更新。
- 添加一条防火墙规则，允许 IDMZ WSUS 服务器通过端口 8530（HTTP）或端口 8531（HTTPS）访问企业 WSUS 服务器 IP 地址，或通过端口 80（HTTP）或端口 443（HTTPS）访问互联网上的 Microsoft 更新服务器。

IDMZ 交换机使用以下配置来支持此代理服务解决方案：

- 为 WSUS 服务器创建一个专用 VLAN，并在与 IDMZ 防火墙的链路上创建相应的子接口。

IDMZ 代理服务/服务器使用以下配置来支持此代理服务解决方案：

- 创建一个专用 vSwitch，对应于在 IDMZ 交换机上创建的 VLAN。
- 构建一个 Microsoft Windows Server 2016/2019 VM 并安装 WSUS 角色。
- 配置 IDMZ WSUS 服务以与 Microsoft 更新服务器或企业上游 WSUS 服务器同步，并启用拉取工业环境中可能存在的任何相关 Microsoft 产品的补丁和更新。

