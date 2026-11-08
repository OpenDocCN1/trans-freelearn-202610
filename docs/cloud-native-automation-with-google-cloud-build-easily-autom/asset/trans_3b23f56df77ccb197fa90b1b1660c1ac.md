
请注意，虽然这些步骤可以并行发生，但并不能保证。相反，被尊重的是构建步骤执行的顺序：

```
$ cd .. && mkdir fan-out-fan-in && cd fan-out-fan-in
$ cat > cloudbuild.yaml <<EOF
steps:

- name: 'bash'
  id: A
  args:
  - 'sleep'
  - '10'
  waitFor: ['-']

- name: 'bash'
  id: B
  args:
  - 'sleep'
  - '20'
  waitFor: ['A']

- name: 'bash'
  id: C
  args:
  - 'sleep'
  - '20'
  waitFor: ['A']

- name: 'bash'
  id: D
  args:
  - 'sleep'
  - '30'
  waitFor: ['B', 'C']
EOF

$ gcloud builds submit . --region=us-central1
```

在此示例中，步骤 A 会立即执行，而步骤 B 和 C 都等待 A 完成执行。完成后，两者并行启动，而步骤 D 则在 B 和 C 都完成后才执行。

此外，我们可以使用相同的功能来执行类似准备的步骤，例如从多个位置获取文件，以便后续的构建步骤可能需要执行任务。通过声明 A-1、A-2 和 A-3 可以同时运行来移除阻塞功能，可能可以加快构建速度，并且还可以通过减少整体构建分钟数来帮助降低成本。然而，虽然这是可能的，但并不能保证它们都会并行运行：

![](img/2fe9576240c146c49e3a21d5e7f6af00_85_0.png)

图 4.7 – 使用 waitFor 同时并发并立即运行多个构建步骤

在讨论了构建步骤之间共享数据以及编排构建步骤的顺序之后，让我们现在回顾一下不特定于任何一个构建步骤的全局配置。

## 配置构建范围规范

最后，有一些配置集可用于配置适用于所有构建步骤的全局设置。我们在前一章中已经讨论了一些：

- `timeout`
- `env`
- `secretEnv`

前述字段的行为与在单个构建步骤级别定义的对应字段相同；核心区别在于，现在全局定义它们会使它们可用于构建中的所有构建步骤。

然而，当您使用 `secretEnv` 时，无论是在单个构建步骤级别还是全局级别，都必须指定一个额外的全局字段：

- `availableSecrets`

在此字段中，您声明希望在构建的某个阶段使用的 `availableSecrets`，以及它们的来源。建议您利用 **秘密管理器** 来存储将在构建中使用的敏感凭据；我们将在 *第 8 章，使用 Cloud Build 将软件交付到 GKE 的安全性* 中详细回顾此内容。

您可以在 https://cloud.google.com/build/docs/build-config-file-schema 上找到 Cloud Build 中构建的完整配置字段列表。发布时的完整架构如下所示：

```
steps:
  - name: string
    args: [string, string, ...]
    env: [string, string, ...]
    dir: string
    id: string
    waitFor: [string, string, ...]
    entrypoint: string
    secretEnv: string
    volumes: object(Volume)
    timeout: string (Duration format)
  - name: string
    ...
  - name: string
    ...
timeout: string (Duration format)
queueTtl: string (Duration format)
logsBucket: string
options:
  env: [string, string, ...]
  secretEnv: string
  volumes: object(Volume)
  sourceProvenanceHash: enum(HashType)
  machineType: enum(MachineType)
  diskSizeGb: string (int64 format)
  substitutionOption: enum(SubstitutionOption)
  dynamicSubstitutions: boolean
  logStreamingOption: enum(LogStreamingOption)
  logging: enum(LoggingMode)
```

```
pool: object(PoolOption)
requestedVerifyOption: enum(RequestedVerifyOption)
substitutions: map (key: string, value: string)
tags: [string, string, ...]
serviceAccount: string
secrets: object(Secret)
availableSecrets: object(Secrets)
artifacts: object (Artifacts)
images:
- [string, string, ...]
```

