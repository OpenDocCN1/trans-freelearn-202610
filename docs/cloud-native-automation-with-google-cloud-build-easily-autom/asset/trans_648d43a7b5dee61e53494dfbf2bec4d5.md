
```
$ BUILD_PROJECT=build-project
$ ARTIFACT_REGISTRY_IMAGE_REPO=image-repo
$ GCS_BUCKET=packt-cloudbuild-dev-bucket
$ GIT_REPO=packt-cloudbuild
$ GIT_OWNER=user
```

> **注意**
> 在以下示例中，dev-project 必须禁用 iam.disableCrossProjectServiceAccountUsage 组织策略。组织策略在组织级别配置，但可以根据约束类型应用于文件夹和单个项目。约束可能有所不同，但决定了可以执行的操作，超出 IAM 权限所定义的范围，后者决定谁可以执行什么。

以下图表显示了此示例将创建的内容，以及构建与用户指定的服务帐户之间的关系：

![](img/2fe9576240c146c49e3a21d5e7f6af00_206_0.png)

图 10.1 – 在单独项目中的用户指定服务帐户

上述图表显示了两个项目之间的关系以及每个项目相关的权限，以便在另一个项目中利用用户指定的服务帐户。

创建服务帐户以用作 Cloud Build 管道的用户指定服务帐户：

```
$ gcloud iam service-accounts create ${SA_NAME} \
  --description="Build Dev Service Account" \
  --project ${DEV_PROJECT}
```

服务帐户需要服务帐户用户权限，以便 Cloud Build 服务代理可以在将使用 Cloud Build 服务的项目中使用它：

```
$ gcloud projects add-iam-policy-binding ${DEV_PROJECT} \
  --member=serviceAccount:${SA_NAME}@${DEV_PROJECT}.iam.gserviceaccount.com \
  --role=roles/iam.serviceAccountUser
```

将利用 Cloud Build 服务的 Google Cloud 项目的项目编号是必需的，以便可以执行服务帐户模拟。我们设置 Cloud Build 服务代理的权限，使其能够创建服务帐户令牌，以代表托管在我们开发项目上的服务帐户执行命令：

```
$ export BUILD_PROJECT_NUMBER=$(gcloud projects list \
  --filter="${BUILD_PROJECT}" \
  --format="value(PROJECT_NUMBER)"
)
$ gcloud projects add-iam-policy-binding ${DEV_PROJECT} \
  --member="serviceAccount:service-${BUILD_PROJECT_NUMBER}@gcp-sa-cloudbuild.iam.gserviceaccount.com" \
  --role="roles/iam.serviceAccountTokenCreator"
```

尽管服务帐户位于开发项目上，但我们授予服务帐户向将使用 Cloud Build 服务的构建项目写入日志的权限：

```
$ gcloud projects add-iam-policy-binding ${BUILD_PROJECT} \
  --member=serviceAccount:${SA_NAME}@${DEV_PROJECT}.iam.gserviceaccount.com \
  --role=roles/logging.logWriter
```

由于我们使用的是用户指定的服务帐户，因此需要使用我们的 `cloudbuild.yaml` 配置进行配置以仅记录日志。以下代码显示了 `cloudbuild.yaml` 文件的一部分。本章后面我们将介绍一个场景，其中日志存储在 **Google Cloud Storage (GCS)** 存储桶中：

```
...
options:
  logging: CLOUD_LOGGING_ONLY
...
```

触发器是在构建项目上创建的，但用户指定的服务帐户是我们开发项目中创建的那个：

```
$ gcloud beta builds triggers create github \
  --repo-name=${GIT_REPO} \
  --repo-owner=${GIT_OWNER} \
  --branch-pattern=^main$ \
  --name=pipeline-trigger \
  --build-config=cloudbuild.yaml \
  --project=${BUILD_PROJECT} \
  --service-account=projects/${DEV_PROJECT}/serviceAccounts/${SA_NAME}@${DEV_PROJECT}.iam.gserviceaccount.com
```

在此示例中，我们将容器镜像推送到 Artifact Registry 的镜像存储库，该存储库也托管在同一个 Cloud Build 项目上。写入镜像存储库需要权限：

```
$ gcloud artifacts repositories add-iam-policy-binding ${ARTIFACT_REGISTRY_IMAGE_REPO} \
  --location=us-central1 \
  --member=serviceAccount:${SA_NAME}@${DEV_PROJECT}.iam.gserviceaccount.com \
  --role=roles/artifactregistry.writer \
  --project ${BUILD_PROJECT}
```

这是前述示例中触发构建的构建详细信息的示例输出。构建执行的 Project ID 在 `build-project` 中，服务帐户来自 `dev-project`。我们可以通过描述构建来获取此信息：

