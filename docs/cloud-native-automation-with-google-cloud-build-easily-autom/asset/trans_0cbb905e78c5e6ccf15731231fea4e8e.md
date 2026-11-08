
```
$ git checkout test
$ git add .
$ git commit -m "Add Cloud Build YAML files."
$ git push --set-upstream origin HEAD
$ cd ../..
```

我们现在可以访问 `GitHub.com` 的网页 **用户界面 (UI)**，在其中我们将被提示在每个仓库中分别创建 PR，以合并添加到测试分支的更改。

打开每个仓库的 PR 后，我们将看到第一对触发器启动。它们应分别构建 Team A 和 Team B 的容器镜像。通过指定 `images` 字段，我们还确保它们将由 Cloud Build 推送，并自动生成构建来源，如 *第 6 章、环境安全管理* 中所述。

现在，我们可以合并每个仓库的 PR，这将启动我们的第二对触发器。这些构建各运行两个步骤：首先，通过为每个容器镜像插入摘要来准备 Kubernetes 清单，其次，将这些清单应用到 GKE 集群。

由于需要审批，您必须在构建启动前批准每个构建。批准构建并成功完成后，导航到侧边栏中 GKE 菜单中的 **工作负载**。我们现在可以看到 `team-a-app` 和 `team-b-app` 已成功部署。您也可以导航到侧边栏中 GKE 菜单中的 **服务和入口**，以访问面向您各自应用请求的负载均衡器端点。

## 从构建中启用工件的可验证信任

最后，在通过 Cloud Build 将软件从源代码安全地交付到 GKE 时，您希望确保能够验证在集群中运行的软件工件确实是在受信任的环境中构建的——在这种情况下，是您的 Cloud Build 工作器。

Cloud Build 提供自动构建来源 (`https://cloud.google.com/build/docs/securing-builds/view-build-provenance`)，它使 Cloud Build 能够为其构建的每个容器镜像生成签名元数据，证明该工件源自 Cloud Build 中的构建，而不是通过 **场外 (OOB)** 的恶意行为者。

## 使用构建来源构建镜像

最后，在通过 Cloud Build 将软件从源代码安全地交付到 GKE 时，您希望确保能够验证在集群中运行的软件工件确实是在受信任的环境中构建的——在这种情况下，是您的 Cloud Build 工作器。您可以在此处看到此过程的说明：

![](img/2fe9576240c146c49e3a21d5e7f6af00_176_0.png)

图 8.12 – 构建来源提供证明工件来源的加密签名元数据

我们可以通过运行以下命令查看此元数据：

```
$ gcloud artifacts docker images describe \
$REGION-docker.pkg.dev/$PROJECT_ID/$REPOSITORY_A/team-a-app:v1 \
  --show-provenance
```

现在，我们将看看如何在部署过程中使用此元数据。

## 利用 Binary Authorization 进行准入控制

有了从 Cloud Build 角度生成的此元数据，我们现在可以通过启用 Binary Authorization 并创建策略来保护目标环境，这些策略将拒绝运行没有由 Cloud Build 生成的构建来源的容器镜像。

这使您能够生成对 Cloud Build 的可验证信任，并在 GKE 或 Cloud Run 中在运行时验证该信任。该过程在以下图表中说明：

![](img/2fe9576240c146c49e3a21d5e7f6af00_176_1.png)

图 8.13 – 利用证明自动将工件准入或拒绝到生产 GKE 集群

在此示例中，我们将通过导入策略来启用 Binary Authorization，该策略要求来自 built-by-cloud-build 证明者的证明，该证明者在运行生成构建来源的构建时会自动为您生成。

运行以下命令创建所需的策略：

```
$ cat << EOF > infra/policy.yaml
globalPolicyEvaluationMode: ENABLE
defaultAdmissionRule:
    evaluationMode: REQUIRE_ATTESTATION
    enforcementMode: ENFORCED_BLOCK_AND_AUDIT_LOG
    requireAttestationsBy:
    - projects/${PROJECT_ID}/attestors/${ATTESTOR_ID}
EOF
```

运行以下命令导入所需的策略：

```
$ gcloud container binauthz policy import infra/policy.yaml
```

