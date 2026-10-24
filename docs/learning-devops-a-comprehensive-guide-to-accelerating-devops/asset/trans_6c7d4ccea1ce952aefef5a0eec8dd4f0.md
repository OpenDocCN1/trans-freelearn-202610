

## 4. 将 Docker 镜像推送到 Docker Hub

为镜像打上标签后，最后一步是将打好标签的镜像推送到 Docker Hub。

为此，我们将执行以下命令：

```
docker push docker.io/<dockerhub login>/demobook:v1
```

以下截图显示了上述命令的执行情况：

![](img/d3344b39c888aab0b832625465f837c9_302_0.png)

从这次执行中我们可以看到，镜像已上传到 Docker Hub。

要在 Docker Hub 中查看推送的镜像，我们连接到 Docker Hub 网页门户 https://hub.docker.com/，可以看到该镜像已存在，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_302_1.png)

默认情况下，推送到 Docker Hub 的镜像处于公开模式——所有人都可以在资源管理器中查看并使用它。

我们可以在 Docker Hub 搜索引擎中访问此镜像，如下图所示：

图 9.19 – 在 Docker Hub 中查找镜像

要将此镜像设为私有——即只有经过身份验证的你才能使用它——你必须进入镜像的 Settings 选项卡，然后单击 Make private 按钮，如下图所示：

图 9.20 – 将 Docker 镜像设为私有

在本节中，我们介绍了通过命令行登录 Docker Hub 的步骤和 Docker 命令，然后介绍了用于将 Docker 镜像上传到 Docker Hub 的 tag 和 push 命令。

在下一节中，我们将通过一个 ACR 实例示例，了解如何将 Docker 镜像推送到私有 Docker 注册表。

## 将 Docker 镜像推送到私有注册表 (ACR)

在上一节中，我们学习了如何将 Docker 镜像推送到 Docker Hub（这是一个公共注册表）。现在，我们将学习如何将 Docker 镜像推送到私有注册表。

有许多本地或云解决方案支持 Docker 私有注册表。以下是这些解决方案的列表：

- Docker registry server: `https://docs.docker.com/registry/deploying/`
- Artifactory from JFrog: `https://www.jfrog.com/confluence/display/JFROG/Docker+Registry`
- Amazon Elastic Container Registry (ECR): `https://aws.amazon.com/ecr/`
- Google Container Registry (GCR): `https://cloud.google.com/container-registry`
- ACR: `https://azure.microsoft.com/en-us/services/container-registry/`

在本节中，我们将研究其中一种解决方案——ACR。

要将 Docker 镜像推送到 ACR，我们将执行以下步骤：

1. 在推送 Docker 镜像之前，我们将使用以下方法之一创建 ACR 资源：

   - Azure CLI (`https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-azure-cli`)。创建资源组和 ACR 资源的 az cli 脚本如下所示：

   ```
   az group create --name RG-ACR --location eastus
   az acr create --resource-group RG-ACR --name acrdemo --sku Basic
   ```

   - PowerShell (`https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-powershell`)。创建资源组和 ACR 资源的 PowerShell 脚本如下所示：

   ```
   New-AzResourceGroup -Name RG-ACR -Location EastUS
   $registry = New-AzContainerRegistry -ResourceGroupName "RG-ARC" -Name "acrdemo" -EnableAdminUser -Sku Basic
   ```

   - Azure 门户 (`https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal`)

   在下一步中，我们将使用名为 demobookacr 的 ACR 资源。

2. 然后，我们将通过运行以下 az cli 命令连接到 Azure 帐户：

   ```
   az login
   ```

3. 我们将使用以下 az acr login 命令连接到已创建的 ACR 资源（步骤 1），并将 --name 参数作为步骤 1 中创建的 ACR 资源的名称，如下所示：

   ```
   az acr login --name demobookacr
   ```

   此命令将在后台使用 docker login 命令连接到 Docker 注册表。

4. 要将 Docker 镜像推送到此 ACR 资源，我们将执行几个命令。

   第一个命令是为本地镜像创建标签，如下所示：

   ```
   docker tag demobook:v1 demobookacr.azurecr.io/demobook:v1
   ```

   第二个命令是将镜像推送到 ACR 资源，如下所示：

   ```
   docker push demobookacr.azurecr.io/demobook:v1
   ```

   这两个命令与我们学习的用于将 Docker 镜像打标签并推送到 Docker Hub 的命令完全相同，区别在于此处 ACR 的 Docker 注册表 URL 为 <acr name>.azurecr.io。

   以下截图显示了这些命令的执行情况：

   ![](img/d3344b39c888aab0b832625465f837c9_305_0.png)

   图 9.21 – 将 Docker 镜像推送到 ACR

   以下截图显示了 ACR 中推送的 Docker 镜像：

   ![](img/d3344b39c888aab0b832625465f837c9_306_0.png)

   图 9.22 – ACR 中的 Docker 镜像