```
$ gcloud builds describe 2ddd7e2e-1b75-49e2-b3a9-4781ad509f18 --project build-project
```

以下是运行前述命令的输出，其中标识了正在使用的用户指定服务帐户：

```
id: 2ddd7e2e-1b75-49e2-b3a9-4781ad509f18
...
projectId: build-project
...
serviceAccount: projects/dev-project/serviceAccounts/build-dev@dev-project.iam.gserviceaccount.com
```

由于构建在构建项目中运行，并且在存储库有提交时会自动触发，很可能出现开发人员或特定团队成员仅有权访问特定项目的情况——在本例中为 dev-project。在前面的示例中，我们将日志写入 build-project，但我们可以设置一个接收器并将日志从 build-project 转发到 dev-project（https://cloud.google.com/logging/docs/export/configure_export_v2#supported-destinations）。

另一种方法是将日志写入由 dev-project 管理和托管的 GCS 存储桶。构建日志数据由 build-project 创建并存储在 dev-project 中以供进一步分析：

![](img/2fe9576240c146c49e3a21d5e7f6af00_209_0.png)

图 10.2 – 记录到另一个项目中的 GCS 存储桶

上述图表显示了构建如何写入 GCS 存储桶，以及用户指定服务帐户在存储桶级别所需的特定权限。

首先，让我们在 dev-project 中创建存储桶：

```bash
$ gsutil mb -p ${DEV_PROJECT} \
    gs://${GCS_BUCKET}
```

为了允许 Cloud Build 服务使用我们的用户指定服务帐户写入 GCS 存储桶，我们需要为我们的服务帐户授予必要的权限：

```bash
$ gsutil iam ch \
    serviceAccount:${SA_NAME}@${DEV_PROJECT}.iam.gserviceaccount.com:roles/storage.admin \
    gs://${GCS_BUCKET}
```

我们还需要在 cloudbuild.yaml 文件中告诉 Cloud Build 我们希望将日志存储在 GCS 存储桶中：

```yaml
...
logsBucket: "gs://${REPLACE_WITH_YOUR_GCS_BUCKET_NAME}"
options:
  logging: GCS_ONLY
...
```

成功运行构建后，我们可以看到日志现在可在我们的存储桶中使用。我们可以在示例输出中看到这一点：

```bash
$ gsutil ls gs://${GCS_BUCKET}
gs://packt-cloudbuild-dev-bucket/log-44045da7-3a03-4f9d-8aa0-f9599e3efd47.txt
```

> **注意**
> 如果未正确设置 Cloud Build 记录到 GCS 存储桶的权限，构建将无法正常执行。在这种情况下，您可能会收到来自 Cloud Build 服务的以下错误：
> 
> ```
> generic::invalid_argument: invalid bucket "packt-cloudbuild-dev-bucket"; builder service account does not have access to the bucket
> ```

利用不同的项目时，可能需要在不同的项目中存储日志数据。本节中的示例展示了如何实现这一点。

## 进一步保护构建触发器

在 *第 6 章，管理环境安全* 中，我们讨论了 **虚拟私有云服务控制** (VPC-SC) 作为一种机制，有助于防止 GCP 服务（包括 Cloud Build）的数据外泄。Cloud Build 中提供的另一种机制可防止来自外部源的触发器以保护构建环境，即通过组织策略。通过设置此约束，我们可以指定哪些域可以执行功能或拒绝特定域。

到目前为止，我们的示例都是通过 github.com 触发的。现在，我们将拒绝来自 github.com 的触发器。为此定义，我们必须设置约束并指定权限：

```
name: projects/**REDACTED_project_number**/policies/cloudbuild.allowedIntegrations
spec:
  inheritFromParent: false
  rules:
    - values:
        deniedValues:
          github.com
```

我们将拒绝源自 github.com 的触发器。请注意我们将强制执行的约束——即 cloudbuild.allowedIntegrations。一旦强制执行，前一节中的触发器将不再能够执行并创建构建。以下 Cloud Logging 输出片段显示了构建无法触发和执行的原因：

```
status: {
code: 9
message: "Constraint \"constraints/cloudbuild.allowedIntegrations\" violated for \"**REDACTED_project_id**\" attempting to process an event from \"github.com\""
}
```

这提供了另一种帮助保护构建的功能。

## 通知

当您最初设置构建或手动触发构建时，您可能会在逐步测试构建配置时与构建交互并监视构建。您会知道构建何时失败或成功。然而，当构建基于外部事件（如对 **源代码管理 (SCM)** 仓库的提交）自动触发时，这并不是典型的使用模式。组织有不同类型的机制来通知开发人员和运营团队构建的状态。这些机制可以从发布有关构建消息的系统各不相同
