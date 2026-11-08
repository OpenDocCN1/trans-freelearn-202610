
```
--network=$CLUSTER_VPC_NAME \
--export-custom-routes \
--no-export-subnet-routes-with-public-ip
```

让我们回顾一下 GKE 集群的当前架构，现在我们已经创建了用于 GKE 私有集群的 VPC 以及集群本身。你可以在这里查看：

![](img/2fe9576240c146c49e3a21d5e7f6af00_152_0.png)

图 8.3 – 已创建的私有 GKE 集群的高级架构

总结一下，你所创建的关键方面包括以下内容：

- 确保对 GKE 控制平面的访问仅通过私有连接可用。
- 节点无法从公共镜像仓库拉取容器镜像。
- 节点通过私有内部 IP 地址拉取容器镜像。

完成此项后，让我们继续使用 Cloud Build 私有池创建安全的构建和部署基础设施。

## 保护构建和部署基础设施

在 Cloud Build 中保护软件交付流程时，重要的是从运行构建本身的底层基础设施开始。如果遭到入侵，攻击者可以访问敏感信息，例如你的源代码、构建可能访问的机密信息，以及构建所交互的部署目标。

*本章示例中的底层基础设施*具体指以下内容：

- 执行 Cloud Build 构建的私有池工作器
- VPC 网络，将工作器连接到 Artifact Registry 和 GKE 等系统
- 执行构建步骤的最小化或受管容器镜像

我们将首先为示例创建一个私有池。

## 使用安全最佳实践创建私有池

在第 2 章《配置 Cloud Build 工作器》中已介绍过，私有池是 Cloud Build 工作器的特定模式，其功能与默认池不同，包括 VPC 连接性和更大的机器类型可用性，同时仍完全托管在 Google 拥有的环境中。

在本示例中，我们将使用私有池运行四个构建，具体如下：

- 团队 A，构建 1—为 Python 应用构建容器镜像。
- 团队 A，构建 2—将容器镜像部署到 GKE。
- 团队 B，构建 1—为 Python 应用构建容器镜像。
- 团队 B，构建 2—将容器镜像部署到 GKE。

这模拟了这样一种场景：两个不同的团队都将工作负载部署到同一个 Kubernetes 集群，该集群通过使用 Kubernetes 命名空间隔离集群中的应用程序而配置为简单的多租户，如下图所示：

![](img/2fe9576240c146c49e3a21d5e7f6af00_153_0.png)

图 8.4 – 本章软件交付工作流的高级架构

在运行构建之前，我们必须创建私有池并将其设置好以与我们的 GKE 集群通信，同时牢记以下目标：

- 创建安全的 worker 在私有池中运行构建。
- 安全地访问 GKE 控制平面。
- 通过允许列表限制对 GKE 控制平面的访问。
- 保护 Cloud Build 和 GKE 通信的网络边界。

作为提醒，如果你在处理上一节后丢失了上下文，请运行以下命令导航到正确的目录并加载适当的环境变量。

将此处显示的 <add-project-id> 替换为你正在使用的项目：

```
$ gcloud config set project <add-project-id>
```

按如下方式导航到你正在工作的目录：

```
$ cd ~/cloudbuild-gke/08
```

按如下方式加载所需的环境变量：

```
$ source bin/variables.sh
```

牢记第一个目标，你将创建一个 Cloud Build 私有池，并确保我们可用的安全配置得到利用。但在创建私有池之前，我们必须创建以下几个额外的网络资源：

