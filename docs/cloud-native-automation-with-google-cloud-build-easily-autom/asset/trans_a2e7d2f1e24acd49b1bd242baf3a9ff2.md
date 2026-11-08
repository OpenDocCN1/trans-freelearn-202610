
现在，使用上一条命令中创建的配置文件创建一个私有池，如下所示：

```bash
$ gcloud builds worker-pools create $PRIVATE_POOL_NAME
--config-from-file infra/private-pool.yaml --region $REGION
```

然而，对许多人来说，仅使构建系统与 GKE 控制平面之间的通信保持私有是不够的。如果攻击者能够在具有该 VPN 访问权限的 VPC 中创建资源，他们仍可能与你的私有 GKE 控制平面进行交互。

让我们回顾一下如何从网络边界内的其他资源保护控制平面。

## 保护对私有 GKE 控制平面的访问

作为提醒，如果你在处理上一节后丢失了上下文，请运行以下命令以导航到正确的目录并加载正确的环境变量。

将此处显示的 <add-project-id> 替换为你正在使用的项目：

```bash
$ gcloud config set project <add-project-id>
```

按如下方式导航到你正在工作的目录：

```bash
$ cd ~/cloudbuild-gke/08
```

按如下方式加载所需环境变量：

```bash
$ source bin/variables.sh
```

建议使用 GKE 中的 GKE 控制平面访问功能的授权网络配置允许列表（https://cloud.google.com/kubernetes-engine/docs/how-to/authorized-networks），如下图所示：

![](img/2fe9576240c146c49e3a21d5e7f6af00_159_0.png)

图 8.6 – 确保用户来自已知且受信任的网络身份

这将使你能够通过拒绝不在特定允许列表上的网络身份发来的任何流量来限制对控制平面的访问。这也意味着你必须向 GKE 提供应被授权调用控制平面的网络范围。你可以通过运行以下命令来实现：

```bash
$ gcloud container clusters update $CLUSTER_NAME \
  --enable-master-authorized-networks \
  --region=$REGION \
  --master-authorized-networks=$PRIVATE_POOL_IP_RANGE/$PRIVATE_POOL_IP_RANGE_SIZE
```

这现在意味着你只能利用此私有池中的 Cloud Build 工作器与你的 GKE 控制平面交互。如果你想使用 Compute Engine 虚拟机（VM）创建一个私有工作站或堡垒主机，你还需要将其私有 IP 地址添加到允许列表中，以便使用 kubectl 与 GKE 控制平面交互。

在工作器、集群和网络配置设置完成后，是时候开始评估我们的构建在此环境中运行的权限了。

## 将 POLP 应用于构建

当构建运行并与其他 Google Cloud 资源交互时，默认情况下它们会使用 Cloud Build 服务账号作为其身份，并为其分配权限。

然而，这并不能使构建或用户应用 POLP；如果有一个 GCP 服务账号被多个执行不同任务或与不同资源交互的构建使用，那么使用该服务账号的所有构建都会过度授权。

Cloud Build 支持按构建或按触发器的服务账号；这使你能够有意地并根据 POLP 创建每个服务账号。

这一原则确保每个构建拥有的权限不超过其成功执行所需的最小权限；这是通过特定用途的 GCP 服务账号实现的。

## 创建特定于构建的 IAM 服务账号

让我们首先创建两个 GCP 服务账号，每个模拟的团队一个。

作为提醒，如果你在处理上一节后丢失了上下文，请运行以下命令以导航到正确的目录并加载正确的环境变量。

将此处显示的 <add-project-id> 替换为你正在使用的项目：

```bash
$ gcloud config set project <add-project-id>
```

按如下方式导航到你正在工作的目录：

```bash
$ cd ~/cloudbuild-gke/08
```

按如下方式加载所需环境变量：

```bash
$ source bin/variables.sh
```

现在，创建第一个服务账号。它将与团队 A 相关联，并将与负责构建容器镜像并将其推送到 Artifact Registry 中团队 A 仓库的构建相关联。以下是你需要执行的代码：

