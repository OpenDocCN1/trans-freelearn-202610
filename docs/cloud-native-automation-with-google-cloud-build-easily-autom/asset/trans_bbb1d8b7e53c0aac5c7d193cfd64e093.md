
您可以通过访问 console.cloud.google.com/logs 并在日志浏览器中输入查询，例如以下查询，来搜索与构建步骤 ID 关联的日志，并筛选出 textPayload 中包含构建步骤 ID 的日志：

```
resource.type = "build"
textPayload =~ "Deploy container to VM"
```

此外，在 Cloud Build 控制台中，您可以看到每个构建步骤现在都由其 id 字段引用：

| 步骤 | 时长 |
|---|---|
| **构建摘要**
3 个步骤 | 00:01:32 |
| 0: Docker 构建
build -t us-central1-docker.pkg.dev/agmsb-k8s/my-docker-repo/myima... | 00:00:01 |
| 1: Docker 推送
push us-central1-docker.pkg.dev/agmsb-k8s/my-docker-repo/myimage | 00:00:03 |
| 2: 部署容器到 VM
gcloud compute instances create-with-container my-vm-name --contai... | 00:01:20 |

图 4.4 – 为每个构建步骤定义 ID 字段时的构建步骤

使用 id 字段对于在需要以复杂模式运行的情况下定义构建步骤之间的关系也至关重要，例如按特定顺序运行多个构建步骤。我们将在“定义各个构建步骤之间的关系”部分稍后回顾此内容。

现在我们已经回顾了如何在 name 字段中提供构建步骤将运行的镜像，以及如何使用 id 字段更好地标识构建步骤，让我们回顾 args 字段。

## 定义构建步骤参数

这里，args 是您提供给云构建器以在执行构建步骤任务时使用的参数的字段。

请注意，args 可以使用多种样式用 YAML 编写：