5. 最后，要拉取此 Docker 镜像，我们将使用以下命令：

   ```
   docker pull demobookacr.azurecr.io/demobook:v1
   ```

   在本节中，我们学习了如何使用 Docker 在私有 Docker 注册表（即 ACR）中推送和拉取 Docker 镜像。

   在下一节中，我们将了解如何使用 CI/CD 管道在托管云容器服务（ACI 和 Terraform）中部署此镜像。

## 使用 CI/CD 管道将容器部署到 ACI

Docker 之所以迅速受到开发人员和运维团队的青睐，主要原因之一是 Docker 镜像和容器的部署使企业应用程序的 CI 和 CD 管道变得更加容易。

为了自动化我们应用程序的部署，我们将创建一个 CI/CD 管道，用于部署包含我们应用程序的 Docker 镜像到 ACI。

ACI 是 Azure 提供的一项托管服务，可让您非常轻松地部署容器，而无需担心硬件架构。

> **Note**
> 要了解有关 ACI 的更多信息，请访问官方页面 `https://azure.microsoft.com/en-us/services/container-instances/`。

此外，我们将使用 Terraform 进行 **基础设施即代码 (IaC)**，我们在 _第 2 章，使用 Terraform 预配云基础设施_ 中讨论过这一点，并使用 Azure ACI 资源及其与 Docker 镜像的集成。

因此，我们将本节分为以下两个部分：

- Azure ACI 的 Terraform 代码配置及其与我们 Docker 镜像的集成
- Azure Pipelines 中 CI/CD 管道的示例，允许您执行 Terraform 代码

首先，我们将编写允许您在 Azure 中预配 ACI 资源的 Terraform 代码。

## 编写 ACI 的 Terraform 代码

要使用 Terraform 预配 ACI 资源，我们导航到新的 `terraform-aci` 目录并创建一个 Terraform 文件 `main.tf`。

在此代码中，我们将使用 `azurerm_container_group` Terraform 对象为资源组和 ACI 资源提供 Terraform 代码。

> **Note**
>
> Terraform ACI 资源的文档可在此处查阅：`https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/container_group`。

此 `main.tf` 文件包含以下 Terraform 代码，用于创建资源组：

```
resource "azurerm_resource_group" "acidemobook" {
  name = "demoBook"
  location = "westus2"
}
```

在此 `main.tf` 文件中，我们在 Terraform 配置中添加 `variable` 声明，如下所示：

```
variable "imageversion" {
  description="Tag of the image to deploy"
}
variable "dockerhub-username" {
  description ="Tag of the image to deploy"
}
```

然后，我们使用 `azurerm_container_group` 资源块添加 ACI 的 Terraform 代码，如下所示：

```
resource "azurerm_container_group" "aci-myapp" {
 name = "aci-agent"
 location = "West Europe"
 resource_group_name = azurerm_resource_group.acidemobook.name
 os_type = "linux"
 container {
 name = "myappdemo"
 image = "docker.io/mikaelkrif/${{var.dockerhub-username}}:${{var.imageversion}}"
 cpu = "0.5"
 memory = "1.5"
 ports {
 port = 80
 protocol = "TCP"
 }
 }
}
```

在前面的代码片段中，我们执行以下操作：

- 我们声明 `imageversion` 和 `dockerhub-username` 变量，这些变量将在 CI/CD 管道期间实例化，并包含要部署的镜像的用户名和标签。
- 我们使用 Terraform 的 `azurerm_container_group` 资源来管理 ACI。在其 `image` 属性中，我们指示要部署的镜像的信息——即，其在 Docker Hub 中的完整名称及其标签，在我们的示例中，该标签在 `imageversion` 变量中。

最后，为了保护 Terraform 状态文件，我们可以使用 Terraform 远程后端，通过 Azure Blob 存储实现，正如我们在 _第 2 章，使用 Terraform 预配云基础设施_ 的 _保护远程后端中的状态文件_ 部分中讨论的那样。