```bash
$ gcloud iam service-accounts create $GCP_SA_NAME_01 \
  --display-name=$GCP_SA_NAME_01
```

该服务账号所需的权限是能够向 Artifact Registry 中的特定仓库写入容器镜像。这符合 POLP，因为我们不想授予对 Artifact Registry 中所有仓库的访问权限，特别是如果我们有其他团队构建和存储与团队 A 无关的镜像。以下截图中展示了其表示：

![](img/2fe9576240c146c49e3a21d5e7f6af00_161_0.png)

图 8.7 – 仅对特定 Artifact Registry 仓库具有权限的按构建服务账号

让我们在 Artifact Registry 中创建一个仓库，并附带一个角色绑定，该绑定将使服务账号获得对 Artifact Registry 中团队 A 的单个命名仓库的权限，如下所示：

```bash
$ gcloud artifacts repositories create $REPOSITORY_A \
  --repository-format=docker \
  --location=$REGION

$ gcloud artifacts repositories add-iam-policy-binding team-a-repository\
  --location us-west1 --member=$GCP_SA_NAME_01 --role=roles/artifactregistry.writer
```

现在让我们重复相同的过程，只是针对团队 B 用于构建和推送容器镜像到其在 Artifact Registry 中自己的仓库的特定服务账号，如下所示：

```bash
$ gcloud iam service-accounts create $GCP_SA_NAME_02 \
  --display-name=$GCP_SA_NAME_02

$ gcloud artifacts repositories create $REPOSITORY_B \
  --repository-format=docker \
  --location=$REGION

$ gcloud artifacts repositories add-iam-policy-binding team-b-repository\
  --location us-west1 --member=$GCP_SA_NAME_02 --role=roles/artifactregistry.writer
```

我们还必须授予服务账号日志访问权限，以便将构建日志写入 Cloud Logging，使用以下两个命令：

```bash
$ gcloud projects add-iam-policy-binding ${PROJECT_ID} \
--member="serviceAccount:${GCP_SA_NAME_01}@${PROJECT_ID}.iam.gserviceaccount.com" \
--role=roles/logging.logWriter
$ gcloud projects add-iam-policy-binding ${PROJECT_ID} \
--member="serviceAccount:${GCP_SA_NAME_02}@${PROJECT_ID}.iam.gserviceaccount.com" \
--role=roles/logging.logWriter
```

完成此操作后，我们将继续配置 GKE 集群侧服务账号的最小权限。与我们在 Artifact Registry 中所做的一样，我们将对集群访问应用 POLP。

## 构建服务账号的自定义 IAM 角色

由于团队 A 和团队 B 在你的 GKE 集群中将拥有自己的命名空间，我们只希望他们的服务账号对这些命名空间具有权限，而不是整个 GKE 集群，如下图所示：

![](img/2fe9576240c146c49e3a21d5e7f6af00_162_0.png)

为此，我们必须在你的 GKE 集群中创建命名空间，为团队 A 和团队 B 的构建服务账号创建自定义 IAM 角色，并利用 Kubernetes 基于角色的访问控制（RBAC）以在 GKE 集群内拥有最小权限。

为了区分名称非常相似的资源，请参阅下图：

![](img/2fe9576240c146c49e3a21d5e7f6af00_163_0.png)

图 8.9 – 用于获取凭据的 GCP 服务账号；用于其他所有事物的 Kubernetes 服务账号

首先创建每个服务账号将用于获取 GKE 集群凭据的自定义角色，如下所示：

```bash
$ cat << EOF > infra/minimal-gke-role.yaml
title: minimal-gke
description: Gets credentials only, RBAC for authz.
stage: GA
includedPermissions:
- container.apiServices.get
- container.apiServices.list
- container.clusters.get
- container.clusters.getCredentials
EOF
$ gcloud iam roles create minimal-gke-role --project=$PROJECT_ID \
  --file=infra/minimal-gke-role.yaml
```