- 私有池工作器的私有 IP 范围
- VPC 对等，以便将我们之前创建的 VPC 与 Service Networking API (https://cloud.google.com/service-infrastructure/docs/enabling-private-services-access) 进行对等，使其能够连接到私有池工作器

要为私有池创建 IP 范围，请运行以下命令：

```
$ gcloud compute addresses create $PRIVATE_POOL_IP_RANGE_NAME \
  --global \
  --addresses=$PRIVATE_POOL_IP_RANGE \
  --purpose=VPC_PEERING \
  --prefix-length=$PRIVATE_POOL_IP_RANGE_SIZE \
  --network=$PRIVATE_POOL_VPC_NAME
```

要与我们之前创建的 VPC 创建对等连接，请运行以下命令：

```
$ gcloud services vpc-peerings connect \
  --service=servicenetworking.googleapis.com \
  --ranges=$PRIVATE_POOL_IP_RANGE_NAME \
  --network=$PRIVATE_POOL_VPC_NAME \
  --project=$PROJECT_ID
$ gcloud compute networks peerings update servicenetworking-googleapis-com \
  --network=$PRIVATE_POOL_VPC_NAME \
  --export-custom-routes \
  --no-export-subnet-routes-with-public-ip
```

我们使用两个值得注意的配置创建了对等连接：--export-custom-routes 和 --no-export-subnet-routes-with-public-ip。这些配置分别允许我们在私有池和私有 GKE 控制平面之间导出路由，同时防止任何带有公共 IP 地址的路由被导出。

在对等连接正确设置后，我们将通过以下方式完成网络配置，发布我们的私有池和 GKE 控制平面 IP 地址：

```
$ gcloud compute routers update-bgp-peer ${PRIVATE_POOL_ROUTER} \
  --peer-name=$PRIVATE_POOL_ROUTER_PEER_0 \
  --region=${REGION} \
  --advertisement-mode=CUSTOM \
  --set-advertisement-ranges=${PRIVATE_POOL_IP_RANGE}/${PRIVATE_POOL_IP_RANGE_SIZE}

$ gcloud compute routers update-bgp-peer ${PRIVATE_POOL_ROUTER} \
  --peer-name=$PRIVATE_POOL_ROUTER_PEER_1 \
  --region=${REGION} \
  --advertisement-mode=CUSTOM \
  --set-advertisement-ranges=${PRIVATE_POOL_IP_RANGE}/${PRIVATE_POOL_IP_RANGE_SIZE}

$ gcloud compute routers update-bgp-peer ${CLUSTER_ROUTER} \
  --peer-name=${CLUSTER_ROUTER_PEER_0} \
  --region=${REGION} \
  --advertisement-mode=CUSTOM \
  --set-advertisement-ranges=$CLUSTER_CONTROL_PLANE_CIDR

$ gcloud compute routers update-bgp-peer ${CLUSTER_ROUTER} \
    --peer-name=${CLUSTER_ROUTER_PEER_1} \
    --region=${REGION} \
    --advertisement-mode=CUSTOM \
    --set-advertisement-ranges=$CLUSTER_CONTROL_PLANE_CIDR
```

现在，我们可以继续创建私有池。要创建你的私有池，请先加载所需的环境变量，并在 infra/ 目录下创建名为 private-pool.yaml 的私有池配置文件，如下所示：

```
$ cat > infra/private-pool.yaml <<EOF
privatePoolV1Config:
  networkConfig:
    # egressOption: NO_PUBLIC_EGRESS
    peeredNetwork: projects/$PROJECT_ID/global/
networks/$PRIVATE_POOL_VPC_NAME
  workerConfig:
    machineType: e2-standard-2
    diskSizeGb: 100
EOF
```

在此配置文件中，与安全相关的最重要部分位于 networkConfig 部分。私有池由于完全由 Google 管理，其运行方式类似于 GKE 控制平面，即通过 VPC 对等获得对你 VPC 中资源的访问权限。你可以在下图中看到对此的说明：

![](img/2fe9576240c146c49e3a21d5e7f6af00_157_0.png)

图 8.5 – 你 VPC 与私有池运行的 VPC 之间的网络架构

让我们回顾一下 `egressOption` 和 `peeredNetwork` 字段以及它们对我们私有池的含义，如下所示：

- 将 `egressOption` 设置为 `NO_PUBLIC_EGRESS` 会从私有池中的工作器中移除公共 IP 地址。
- 将 `peeredNetwork` 设置为与我们的 GKE 集群 VPC 分开的独立 VPC 是必需的。这是因为缺乏传递式对等功能。

如果我们从私有池中移除公共 IP 地址，这意味着我们还可以拒绝工作器基础设施访问公共互联网以及可能具有恶意或易受攻击的工件。这有助于减少软件交付管道可能被入侵的攻击面。

> **注意**
> 
> 对于本示例，我们将在私有池配置中省略 `NO_PUBLIC_EGRESS` 选项，因为我们将连接到的源代码仓库将是 `GitHub.com`。这是为了简化并避免任何许可需求。
> 
> 在安全的软件交付设置中，你理想情况下会将此参数与私有 **源代码管理 (SCM)** 系统（如 GitHub Enterprise）一起使用，私有池可以通过私有连接访问该系统。

