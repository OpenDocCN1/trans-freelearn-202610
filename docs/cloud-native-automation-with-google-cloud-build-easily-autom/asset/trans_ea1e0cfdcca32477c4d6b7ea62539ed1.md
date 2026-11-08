
前面的屏幕截图显示了 UI 编辑器中的管道配置。以下代码是 cloudbuild.yaml 配置的完整示例：

```
steps:
[1] - name: gcr.io/cloud-builders/git
  args:
    - '-c'
    - |
      echo "$${GITLAB_SSH_KEY}" > /root/.ssh/id_rsa
      chmod 400 /root/.ssh/id_rsa
      ssh-keyscan gitlab.com > /root/.ssh/known_hosts
  entrypoint: bash
  secretEnv:
    - GITLAB_SSH_KEY
  volumes:
    - name: ssh
      path: /root/.ssh
[2] - name: gcr.io/cloud-builders/git
  args:
    - clone
    - '-n'
    - 'git@gitlab.com:##redacted_repo_org##/packt-cloudbuild'
    - .
  volumes:
    - name: ssh
      path: /root/.ssh
[3] - name: gcr.io/cloud-builders/git
  args:
    - checkout
    - $_TO_SHA
  - name: gcr.io/cloud-builders/docker
    args:
      - build
      - '-t'
      - 'us-central1-docker.pkg.dev/${PROJECT_ID}/image-repo/myimage'
      - .
```

```yaml
- name: gcr.io/cloud-builders/docker
  args:
    - push
    - 'us-central1-docker.pkg.dev/${PROJECT_ID}/image-repo/myimage'
availableSecrets:
[4] secretManager:
  - versionName: projects/##REDACTED-project_number/secrets/GITLAB_SSH_KEY/versions/1
    env: GITLAB_SSH_KEY
```

已添加突出显示的编号以供参考。我们将使用它们逐步介绍并描述每个步骤：

- 1. 此步骤准备安全的 Git 连接，并将 GitLab ssh 密钥变量 GITLAB_SSH_KEY 的内容存储在挂载的卷中。
- 2. 使用存储在挂载卷中的密钥克隆指定的私有仓库。
- 3. 通过使用 checkout 命令检索特定提交 SHA 的内容。SHA 从触发器配置中指定的 `$_TO_SHA` 替换变量中检索。如果您还记得，该变量引用自 GitLab 触发的 webhook URL 的 HTTP POST 中来的 `${body.after}` 值。Cloud Build 解析 JSON 正文内容并使其可供管道使用。
- 4. 检索存储在 Secret Manager 中的 GitLab ssh 密钥。指定了 secret 上下文路径以定位特定的 secret。然后将其关联到 GITLAB_SSH_KEY 变量。

此示例 cloudbuild.yaml 配置包含几个步骤来克隆并检出特定的提交标签。一旦仓库可由 Cloud Build 访问，我们就可以继续执行完成管道的步骤。

## 手动触发器

手动触发器可以通过多种机制调用，以扩展自定义集成的类型。具有适当凭据且对构建管道至关重要的现有系统工具可以使用 gcloud CLI 或 REST API 与手动触发器集成。

以下是以 gcloud 命令执行手动触发器的示例：

```bash
$ gcloud beta builds triggers run [trigger_name] \
--region global
```

另一个利用手动触发器的机制是当构建管道需要在特定时间或间隔执行时。Cloud Scheduler 与 Cloud Build 直接集成，以便在指定时间执行 Cloud Build 手动触发器。从 Cloud Build 手动触发器中，您可以按计划运行触发器，该计划可通过 UI 创建：

![](img/2fe9576240c146c49e3a21d5e7f6af00_100_0.png)

图 5.7 – Cloud Scheduler 作业创建

在向导中，您选择 Cloud Scheduler 用于调用触发器的服务账号。可以创建默认服务账号，也可以指定已设置适当权限的账号。

![](img/2fe9576240c146c49e3a21d5e7f6af00_101_0.png)