虽然我们没有回顾此架构中的每个字段，但我们将在 *第 7、8、9 和 10 章* 中的实际应用中继续介绍剩余的配置。

## 摘要

Cloud Build 配置架构提供了一种直观、灵活且集成的方法来定义构建步骤和构建。无论您只是想运行云构建器涵盖的简单常见任务，还是构建和运行自己的自定义构建步骤以用于复杂管道，Cloud Build 的配置都具有很强的可配置性。

现在我们可以使用架构编写相当复杂的构建，让我们深入了解如何根据某些事件触发这些构建的执行，内容见 *第 5 章，触发构建*。

## 触发构建

自动化中的关键部分之一是确定管道应如何启动。这是通过为每个管道设置触发器来配置的。在第 3 章 *入门 – 我可以获得哪些构建信息？* 中，我们介绍了如何使用 CLI 通过 `gcloud` 手动启动构建。Cloud Build 提供了多种触发构建的方式，无论是集成 **Google 云平台 (GCP)** 提供的服务，还是外部机制例如 **Webhook**。在本章中，我们将了解 Cloud Build 提供的各种触发选项，同时介绍以下主题：

- 触发器的结构
- 与源代码管理 (SCM) 系统的集成
- 定义您自己的触发器

### 技术要求

- 软件构建概念和工具
- Git 仓库

### 触发器的结构

Cloud Build 触发器至少需要一些信息才能正确配置。触发器需要以下数据点：

- 名称
- 区域
- 事件触发器（默认为推送到分支）：
  - 推送到分支
  - 推送新标签
  - 拉取请求
- 其他机制
  - 手动调用 – 通过 GCP 控制台或自动化方式（CLI、REST 或 SDK）触发
  - Google Cloud Pub/Sub 消息
  - Webhook 事件 – 即向指定 URL 发出的 HTTP POST 请求
- 源仓库
- 源分支/标签：
  - 可以使用正则表达式来确定哪些分支/标签名称会触发事件。
- Cloud Build 配置文件（默认为 cloudbuild.yaml）：
  - Dockerfile – 如果您想构建容器
  - Buildpacks – 可以检测源代码语言并根据定义规范构建容器

其他字段也可以在帮助自定义触发器方面发挥重要作用：

- 变量 – 用于在构建配置中进行替换。这可以覆盖 Cloud Build 配置文件（yaml 或 json）中预定义的变量。
- 审批 – 将决定在构建开始前是否需要审批。这可以在 GCP UI 控制台中完成，也可以通过自动化方式完成。
- 服务账号 – 允许工作器使用包含针对构建的受限或特定权限的指定服务账号。

触发器中的仓库至关重要，因为它需要构建中使用的资源的来源。在上一章中，我们能够通过 CLI 在没有仓库的情况下手动提交构建，因为我们引用了本地路径。CLI 为我们打包了路径及其内容，以便作为构建的一部分使用。

![](img/2fe9576240c146c49e3a21d5e7f6af00_90_0.png)

图 5.1 – 当构建通过 CLI 手动触发且源位于本地文件系统时的示例流程

当触发器预先配置了 SCM 仓库时，构建将从集成仓库为该特定分支/标签和提交（如果可用）获取代码。

![](img/2fe9576240c146c49e3a21d5e7f6af00_91_0.png)

图 5.2 – 构建从配置的 SCM 获取仓库时的流程

本章中的命令和输出用于说明性目的，以描述功能以及在适用情况下对输出的分析。其中一些命令可能需要在执行前配置先决条件。

可以使用以下示例通过 CLI 为 GitHub 创建触发器：

```
$ gcloud builds triggers create github \
  --name=${TRIGGER_NAME} \
  --repo-name=${REPO_NAME} \
  --repo-owner=${REPO_OWNER} \
  --branch-pattern=${REGEX} \
  --build-config="cloudbuild.yaml"
```

CLI、REST 和 SDK 可用于创建、描述、更新和删除触发器。在前面的示例中，预计 Cloud Build GitHub 应用已获得对指定 ${REPO_NAME} 的授权访问权限。

我们刚刚介绍了触发器的灵活性，它允许进行不同程度的自定义。接下来，我们将深入研究与不同源代码管理平台的内置集成。
