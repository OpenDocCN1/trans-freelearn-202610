

- **不要忘记网络**：在本地运行集群时，很容易忽略或视若理所当然地对待网络基础架构。在可行的情况下，将网络协议栈添加到监控中。一些组织将网络和服务器运维团队分开，这可能会使将网络协议栈添加到 Docker 监控中在政治上存在问题。在这种情况下，与网络团队成为朋友。

- **创建有用的仪表板**：能够在同一位置查看相关组件的日志和指标可以简化故障排除。诀窍在于知道什么值得查看，什么不值得。过多信息与信息过少一样成问题。

### 使用 Sysdig 实时监控服务

Sysdig 是一种实时命令行监控工具。它可以监控大多数主机组件，并支持报告容器统计信息。它还支持查看基于 Kubernetes Pod、复制控制器、服务等的指标。

在可能的情况下，Sysdig 应直接安装并运行在主机上。这样可以使其对系统拥有最大可见性。Debian 和基于 Red Hat 的系统提供了安装包。安装说明可在 http://www.sysdig.org/install/ 找到。

也可以在容器中运行 Sysdig。如果 Docker 主机运行的 Linux 发行版不受直接支持，或者像 CoreOS 这样在容器中运行所有内容的系统，这尤其有用。启动容器的命令如下：

```
$ docker run -it --rm --name=sysdig --privileged=true \
--volume=/var/run/docker.sock:/host/var/run/docker.sock \
--volume=/dev:/host/dev \
--volume=/proc:/host/proc:ro \
--volume=/boot:/host/boot:ro \
--volume=/lib/modules:/host/lib/modules:ro \
--volume=/usr:/host/usr:ro \
sysdig/sysdig
```

需要注意以下几点：

- 首先，在容器中运行 `sysdig` 需要使用 `--privileged` 标志。它需要此权限才能加载 `sysdig` 内核模块并访问所有所需信息。
- 第二，该命令映射了多个重要的文件系统，例如 `/proc`，以实现可见性。
- 第三，/var/run/docker.sock 被映射到容器中，以便 sysdig 可以检索正在运行的 Docker 容器的信息。

安装 sysdig 后，运行命令 `cysdig` 启动 curses 界面。这提供了一个类似 top 的界面，让你查看服务器上正在发生的事情。使用 F2 切换视图。`-pc` 标志将容器信息添加到许多可用视图中：

```
$ cysdig -pc
```

![](img/02bead9c8ff4515f78dac0eb33163961_223_0.png)

Sysdig 在实时排查系统问题时非常有价值。每个视图都有深入选项，可以查看特定事物的更多信息，该事物可以是容器、网络连接、主机进程或其他许多项目。

## 摘要

本章介绍了如何将 Docker 日志和指标发送到远程服务。它展示了如何使用 Kibana 查看存储在 Elasticsearch 中的日志，以及如何使用 Kibana 和 Grafana 基于存储在 Elasticsearch 和 InfluxDB 中的指标构建图表和仪表板，以快速提供有用信息。还介绍了一些监控 Docker 集群中服务的一般提示。最后，介绍了 Sysdig，它可以提供对主机和容器的实时监控。下一章将介绍如何使用持续集成来构建、测试和部署容器。

## 使用持续集成构建、测试和部署容器

运行容器最重要的部分之一是构建容器所基于的镜像。毕竟，没有镜像就没有容器。一个挑战是确保 Docker 镜像不仅能够构建，而且在部署 *之前* 也能正常工作。

本章将演示如何使用 GitLab 为 Docker 镜像实施 **持续集成**（CI）。它将展示如何自动构建镜像、针对其运行测试套件、将新镜像推送到 Docker Registry，甚至将新镜像部署到 Docker 集群。

本章将涵盖以下主题：

- 使用 CI 的重要性
- 在 GitLab 中为 CI 设置项目
- 自动构建新镜像
- 针对镜像运行测试
- 发布经过测试的镜像
- 自动部署镜像
- 使用 GitLab CI 创建 DAB 文件
- 使用 Docker Cloud 进行镜像测试

### 使用 CI 的重要性

值得牢记如何定义 Docker 应用程序。在最基本的层面上，Docker 应用程序实际上与任何软件项目没有区别。它只是一个 `Dockerfile`，而镜像所需的资源很可能存储在版本控制中。它是软件。

将 Docker 应用程序视为代码意味着管理员或管理员团队可以从软件开发方法论的所有进步中受益。其中之一就是项目的自动构建和测试。这个过程称为 CI。每当对应用程序进行更改时，无论是配置文件的小改动还是对实际应用程序的升级，都可以自动重新构建和测试镜像，以确保部署时镜像能按预期工作。

我经常开玩笑说，作为系统管理员，我的工作是让其他人或事物为我工作。现实是，我能让组织中其他人获得多少权力，我们就能变得多么高效。例如，想象一个正在开发 Web 应用程序的开发团队。在传统模型中，这些开发人员必须等待管理员将任何更改部署到测试和生产环境，并希望在这个过程中不会出任何问题。使用合适的 CI 工作流意味着，无论开发人员何时准备好部署，CI 工具都可以安全地部署应用程序，而无需等待管理员抽出时间。最终结果是更快、更一致、更可靠的部署。

### 使用 GitLab 进行 CI

GitLab 是 Docker 镜像和服务 CI 与部署的绝佳工具。它将 Git 与问题跟踪、Docker Registry 和 CI 相结合，提供统一的体验。然而，此过程并非 GitLab 特有。本节所学的经验无论使用什么系统都应该适用。

### 为 CI 设置 GitLab

GitLab CI 作为 GitLab.com 提供服务的一部分免费提供。也可以付费购买提供更多资源的企业版许可证。GitLab 的免费社区版和商业企业版也可以本地安装。在第 3 章“集群构建块 – Registry、Overlay Networks 和 Shared Storage”中，我们展示了如何安装 GitLab 以及如何启用 Docker Registry。

**Registry** 是使 CI 工作的重要部分。它为成品镜像在集群中使用提供存储位置。可以使用其他 Registry 代替 GitLab。例如，组织可以选择使用 Docker Hub 或 Docker Datacenter 中的安全 Registry。

从 GitLab 8.0 开始，GitLab CI 已完全集成到主程序包中。除了一个例外外，在 GitLab 中无需再安装或启用其他内容即可使用这些功能。最后需要的组件是 GitLab Runner。

### 在 Ubuntu 上安装 GitLab Runner

GitLab Runner 的工作是执行实际的镜像构建。每个构建都会排队并依次处理。可以使用多个 Runner 来允许并行执行多个构建。Runner 可以打标签以运行特定任务，例如构建 Docker 镜像或为 Microsoft Windows 构建应用程序。

GitLab.com 提供了一个可用于构建镜像的共享 Runner。这对于较小的项目或测试非常有用。较大的组织应考虑购买企业版许可证或在本地安装自己的 Runner。如果镜像测试包含敏感信息，或者需要连接到无法从 Internet 访问的资源，也可能希望使用本地 Runner。

运行 GitLab Runner 的首选方法是将其原生安装在 Debian、Ubuntu、RHEL 或 CentOS 上。但是，也有官方 Docker 镜像。有关使用该镜像的详细信息，请参阅 https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/blob/master/docs/install/docker.md。

