
-   **Keywhiz:** 该驱动程序可用于使容器与远程 Keywhiz 服务器通信。Keywhiz 是一种用于管理和分发机密数据的系统，例如 TLS 证书/密钥、GPG 密钥、API 令牌和数据库凭据。不是将此数据放在配置文件中或复制文件（这可能会泄露或难以追踪），Keywhiz 使其管理更轻松、更安全：集群中的 Keywhiz 服务器将机密集中存储在加密的数据库中。客户端使用相互认证的 TLS（mTLS）检索其有权访问的机密。

正如前面的示例所示，它们相当有趣，有时甚至很奇特。由于 Docker 及其插件架构的可扩展性，您可以创建非常灵活的设置。但第三方驱动程序并不总是引入完全新的存储类型，有时它们只是扩展现有驱动程序。一个例子是 `Local Persist Plugin`，这是一个卷插件，它通过允许您在主机上任意位置指定挂载点来扩展默认本地驱动程序的功能，从而使文件始终保持持久，即使通过 Docker 的 `volume rm` 命令删除卷也是如此。

如果您需要尚未可用的卷插件，您可以自行编写——该流程在 Docker 的 GitHub 页面上记录得非常详细，并附带可扩展的示例。

## Summary

我们现在已经介绍了如何将容器开放给外部世界——我们可以使用网络和挂载的卷在容器之间以及容器与其他主机之间共享数据。让我们总结一下本章迄今为止学到的内容。我们知道，来自同一 Docker 主机的容器在默认的 `bridge` 网络上会自动相互看到，前提是容器绑定到默认桥接。我们可以使在不同主机上运行的容器相互通信。为此，我们需要多主机 `overlay` 网络。您需要运行在 swarm 模式下的 Docker 1.12 或更高版本，或者 alternatively 运行一个键值存储。如果 Docker 提供的驱动程序不足，我们可以使用网络插件进一步扩展网络数据交换。我们已经介绍了卷创建过程，并且知道卷即使在容器重启后也能持久保存数据；我们还知道对卷上文件的更改是直接进行的，但在更新镜像时不会包含这些更改。

卷允许在主机文件系统与 Docker 容器之间，或在 Docker 容器之间共享数据，并且即使容器本身被删除，它们也会持久存在。我们可以使用卷驱动程序进一步扩展文件交换的可能性。

在下一章中，我们将重点关注 Docker Hub。我们将学习如何查找镜像，这些镜像将成为下一章的基础，在下一章中我们将使用 Dockerfile 实际创建镜像。

## 5

## Finding Images

在之前的章节中，我们学习了如何安装 Docker，并熟悉了 Docker 架构、网络和持久存储功能。我们在 `Chapter 3`、*Understanding Images and Containers* 中稍微提到了使用 Docker Hub 和远程仓库的工作，但这次我们将深入详细了解。

本章将涵盖以下主题：
-   The Docker Hub
-   Image tags
-   Searching for images using the web interface and a command line
-   Docker Hub REST API

在下一章实际构建自己的镜像之前，我们将首先专注于使用 Docker Hub——创建账户、身份验证、查找镜像并拉取它们。我们将更详细地介绍版本和标签，解释最容易被误解的 Docker 标签之一 `latest`。本章还将介绍 Docker Hub 的 REST API，如果您计划以编程方式使用 Docker Hub——来自您自己的应用程序或集成流程，这可能会派上用场。

这次我们还将在 Docker Hub 上创建账户——当我们将镜像发布到世界时，这是强制性的。

## The Docker Hub

Docker 镜像仓库的主要功能是根据描述查找特定镜像。仓库在仓库中处理 Docker 镜像。了解 registry 和 repository 之间的区别很重要。Docker registry 是一个存储 Docker 镜像的服务，而 repository 是具有相同名称的不同 docker 镜像的集合，它们具有不同的标签（我们稍后会回到标签）。与 git 相当，Docker 仓库由 URI 标识，可以是公共的也可以是私有的。