图 5.8 – Cloud Scheduler 集成

前面的屏幕截图是 Cloud Build 手动触发器生成的 Cloud Scheduler 作业。请注意，Cloud Scheduler 使用 Cloud Build REST API 来调用手动触发器。

## 摘要

Cloud Build 提供了多种与现有系统工具和 SCM 平台集成的方式，以参与自动化构建管道。Cloud Build 触发器提供了执行构建管道的能力。这可以通过 SCM 事件（例如提交到分支）、外部因素（例如成为另一个系统工具 larger automation pipeline 的一部分）或在指定时间来实现。

在本章中，我们展示了将 SSH 密钥存储在 Secret Manager 中以供构建管道使用的示例。在下一章中，我们将进一步探讨 Cloud Build 的安全最佳实践。

## 管理环境安全

在托管服务中，必须考虑保护资源的共同责任。提供商负责保护基础设施、管理其上运行的服务以及提供安全相关功能。消费者负责谁有权访问该服务以及保护托管服务之外的数据。

在本章中，我们将介绍 Cloud Build 如何在与其它安全相关服务集成的同时提供安全性的以下主题：

- 防御纵深
- 最小权限原则
- 访问敏感数据和密钥
- 为容器镜像构建元数据
- 保护网络边界

## 防御纵深

计算中的纵深防御概念由信息安全使用，以确保在每一层都部署安全结构以进行保护。Cloud Build 确保这些安全结构到位；一些示例在图 6.1 中注明：

![](img/2fe9576240c146c49e3a21d5e7f6af00_104_0.png)

虽然 Cloud Build 提供了此处提到的功能，但某些服务必须在管道中启用或利用。市场上还有其他安全相关解决方案可能与 Cloud Build 没有直接集成，但由于 Cloud Build 对每个步骤中自定义容器镜像的支持，它们仍可用于管道步骤。例如，如果您的容器镜像具有正确的库和工具，您可能能够从 HashiCorp Vault 检索值。

还有一些需要注意的其他示例，可能会导致管道共享敏感信息：

- 输出并记录敏感信息的步骤
- 使用不安全协议与其它系统通信的步骤
- 在管道中使用未知或存在漏洞的容器镜像作为步骤
- 在步骤之间将敏感信息存储在 Cloud Build 工作目录中

纵深防御是整体安全策略的组成部分；因此，共同责任模型也适用。如前所述，Cloud Build 提供了许多用于保护管道的元素；然而，服务消费者也必须采取某些步骤和措施。

## 最小权限原则

最小权限原则是保护资源的另一个安全结构。其目标是仅提供完成工作所需的资源访问权限。如果管道不需要访问对象存储中的数据，则没有理由授予调用管道的参与者的访问权限。例如，组织在开发阶段可能对安全权限限制较少，但在生产阶段限制更严格。虽然这可能使启动变得更容易，但不一致可能会导致团队进入更高级别的环境时出现问题，造成不必要的故障排除任务。组织内部将安全左移的文化运动可能涉及对资源的最小特权访问概念。这因组织而异，因为不同的监管机构和行业可能有不同的要求。

想象一下，如果恶意行为者获得访问权限或组织中的某人无意中使用了服务账号。图 6.2 以视觉形式描绘了该场景：他们将能够写入服务 A，影响数据的完整性，同时还能在不需要完成管道时访问或写入服务 B 中的数据：

![](img/2fe9576240c146c49e3a21d5e7f6af00_105_0.png)

图 6.2 – 在不需要时授予额外访问权限

在利用最小权限原则时，仅向特定管道必需的服务授予访问权限。图 6.3 描绘了尽管服务存在，但由于在管道中不需要而未授予访问权限。虽然这是在构建管道的上下文中，但可以应用于组织中所有服务的各个方面：

![](img/2fe9576240c146c49e3a21d5e7f6af00_105_1.png)

图 6.3 – 将对服务的访问限制为仅所需内容