启用此策略后，我们现在可确保只有由 Cloud Build 构建并具有构建来源的镜像才会在部署时被准入到我们的集群。

让我们通过尝试部署一个 OOB 容器镜像来测试这一点，该镜像在 Cloud Shell 中本地构建，而不是通过 Cloud Build 中的工作器构建。

我们将通过将一个未由 Cloud Build 构建的现有示例容器拉取到我们的项目中来实现此目的。在 Cloud Shell 中配置 Docker 以从 Artifact Registry 拉取镜像，并像这样从 google-samples 拉取 hello-app 镜像：

```
$ gcloud auth configure-docker $REGION-docker.pkg.dev

$ docker pull us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0
```

然后，将容器镜像标记为 out-of-band-app 并推送到 Artifact Registry 中 Team A 的仓库，如下所示：

```
$ docker tag us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0 $REGION-docker.pkg.dev/$PROJECT_ID/$REPOSITORY_A/out-of-band-app:tag1
```

```
$ docker push $REGION-docker.pkg.dev/$PROJECT_ID/$REPOSITORY_A/out-of-band-app:tag1
```

现在，让我们使用 Cloud Build 中的构建创建 Kubernetes 清单并将其应用到我们的集群，如下所示：

```
$ cd bin
$ mkdir out-of-band && cd out-of-band

$ DIGEST=$(gcloud artifacts docker images describe $REGION-docker.pkg.dev/$PROJECT_ID/$REPOSITORY_A/out-of-band-app:tag1 --format="json" | jq '.image_summary."digest"' | cut -d "\"" -f 2)

$ cat << EOF > k8s.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: out-of-band-app
spec:
  selector:
    matchLabels:
      app: out-of-band
  replicas: 1
  template:
    metadata:
      labels:
        app: out-of-band
    spec:
      containers:
      - name: app
        image: $REGION-docker.pkg.dev/$PROJECT_ID/$REPOSITORY_A/out-of-band@$DIGEST
        imagePullPolicy: Always
EOF

$ cat << EOF > cloudbuild.yaml
steps:
- name: gcr.io/google.com/cloudsdktool/cloud-sdk
  id: Get kubeconfig and apply manifest of out of band app
  entrypoint: sh
  args:
    - '-c'
    - |
        gcloud container clusters get-credentials $CLUSTER_NAME --region $REGION --project $PROJECT_ID
        kubectl apply -f k8s.yaml -n $NAMESPACE_01
  serviceAccount: 'projects/${PROJECT_ID}/serviceAccounts/${GCP_SA_NAME_01}@${PROJECT_ID}.iam.gserviceaccount.com'
  options:
    workerPool: 'projects/$PROJECT_NUM/locations/$REGION/workerPools/$PRIVATE_POOL_NAME'
    logging: CLOUD_LOGGING_ONLY
EOF
$ gcloud builds submit . --config=cloudbuild.yaml
```

当在您的 GKE 集群中导航到 out-of-band-app 时，您应该会看到类似于此处的错误：

![](img/2fe9576240c146c49e3a21d5e7f6af00_179_0.png)

图 8.14 – 显示 out-of-band-app 没有所需证明的错误

Binary Authorization 具有灵活性和可扩展性；您还可以基于自定义证明和元数据创建策略，或利用与安全扫描的集成，并基于与工件内容相关联的漏洞级别创建策略。

使用 Binary Authorization 创建符合您要求和组织策略的策略是最佳实践——您可以在此处找到更多策略和用例示例：https://cloud.google.com/container-analysis/docs/ods-cloudbuild#build_your_container.

## 摘要

在本章中，我们回顾了如何为共享 GKE 集群的多个团队在 Cloud Build 中设置安全的软件交付管道。我们使用私有连接和具有最小权限的每个构建 GCP 服务账户设置了安全的基础设施。我们引入了带有人机回路审查的 Cloud Build 审批，同时确保我们只将受信任且经过验证的容器镜像准入到我们的 GKE 集群。

这些功能和实践使您能够开始实施更安全的软件交付管道到 GKE，这是 Google Cloud 中最受欢迎的运行时之一。

接下来，我们将把重点转移到使用 Cloud Build 通过更以开发者为中心、无服务器的平台——Cloud Run 来自动化工作流。