最知名的托管 Docker registry 版本是 Docker Hub。它提供了一个美观且易用的 UI。Docker Hub 是一个用于共享和管理 Docker 镜像的软件即服务（SaaS）平台。换句话说，它是一种能够托管和分发镜像的基于云的 registry 服务。镜像保存在公共或私有仓库中。目前，Hub 提供数十万个公共仓库，并且数量每天都在增长。其中近三分之一的公共仓库使用自动构建，这意味着镜像是由 Hub 本身使用已发布的 Dockerfile 构建的。其余仓库包含通过 `docker push` 命令手动上传的镜像。Docker Hub 可在 https://hub.docker.com/ 获取。

![](img/0c87321919fa245e22f056c67ec70003_113_0.png)

Docker Hub 是默认的 registry。如果您在使用 `login`、`pull` 或 `push` 等 registry 相关命令时未提供 registry 主机名，Docker 将使用 Docker Hub。

Registry 提供诸如推送（将镜像上传到仓库）和拉取（从仓库下载镜像）等操作。Docker registry 还有另一个重要角色，即索引。索引维护有关用户账户、镜像校验和以及公共命名空间的信息。事实上，Docker Hub 是公共仓库的集中式索引。

Docker Hub 是一个公共服务，如果您注册账户，您可以免费获得一个私有仓库。如果您需要更多私有仓库，您可以升级 Docker Hub 计划，这将不是免费的。

Docker Hub 不仅仅是一个 registry 和索引——一个搜索、存储或获取镜像的地方——它更像是一个伞形项目；它允许链接到代码仓库（如 GitHub 或 BitBucket），创建镜像的自动构建并对其进行测试。Docker Hub 还允许链接到 Docker Cloud，以便您可以将镜像部署到自己的主机上。所有这些方面在开发管道中创建镜像工作流自动化时都很有用。我们将在 `Chapter 8, Publishing Images` 中详细介绍这些与开发相关的功能，例如发布和创建自动构建。

除了管理单个用户账户和身份验证之外，Docker Hub 还管理组织。通过创建组织，您可以轻松地让同事访问共享镜像仓库，并让 Docker Hub 管理团队协作和变更管理。

让我们总结 Docker Hub 的功能：
-   **Accounts**: 这有助于用户管理和身份验证。
-   **Organizations**: 这用于创建工作组以管理对镜像仓库的访问。
-   **Image repositories**: 这用于从社区、官方或私有镜像仓库中搜索、`pull`、`push` 和 `manage` 镜像。
-   **Automated builds and web hooks**: 这用于在源代码发生变化时自动触发镜像构建过程。Web Hook 是自动构建的一部分，允许您在将镜像推送到仓库后执行不同的操作。
-   **Integration source code repositories**: 这用于将您的 GitHub 和/或 Bitbucket 源代码仓库链接起来以创建开发工作流。
-   **REST API**: 通过 Docker Hub Web 界面提供的几乎所有功能也都作为一个记录良好的 REST 端点提供，您可以使用它以编程方式处理 Docker Hub。

Docker Hub 账户（称为 Docker ID）对于搜索和拉取镜像不是强制性的。事实上，正如您所记得的，我们在之前的章节中拉取了大量 `hello-world`、`nginx` 和 `ubuntu` 镜像来演示 Docker 的架构，而没有注册适当的 Docker ID。

![](img/0c87321919fa245e22f056c67ec70003_114_0.png)

Docker Hub 账户对于从 Docker Hub 搜索和拉取镜像不是强制性的——您可以在不登录甚至没有账户的情况下下载镜像。

然而，为了发布镜像、发布评论或为仓库加星，您需要一个 Docker ID。通过留下评论，您可以与其他 Docker 社区成员以及特定镜像的维护者互动。

![](img/0c87321919fa245e22f056c67ec70003_114_1.png)

像任何成熟的社区一样，您的评论应精确且有意义。如果您发现任何不合适的评论，您可以将其标记以供审查。
