
### 方法 2 – 为公共 API 和私有 API 分别使用专用负载均衡器

`api-internal.domainname.com` => `internal-<lb-name>.lb.anypointdns.net` (A record)

每个证书将拥有其自己的映射规则。您可以创建映射规则，当请求通过 `api.domainname.com` 到达时将流量导向公共应用程序，以及另一组映射规则，当请求通过 `api-internal.domainname.com` 到达时将流量导向私有应用程序。当专用负载均衡器接收到请求时，它会在请求中查找主机标头，以根据证书端点和后续映射规则匹配请求。例如，假设公共请求通过 `http://api.domainname.com` 从客户端发送到专用负载均衡器。在这种情况下，标头中的主机值将为 `api.domainname.com`，专用负载均衡器将根据主机匹配证书并应用后续映射规则。流量将被转发给 API。

为公共和私有流量使用单个专用负载均衡器的问题在于：当恶意行为者使用修改后的主机标头，通过专用负载均衡器的公共域名以此为目标访问私有应用程序时，就会出现该问题。

例如，恶意行为者可以发送请求到 `api-internal.domainname.com`，并将主机标头设置为 `api-internal.domainname.com`。此请求将匹配私有证书并执行后续映射规则，从而允许通过专用负载均衡器的公共域名访问私有应用程序：

![图 9.23 - 用于公共和私有 API 的单个专用负载均衡器](image/B19922_09_23.jpg)

图 9.23 - 用于公共和私有 API 的单个专用负载均衡器

在图 9.23 中，您可以看到恶意行为者如何通过发送主机值 `api.domainname.com`，经由专用负载均衡器的公共域名访问私有应用程序。另一个问题是允许列表 CIDR 对专用负载均衡器是全局的。如果相同的允许列表 CIDR 同时应用于专用负载均衡器的公共域和私有域，则无法添加特定的私有 IP 范围。要做到这一点，您需要移除 `0.0.0.0/0`，这意味着将不允许公共流量。

为克服使用单个专用负载均衡器的问题，您应该为公共 API 和私有 API 使用单独的专用负载均衡器。采用此方法，您必须在单独的专用负载均衡器中为私有域和公共域配置单独的证书，并使用唯一的映射规则。除此之外，您可以为两个专用负载均衡器配置单独的允许列表 CIDR：

*   **专用负载均衡器（公共）和允许列表 CIDR (`0.0.0.0/0`)**：允许所有人访问。
*   **专用负载均衡器（私有）和允许列表 CIDR (`192.168.1.0/24`, `10.0.1.0/22`)**：允许访问私有 IP 地址范围：

![图 9.24 - 为公共和私有 API 分别使用专用负载均衡器](image/B19922_09_24.jpg)

图 9.24 - 为公共和私有 API 分别使用专用负载均衡器

恶意行为者使用公共专用负载均衡器访问私有 API 的请求将被阻止，因为没有证书与公共专用负载均衡器的主机标头匹配。公共和私有专用负载均衡器的允许列表 CIDR 是分开的。这意味着您可以完全阻止公共流量，并仅允许来自特定私有 IP 地址范围的流量访问私有专用负载均衡器。

# 将 MuleSoft 应用程序部署到 CloudHub 1.0 Runtime Manager 的不同选项

MuleSoft 提供了多种将应用程序部署到 CloudHub 1.0 的方式，所有这些方式都可以与 CI/CD 流水线集成，以实现 MuleSoft 应用程序到 CloudHub 1.0 的自动部署。让我们探讨一下将应用程序部署到 CloudHub 1.0 的所有方式。

## Mule Maven 插件

Mule Maven 插件是用于将 MuleSoft 应用程序部署、取消部署和打包到 CloudHub 1.0 的最重要的 Maven 插件之一。让我们更详细地讨论 Mule Maven 插件。可以在应用程序的 `POM.xml` 文件中配置以下插件，其中包含在 CloudHub 1.0 中部署和运行应用程序所需的所有属性：

```xml
<plugin>
  <groupId>org.mule.tools.maven</groupId>
  <artifactId>mule-maven-plugin</artifactId>
  <version>3.8.6</version>
  <extensions>true</extensions>
  <configuration>
    <cloudHubDeployment>
      <uri>https://anypoint.mulesoft.com</uri>
      <muleVersion>${app.runtime}</muleVersion>
      <username>${anypoint.username}</username>
      <password>${anypoint.password}</password>
      <applicationName>${application.name}</applicationName>
      <environment>${mule.environment}</environment>
      <region>${mule.region}</region>
      <workers>${mule.workers}</workers>
      <workerType>${mule.workerType}</workerType>
      <properties>
        <key>value</key>
      </properties>
    </cloudHubDeployment>
  </configuration>
</plugin>
```

在前面的 Mule Maven 插件中，使用 `${}` 定义的值是您可以在 `mule deploy` 命令中传递的参数，如下所示：

```
mule deploy –DmuleDeploy -Dapp.runtime=4.3.0 -Danypoint.username=anypointusername –Danypoint.password=password -Dapplication.name=music-api-dev –Dmule.environment=Sandbox –Dmule.workers=1 –Dmule.workerType=MICRO
```

以下是在将应用程序部署到 CloudHub 1.0 时可以在 Mule Maven 插件中配置的属性：

| 属性 | 描述 | 是否必需 | 默认值 / 示例 |
| --- | --- | --- | --- |
| `cloudHubDeployment` | 根元素。 | 是 | |
| `uri` | 您的 Anypoint Platform URI。 | 否 | |
| `muleVersion` | 在 CloudHub 2.0 实例中运行的 Mule 运行时引擎版本。 | 是 | 例如，`4.3.0` |
| `username` | CloudHub 用户名。 | 仅当使用 Anypoint Platform 凭据登录时 | |
| `password` | CloudHub 密码。 | 仅当使用 Anypoint Platform 凭据登录时 | |
| `applicationName` | 要在 Runtime Manager 中实现的应用程序标题。 | 是 | |
| `artifact` | 要部署的 JAR 文件的绝对路径。 | 否 | |
| `environment` | 将部署应用程序的 CloudHub 环境。 | 是 | |
| `properties` | 可以传递在应用程序部署期间需要设置的属性。 | 否 | |
| `workers` | 工作器数量。 | 否 | `1` |
| `workerType` | 每个工作器的大小。 | 否 | `MICRO` |
| `region` | 工作器云的区域。 | 否 | `us-e1` |
| `objectStoreV2` | 启用 Object Store V2。 | 否 | `true` |
| `persistentQueues` | 启用持久队列。 | 否 | `false` |
| `businessGroup` | 部署的业务组路径。 | 否 | |
| `businessGroupId` | 业务组 ID 或组织。 | 否 | |
| `deploymentTimeout` | 部署过程允许的持续时间（以毫秒为单位）。这指的是从部署开始到确认工件成功部署之间的时间。 | 否 | `1000000` |
| `server` | Maven 服务器与 Anypoint Platform 凭据集成。服务器 ID 可以在 `settings.xml` 文件中找到，该文件关联了用户的 Anypoint 用户名和密码。 | 否 | |
| `skip` | 启用时，它会跳过插件的部署目标。 | 否 | `false` |
| `skipDeploymentVerification` | 启用时，它会跳过对已部署应用程序状态的确认。 | 否 | `false` |
| `authToken` | 指定用于访问平台的授权令牌。 | 仅当使用授权令牌登录时 | |
