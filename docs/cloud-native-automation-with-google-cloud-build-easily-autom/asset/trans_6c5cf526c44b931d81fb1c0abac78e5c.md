
```
description = "Google Cloud Zone in which regional GCP resources are provisioned"
  type      = string
  default   = "us-west1-a"
}
EOF
```

Network-related variables, such as name and network range, will be used by the compute instance:

```
$ cat << EOF >> variables.tf
variable "network_name" {
  description = "The name of the network to be used when deploying resources"
  type        = string
  default     = "packt-network"
}
variable "subnet_name" {
  description = "The name of the subnet to be used when deploying resources"
  type        = string
  default     = "packt-subnet"
}
variable "subnet_cidr" {
  description = "The name of the subnet to be used when deploying resources"
  type        = string
  default     = "10.128.0.0/24"
}
EOF
```

以下是定义机器类型、名称和操作系统（OS）镜像的计算相关变量：

```
$ cat << EOF >> variables.tf
variable "instance_name" {
  description = "The name of the GCE VM instance named to be provisioned"
  type        = string
  default     = "packt-instance"
}
variable "instance_machine_type" {
    description = "The machine type you want the GCE VM to use"
    type        = string
    default     = "e2-medium"
}
variable "instance_image" {
    description = "The OS image you want the GCE VM boot disk to use"
    type        = string
    default     = "rocky-linux-cloud/rocky-linux-8-v20220406"
}
EOF
```

设置 GCE VM 实例使用的网络：

```
$ cat << EOF > vpc.tf
resource "google_compute_network" "packt_network" {
  name                         = "vpc-network"
  auto_create_subnetworks      = false
}
resource "google_compute_subnet" "packt_subnet" {
  name          = var.subnet_name
  ip_cidr_range = var.subnet_cidr
  region        = var.region
  network       = google_compute_network.packt_network.name
}
EOF
```

启动 GCE VM 实例所需的最小配置如下：

```
$ cat << EOF > gce.tf
resource "google_compute_instance" "packt_instance" {
  name         = var.instance_name
  machine_type = var.instance_machine_type
  boot_disk {
    initialize_params {
      image = var.instance_image
}
}
  network_interface {
    subnetwork = google_compute_subnetwork.packt_subnet.name
  }
}
EOF
```

现在我们已经定义了所有 Terraform 配置文件，可以运行一些 Terraform 命令来测试并启动我们配置的 GCE 实例。我们首先使用 terraform init 命令来验证我们与后端以及配置中指定的必要模块的连接性：

```
$ terraform init
```

以下是 init 命令的示例输出：

```
Initializing the backend...
Successfully configured the backend "gcs"! Terraform will automatically
use this backend unless the backend configuration changes.
Initializing provider plugins...
- Reusing previous version of hashicorp/google from the dependency lock file
- Installing hashicorp/google v4.16.0...
- Installed hashicorp/google v4.16.0 (signed by HashiCorp)
Terraform has been successfully initialized!
```

我们运行 terraform plan 命令来预览配置中指定的更改，并使用 -out 开关将计划输出为 Terraform 推荐的方式。这将允许对 Terraform 将尝试在指定的 Google Cloud 项目上执行的操作进行验证和代码审查。开发人员和运维人员可以使用 terraform show terraform.tfplan 来查看计划：

```
$ terraform plan -out terraform.tfplan
```

以下是计划的示例输出：

```
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated
with the following symbols:
  + create

Terraform will perform the following actions:
  # google_compute_instance.main will be created
  + resource "google_compute_instance" "main" {
      + can_ip_forward      = false
      + cpu_platform        = (known after apply)
...
          + node_affinities {
              + key      = (known after apply)
              + operator = (known after apply)
              + values   = (known after apply)
          }
      }
  }
Plan: 1 to add, 0 to change, 0 to destroy.
...
Saved the plan to: terraform.tfplan
To perform exactly these actions, run the following command to apply:
    terraform apply "terraform.tfplan"
```

而不是直接使用 terraform apply 来实施更改，我们现在有一个计划文件，可以在流水线中专门应用已批准的更改：

```
$ terraform apply terraform.tfplan
```

以下是 apply 函数的示例输出：

