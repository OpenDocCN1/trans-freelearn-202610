

## 构建 Consul 集群

首先，切换到 ch2 的根目录。在这里，我们将找到用于构建集群的 Terraform 脚本。你可以通过运行以下命令来完成此操作：

```
$ cd $github_root/Consul-Up-and-Running/ch2/
```

与构建 Packer 镜像时类似，此目录中有几个文件需要我们注意。暂时不用关心 files 目录，等我们评估用于构建集群的 Terraform 脚本时再深入探讨。

如果你查看我们的 variables.tf 文件，这里定义了 Terraform 所需的变量。我总是分不清变量在哪里定义、在哪里赋值。总之，对于这五个变量，每个都有描述（这总是一个好习惯）和一个合理的默认值（好吧，从我的角度来看是合理的）。请看下面的代码片段：

### variables.tf

```
variable "aws_region" {
  type        = string
  default     = "us-east-1"
  description = "Region for AWS Components"
}
variable "owner" {
  type        = string
  default     = "consul-demo"
}
variable "consul_region" {
  type        = string
  description = "Region of consul server (not AWS Region)"
  default     = "global"
}
variable "vpc_cidr" {
  type        = string
  description = "CIDR of the VPC"
  default     = "192.168.100.0/24"
}
variable "instance_type" {
```

```
type          = string
description   = "machine instance type"
default       = "t3.micro"
}
```

如果你更愿意为必要的变量使用自己的值，别担心——我不会介意的。这些变量在 terraform.tfvars 文件中设置。请看下面的代码片段：

### Terraform.tfvars

```
owner         = "rjackson"
aws_region    = "us-east-2"
instance_type = "t2.small"
```

好了——现在我们已经按照自己的喜好设置好了变量，是时候启动我们的基础设施了！如果你以前从未使用过 Terraform，别担心——它非常简单，而且所有代码都已构建完成。再说一次，我从不自称是软件开发人员，所以请不要嘲笑我的代码！首先，我们要启动基础设施的构建，然后剖析我们构建的内容。要开始，Terraform 需要下载任何必要的特定于提供程序的数据。这通过以下命令执行：

```
$ terraform init
```

当然，在不检查将要构建的内容的情况下就进行构建从来都不是好事，所以接下来，我们要规划我们的构建。此命令的所有输出都应该是绿色的加号（+），表示将要创建哪些基础设施。请注意，其中许多项目在 apply 操作之后才能确定，因为它们需要从 AWS 分配，但浏览一下你将要构建的内容可能会很有趣。如果不感兴趣，别担心——我们会在启动构建后审查细节。

现在，运行以下命令：

```
$ terraform plan
```

接下来，我们要应用我们的基础设施构建。这通过你猜对了的 terraform apply 来完成。这会再次触发一个 plan，并显示你将要构建的内容，但既然我们已经做过这件事了，那就来个危险的操作吧，直接自动批准。请看下面的代码片段：

```
$ terraform apply --auto-approve
```

> 注意
> 对于你在本练习中产生的任何费用，作者概不负责。请务必在白天或夜晚结束时销毁你的基础设施。如果你想设置一个 m3.xlarge 系统并让它连续运行几天，那随你便！

好的——让我们来看看我们在构建什么。以下所有代码块（本节中的）都在 main.tf 文件中。Terraform 的最佳实践是将所有代码片段放在一个文件中，所以我们要做的就是这样！

