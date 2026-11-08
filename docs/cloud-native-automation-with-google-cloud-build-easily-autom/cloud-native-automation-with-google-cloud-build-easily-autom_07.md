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



## 与源代码管理平台的集成

源代码仓库是触发器的关键组成部分，因为它依赖于仓库中的资源以及文件的快照，以便构建可以引用特定的一组源文件。Cloud Build 支持不断增长的 SCM 平台列表。以下是当前受支持的 SCM 平台列表以及它们与 Cloud Build 的集成方式（即触发器类型以及如何访问仓库）。

| SCM 平台 | 触发器类型 | 访问方式 |
|---|---|---|
| Cloud Source Repositories | 原生 | 服务账号权限 |
| GitHub | 原生 | GitHub 身份验证和 Cloud Build GitHub 应用 |
| GitHub Enterprise | 原生 | GitHub 应用和企业配置 |
| GitLab | Webhook | Secret Manager 中的 SSH 密钥 |
| GitLab Enterprise | 原生 | GitLab 身份验证和企业配置 |
| Bitbucket Cloud | 原生 | Secret Manager 中的 SSH 密钥 |
| Bitbucket Server | 原生 | Secret Manager 中的令牌和企业配置 |
| Bitbucket Data Center | 原生 | Secret Manager 中的令牌和企业配置 |

表 5.1 – SCM 平台触发器类型和访问配置

如上所述，这些平台可能具有原生触发器或 Webhook 触发器配置。仓库的身份验证和授权也可能取决于平台。当需要管理机密资源时，原生的 Google Secret Manager 集成可用于帮助管理机密。对于 GitHub，通过 GitHub 应用，还可以在仓库中的提交上更新构建状态。

一旦触发器配置为监听仓库分支或标签的特定正则表达式，匹配的提交将启动构建。

以下命令导出现有触发器：

```
$ gcloud builds triggers export \
    packt-cloudbuild-trigger \
    --destination export.yaml
```

以下是生成的 export.yaml 配置的输出：

```
autodetect: true
createTime: '2022-01-04T20:26:51.731858991Z'
description: Invokes a build every time code is pushed to any branch
github:
  name: packt-cloudbuild
  owner: kenthua
  push:
    branch: ^main$
id: 2aad99c0-2e3c-4a43-9b24-cbdaa12cbaf8
name: packt-cloudbuild-trigger
tags:
- cloudbuild-sample-branch-trigger
```

代码已提交到触发器正在监听的分支：

```
$ git push origin main
```

推送的示例输出如下。注意提交哈希为 5023f77：

```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 411 bytes | 411.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:kenthua/packt-cloudbuild.git
  17ab064..5023f77  main -> main
```

根据前面的示例触发器，一旦代码推送到 main 分支，构建将被触发并拉取源代码。使用 CLI 列出构建历史记录会输出以下内容：

```
$ gcloud builds list
```

以下是正在进行的构建的截断列表。您可以按需查询状态并跟踪构建：

```
ID: a50ec9b7-84a1-467e-b5ca-d89af472f783
CREATE_TIME: 2022-02-10T01:48:44+00:00
DURATION: 2M4S
SOURCE: -
IMAGES: -
STATUS: WORKING
```

以下命令检索指定构建的日志：

```
$ gcloud builds log a50ec9b7-84a1-467e-b5ca-d89af472f783
```

以下是构建的截断日志输出：

```
----------------------------------------------- REMOTE BUILD
OUTPUT -----------------------------------------------
starting build "a50ec9b7-84a1-467e-b5ca-d89af472f783"

FETCHSOURCE
hint: Using 'master' as the name for the initial branch. This
default branch name
hint: is subject to change. To configure the initial branch
name to use in all
hint: of your new repositories, which will suppress this
warning, call:
hint:
hint:   git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main',
 'trunk' and
hint: 'development'. The just-created branch can be renamed via
this command:
hint:
hint:   git branch -m <name>
Initialized empty Git repository in /workspace/.git/
From https://github.com/kenthua/packt-cloudbuild
* branch               5023f77a3913ac3c97ec0bacac7257fa29b60a5a  -> FETCH_HEAD
HEAD is now at 5023f77 update cloud build manifest;
BUILD
Starting Step #0
```

请注意，它获取了提交哈希 5023f77，该哈希已推送到 SCM 仓库。Cloud Build 本身不具有构建资源，因为它是由 SCM 事件触发的，但构建仍需要从仓库获取提交才能继续构建。

## 定义您自己的触发器