```
google_compute_instance.main: Creating...
google_compute_instance.main: Still creating... [10s elapsed]
google_compute_instance.main: Creation complete after 11s
[id=projects/**REDACTED_project_id**/zones/us-west1-a/
instances/test-instance]
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

现在我们已经手动测试了 Terraform 执行，可以将其添加到 Cloud Build 配置流水线以自动化此过程。在 Cloud Build 中，我们将执行前面描述的各个 Terraform 命令步骤，每个步骤都表示为一个构建步骤。如果不取消配置（即销毁）先前创建的资源，Cloud Build 配置将不会进行任何更改，因为之前创建的 Terraform 配置会被重复使用。运行以下命令以销毁已创建的资源：

```
$ terraform destroy --auto-approve
```

在开始使用 Cloud Build 之前，我们需要授予 Cloud Build 默认服务账号网络和计算权限以创建我们的资源。之前的命令在终端中运行 terraform 时使用了你的本地服务账号。当我们使用 Cloud Build 执行且未指定服务账号时，它将使用 Cloud Build 默认服务账号：

![](img/2fe9576240c146c49e3a21d5e7f6af00_127_0.png)

图 7.1 – Cloud Build 和 Terraform，一体化方法

首先，我们需要检索 Google Cloud 项目编号，因为它用于构造 Cloud Build 默认服务账号 PROJECT_NUMBER@cloudbuild.gserviceaccount.com：

```
$ export PROJECT_NUMBER=$(gcloud projects list \
  --filter="$(gcloud config get-value project)" \
  --format="value(PROJECT_NUMBER)"
)
```

为 Cloud Build 服务账号授予我们将创建的资源的管理权限：

```
$ gcloud projects add-iam-policy-binding ${PROJECT_ID} \
  --member=serviceAccount:${PROJECT_NUMBER}@cloudbuild.gserviceaccount.com \
  --role=roles/compute.networkAdmin
```

```bash
$ gcloud projects add-iam-policy-binding ${PROJECT_ID} \
  --member=serviceAccount:${PROJECT_NUMBER}@cloudbuild.
gserviceaccount.com \
  --role=roles/compute.instanceAdmin.v1
```

在此示例中，我们将使用 hashicorp/terraform:1.0.0 容器镜像来提供 terraform 可执行文件，从 terraform init 开始：

```bash
$ cat << EOF > cloudbuild.yaml
timeout: 3600s
steps:
- id: 'tf init'
  name: 'hashicorp/terraform:1.0.0'
  env:
  - "TF_IN_AUTOMATION=true"
  entrypoint: 'sh'
  args:
  - '-c'
  - |
    terraform init -input=false
EOF
```

下一个构建步骤是 terraform plan，用于概述更改：

```bash
$ cat << EOF >> cloudbuild.yaml
- id: 'tf plan'
  name: 'hashicorp/terraform:1.0.0'
  env:
  - "TF_IN_AUTOMATION=true"
  entrypoint: 'sh'
  args:
  - '-c'
  - |
    terraform plan -input=false -out terraform.tfplan
EOF
```

最后一个构建步骤是 terraform apply，用于应用已概述的更改：

```
$ cat << EOF >> cloudbuild.yaml
- id: 'tf apply'
  name: 'hashicorp/terraform:1.0.0'
  env:
  - "TF_IN_AUTOMATION=true"
  entrypoint: 'sh'
  args:
  - '-c'
  - |
    terraform apply -input=false terraform.tfplan
EOF
```

一旦我们拥有 Cloud Build 配置，就可以通过运行在前几章中用于提交构建的命令手动执行构建。将会使用默认的 Cloud Build 服务账号：

```
$ gcloud builds submit --region us-west1
```

成功构建将产生以下截断输出，并显示 SUCCESS 状态：

```
...
Step #2 - "tf apply": Apply complete! Resources: 3 added, 0 changed, 0 destroyed.
Finished Step #2 - "tf apply"
PUSH
DONE
...
STATUS: SUCCESS
```

**注意**
在自动化中，TF_IN_AUTOMATION 变量被设置为这样一些说明会被截断，并且不会呈现手动说明。例如，在设置该变量后自动执行 terraform plan 后，以下语句不会输出到控制台：“To perform exactly these actions...”