第一部分相当简单，只是设置了我们将要使用的 Terraform 提供程序。Terraform 提供程序有点像提供特定于该提供程序的功能的模块——例如，我们正在安装 AWS 提供程序，使我们能够管理 AWS 中的资源。我们指定了该提供程序的版本，以及我们将要工作的区域。provider 块也是调皮的人会插入其 AWS 凭据的地方，但我们永远不会那样做——对吧？？？最后，我们使用 AWS 数据源来查找我们刚刚使用 Packer 构建的镜像。以下是您需要的代码：

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 2.5"
    }
  }
}
provider "aws" {
  region = var.aws_region
}
data "aws_ami" "an_image" {
  most_recent = true
  owners      = ["self"]
  filter {
    name   = "name"
    values = ["${var.owner}-consul-*"]
  }
}
```

下一个部分创建了一个私钥，供我们用来 SSH 登录到我们的机器。一个组织通常会维护自己的私钥用于机器访问，但这些私钥确实不应该随意流传。更安全的方法是使用 HashiCorp Vault Secure Shell (SSH) 助手来生成一次性 SSH 密钥，但那是另一个故事了。密钥将构建在我们的 Consul 机器内部，但我们也会有一个以 -consul-key.pem 结尾的本地 Privacy Enhanced Mail (PEM) 文件。您需要的代码如下面片段所示：

```
resource "tls_private_key" "my_private_key" {
  algorithm = "RSA"
  rsa_bits = 4096
}
resource "local_file" "private_key" {
  content = tls_private_key.my_private_key.private_key_pem
  filename = "${var.owner}-consul-key.pem"
  file_permission = 0400
}
resource "aws_key_pair" "consul_key" {
  key_name = "${var.owner}-consul-key"
  public_key = tls_private_key.my_private_key.public_key_openssh
}
```

本部分提供了我们系统所需的大部分网络信息。由于我们不是在阅读关于 AWS 网络复杂细节的书，你就得相信我，下面片段中的所有代码都很酷：

```
resource aws_vpc "consul-demo" {
  cidr_block           = var.vpc_cidr
  enable_dns_hostnames = true
  tags = {
    Name = "${var.owner}-vpc"
  }
}
resource aws_subnet "consul-demo" {
  vpc_id     = aws_vpc.consul-demo.id
  cidr_block = var.vpc_cidr
  tags = {
    name = "${var.owner}-subnet"
```
```
}
}
resource aws_internet_gateway "consul-demo" {
  vpc_id = aws_vpc.consul-demo.id

  tags = {
    Name = "${var.owner}-internet-gateway"
  }
}
resource aws_route_table "consul-demo" {
  vpc_id = aws_vpc.consul-demo.id
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.consul-demo.id
  }
}
resource aws_route_table_association "consul-demo" {
  subnet_id     = aws_subnet.consul-demo.id
  route_table_id = aws_route_table.consul-demo.id
}
```

好的——这部分网络内容可不是能轻易略过的。AWS 安全组确定了哪些设备之间开放哪些端口。以下是已定义端口及其用途的列表：

-   端口 22—SSH 访问。
-   端口 8300-8302—这些涵盖了节点之间所有的共识、RPC 和 gossip 通信。
-   端口 8500—用于 Consul 的 Web 用户界面（UI）。
-   端口 8600—Consul 利用端口 8600 进行域名系统（DNS）查询，作为服务发现功能的一部分。

这些端口都已在安全组配置中设置并配置完毕。以下是说明这一点的代码：

```
resource aws_security_group "consul-demo" {
  name   = "${var.owner}-security-group"
  vpc_id = aws_vpc.consul-demo.id
```

```
ingress {
    from_port    = 22
    to_port      = 22
    protocol     = "tcp"
    cidr_blocks  = ["0.0.0.0/0"]
}
ingress {
    from_port    = 8300
    to_port      = 8302
    protocol     = "tcp"
    cidr_blocks  = ["0.0.0.0/0"]
}
ingress {
    from_port    = 8500
    to_port      = 8500
    protocol     = "tcp"
    cidr_blocks  = ["0.0.0.0/0"]
}
ingress {
    from_port    = 8600
    to_port      = 8600
    protocol     = "tcp"
    cidr_blocks  = ["0.0.0.0/0"]
}
egress {
    from_port        = 0
    to_port          = 0
    protocol         = "-1"
    cidr_blocks      = ["0.0.0.0/0"]
    prefix_list_ids  = []
}
tags = {
    Name = "${var.owner}-security-group"
}
}
```