预构建的集成对于快速入门非常有帮助。但是，它们并不是与 Cloud Build 集成的唯一方式。您可以利用其他机制，例如手动或 Webhook 触发器，以便与当前尚未在 Cloud Build 中预构建的现有系统或平台集成。

## Webhook 触发器

我们将要运行的示例是一个 Webhook 触发器，我们将使用 GitLab 作为示例。如本章前面所述，原生与 GitLab 产品的集成正在进行中。

![](img/2fe9576240c146c49e3a21d5e7f6af00_95_0.png)

图 5.3 – Webhook 触发器创建示例

让我们分解 Webhook URL：

- 域名：`cloudbuild.googleapis.com`
- Google 项目 ID
- 触发器名称：`packt-cloudbuild-gitlab-trigger`
- 类型：webhook
- 密钥：生成的 API 密钥
- 密钥：指定 **New** 时生成的密钥或现有密钥，例如密码

在前面提到的示例 URL 中，我们有一个标准域名，由 Google 项目 ID 和触发器名称组成的上下文路径。保护 URL 的唯一元素是密钥和密钥。URL 预计使用 HTTP POST 方法，并可选择数据，可解析后在流水线中使用。

## 高级

## 替换变量

替换允许使用不同的变量值重用 cloudbuild.yaml 文件。使用 bash 字符串操作组合变量和绑定，以访问 Webhook 的 JSON 有效负载中的任意数据。了解更多

![](img/2fe9576240c146c49e3a21d5e7f6af00_96_0.png)

图 5.4 – 替换变量

替换变量可用于允许常见流水线文件具有灵活性，并可传入变量以使流水线更灵活。在此情况下，我们使用替换变量来提取要在流水线中使用的 GitLab 提交 ID。如前所述，对于 GitLab，响应包含仓库元数据，最重要的是提交 ID，可用于在流水线中检索特定提交（https://docs.gitlab.com/ee/user/project/integrations/webhook_events.html#push-events）。

在 Webhook 配置中或尝试访问没有预构建集成的仓库时，Cloud Build 无法访问 cloudbuild.yaml 配置的仓库。在这种情况下，触发器必须内联流水线配置。这可以通过 GCP 控制台 UI 编辑。如果使用 gcloud CLI，可以使用 --inline-config 开关添加，该开关引用文件系统中的 cloudbuild.yaml 配置。

## 配置

类型

Cloud Build 配置文件（yaml 或 json）

Dockerfile

Buildpacks

位置

仓库
选择上方的一个仓库

内联
编写内联 YAML

OPEN EDITOR

图 5.5 – 构建配置

在前一图中选择 **OPEN EDITOR** 将打开一个编辑器以指定流水线配置。以下屏幕截图是添加触发器时通过 GCP 控制台 UI 的内联 `cloudbuild.yaml` 的示例配置片段。

## 编辑内联配置

使用 YAML 语法编写构建配置文件。格式和注释不会被存储。

```
steps:
# first, setup SSH:
# 1- save the SSH key from Secret Manager to a file
# 2- add the host key to the known_hosts file
- name: gcr.io/cloud-builders/git
  args:
  - '-c'
  - |
    echo "$GITLAB_SSH_KEY" > /root/.ssh/id_rsa
    chmod 400 /root/.ssh/id_rsa
    ssh-keyscan gitlab.com > /root/.ssh/known_hosts
  entrypoint: bash
  secretEnv:
  - SSHKEY
  volumes:
  - name: ssh
    path: /root/.ssh
# second, clone the repository
- name: gcr.io/cloud-builders/git
  args:
  - clone
  - '-n'
  - 'git@gitlab.com:.../packt-cloudbuild'
  - .
  volumes:
  - name: ssh
    path: /root/.ssh
# third, checkout the specific commit that invoked the build
```

图 5.6 – 内联构建配置编辑器

![](img/2fe9576240c146c49e3a21d5e7f6af00_97_0.png)

![](img/2fe9576240c146c49e3a21d5e7f6af00_97_1.png)



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

### 摘要

Cloud Build 提供了多种与现有系统工具和 SCM 平台集成的方式，以参与自动化构建管道。Cloud Build 触发器提供了执行构建管道的能力。这可以通过 SCM 事件（例如提交到分支）、外部因素（例如成为另一个系统工具 larger automation pipeline 的一部分）或在指定时间来实现。

在本章中，我们展示了将 SSH 密钥存储在 Secret Manager 中以供构建管道使用的示例。在下一章中，我们将进一步探讨 Cloud Build 的安全最佳实践。