- 流式样式 (https://yaml.org/spec/1.2.2/#flow-sequences)
- 块式样式 (https://yaml.org/spec/1.2.2/#block-sequences)

在我们的示例构建配置中，每个构建步骤的 args 字段均以流式样式定义：

```
args: ['build',
    '-t',
    'us-central1-docker.pkg.dev/${PROJECT_ID}/my-docker-repo/myimage',
    '.']
```

对于较小的参数集，流式样式提供了一种简单且可读的方式来编写 args。您还可以为提高可读性而在流式样式中为每个项目换行。

然而，对于更大、更复杂的参数集，使用块式样式可能更容易编写。以下是以块式样式编写的来自前一个示例的同一组 args 字段：

```
args:
  - 'build'
  - '-t'
  - 'us-central1-docker.pkg.dev/${PROJECT_ID}/my-docker-repo/myimage'
  - '.'
```

随意使用不同样式定义 args 的 cloudbuild.yaml 示例进行调整并重新运行构建。

> 注意
> 
> 您为单个构建步骤提供的 args 最大数量为 100，且单个 args 字段中的字符总数上限为 4,000。

无论您以何种方式编写参数，都有两种使用 args 启动构建器执行任务的方法：

- 如果云构建器提供入口点，则 args 中的值将用作该入口点的参数。
- 如果云构建器未提供入口点，则 args 中的第一个值将用作云构建器的入口点，其余值用作参数。

让我们查看我们第一个和第二个构建步骤的构建器镜像。如果我们查看源 Dockerfile (https://github.com/GoogleCloudPlatform/cloud-builders/tree/master/docker)，我们可以看到容器镜像已定义了 ENTRYPOINT。由于 Google Cloud Platform 的 GitHub 仓库中的内容可能会随时间变化，我们还分享了以下 Dockerfile 的内容：

```
FROM gcr.io/gcp-runtimes/ubuntu_20_0_4
ARG DOCKER_VERSION=5:19.03.9~3-0~ubuntu-focal
RUN apt-get -y update && \
    apt-get -y install \
    apt-transport-https \
    ca-certificates \
    curl \
    make \
    software-properties-common && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
    apt-key fingerprint 0EBFCD88 && \
    add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    edge" && \
    apt-get -y update && \
    apt-get -y install docker-ce=${DOCKER_VERSION} docker-ce-cli=${DOCKER_VERSION}
ENTRYPOINT ["/usr/bin/docker"]
```

在 Dockerfile 的最后一行，我们看到 ENTRYPOINT 定义为 ["/usr/bin/docker"]，这意味着云构建器将执行 Docker 二进制文件。由于情况如此，我们的第一个和第二个构建步骤提供参数以指示 Docker 构建容器镜像并将其推送到 Artifact Registry。

然而，情况并非如此，第三个构建步骤我们使用 gcr.io/google.com/cloudsdktool/cloud-sdk 构建器。在此构建步骤中，我们实际上使用 entrypoint 字段在我们的 cloudbuild.yaml 中定义入口点。这是因为用于构建构建器镜像的 Dockerfile 未定义入口点：

```
- name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
  entrypoint: 'gcloud'
```

您可以在配置文件中使用 entrypoint 字段出于两个主要原因：

- 如果构建器容器镜像未指定，则显式定义入口点
- 覆盖构建器容器镜像中定义的 ENTRYPOINT

在配置文件中定义入口点的最常见用例之一是能够将 shell 定义为入口点，然后将任意命令传递给 shell。

您可以按以下示例运行，其中使用块式 YAML 并将 '-c' 作为第一个参数传递，然后将多行字符串管道传输以运行其余的任意命令。

此示例采用 Python 3.9 的公共 Docker Hub 镜像并将其作为构建步骤运行，覆盖默认的 python3 CMD 以将入口点更改为 bash，同时在 shell 中运行一些命令：

```
$ cd .. && mkdir arbitrary-commands && cd arbitrary-commands
$ cat > cloudbuild.yaml <<EOF
steps:
  - name: 'python:3.9'
    id: 'Arbitrary commands in bash'
    entrypoint: 'bash'
    args:
      - '-c'
      - |
        pwd
        ls
        whoami
EOF
$ gcloud builds submit . --region=us-central1
```

构建日志中的输出应与 args 中定义的任意命令相匹配：

```
/workspace
cloudbuild.yaml
root
```

这在运行构建步骤时可以访问 shell 时增加了灵活性和可配置性。

现在我们已经回顾了 name、id、args 和 entrypoint 作为构建步骤运行的核心功能，让我们深入了解配置这些构建步骤的额外方法。

## 调整构建步骤的默认配置

除了定义构建步骤的容器镜像和参数之外，您可能还需要自定义其他配置，以便以特定方式执行该步骤中的任务。

这些可能是您希望在执行环境中设置的环境变量，使特定密钥可用于构建步骤，或在执行构建步骤的工作器上写入特定目录。

build step 段具有涵盖以下情况的附加字段：

- `env`
- `secretEnv`
- `timeout`
- `dir`

让我们从 env 字段开始 – 这提供了一种为执行任务的云构建器设置环境变量的方法。这提供了一种方式，让您提供多个可能希望传递到在构建步骤中运行任务的容器中的环境变量。

> **注意**
> 
> 有趣的是，`env` 和 `secretEnv` 不仅可以为单个构建步骤定义，还可以为构建中的所有构建步骤在全局级别定义。虽然我们将在 *配置构建范围规范* 部分回顾全局配置，但对于任何敏感信息，建议您在单个构建步骤级别配置这些值。

在我们示例的第三个构建步骤中，您可以看到我们定义 env 以包含两个不同的变量：

- `CLOUDSDK_COMPUTE_REGION=us-central1`
- `CLOUDSDK_COMPUTE_ZONE=us-central1-a`

鉴于我们的入口点正在运行 gcloud CLI，提前定义这些环境变量使 gcloud 能够使用这些属性已定义来引导。具体来说，已定义这些属性后，构建步骤能够在 us-central1-a 区域创建将运行容器镜像的 VM。

这代表了定义环境变量的典型用例 – 为构建步骤能够执行其任务做好环境准备。

如果我们想将凭据或密码投影到环境变量中，我们将改用 secretEnv。我们将在第 8 章《使用 Cloud Build 保护向 GKE 的软件交付》中更详细地回顾这一点。
