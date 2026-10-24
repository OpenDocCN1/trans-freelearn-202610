

除此之外，Terraform 拥有一大套内置函数，可用于操作数据或变量。要了解这些函数的更多信息，请参阅官方文档 `https://www.terraform.io/docs/configuration/functions.html`。

当然，还有许多其他良好实践，但这些实践应从第一行代码开始应用，以确保您的代码得到良好维护。

> **重要提示**
> 
> 本示例的完整最终代码可在 `https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP02/terraform_vars_interp` 获取。

我们已编写了一个 Terraform 配置，采用最佳实践，允许我们在 Azure 中创建一个提供网络和虚拟机的简单云基础设施。现在，让我们来看看如何使用我们的代码运行 Terraform 来预配此基础设施。

## 运行 Terraform 进行部署

编写好 Terraform 配置后，我们现在需要运行 Terraform 来部署基础设施。

但是，在任何执行之前，首先需要使用 Azure SP 进行身份验证，以确保 Terraform 可以管理 Azure 资源。

为此，我们可以设置特定于 Terraform 的环境变量以包含在 _Configuring Terraform for Azure_ 部分中先前创建的 SP 的信息，也可以使用 `az cli` 脚本。

以下脚本在 Linux 操作系统中导出四个 Terraform 环境变量：

```
export ARM_SUBSCRIPTION_ID=xxxxx-xxxxx-xxxxx-xxxx
export ARM_CLIENT_ID=xxxxx-xxxxx-xxxxx-xxxx
export ARM_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxx
export ARM_TENANT_ID=xxxxx-xxxxx-xxxxx-xxxx
```

此外，我们可以使用带有登录命令的 az cli 脚本：

```
az login
```

身份验证完成后，我们可以运行 Terraform 工作流。

在我们的场景中，我们从一个没有任何 Azure 资源组的空 Azure 订阅开始；然而，在现实世界中，我们的订阅可能已经包含一个资源组。

在运行 Terraform 之前，在 Azure 门户中，检查您的订阅中是否没有资源组，如下所示：

![](img/d3344b39c888aab0b832625465f837c9_72_0.png)

图 2.9 – Azure 上没有资源组

要运行 Terraform，我们需要打开一个命令行终端，例如 CMD、PowerShell 或 Bash，并导航到我们之前编写的 Terraform 配置文件所在的目录。

Terraform 配置的执行分为几个步骤，包括初始化、更改预览和应用这些更改。

接下来，让我们详细了解这些步骤的执行过程，从初始化步骤开始。

## 初始化

初始化步骤允许 Terraform 执行以下操作：

- 初始化 Terraform 上下文以检查并建立 Terraform 提供程序与远程服务之间的连接——在我们的案例中，这是与 Azure 的连接。
- 下载提供程序的插件——在我们的案例中，将是 azurerm 提供程序。
- 检查代码变量。

要执行初始化，请运行 init 命令：

```
terraform init
```

以下是 terraform init 的截图：

![](img/d3344b39c888aab0b832625465f837c9_73_0.png)

图 2.10 – 执行 terraform init 命令

如前面命令执行过程中所了解到的，Terraform 执行以下操作：

- 它下载 `azurerm` 插件的最新版本。
- 它创建一个工作 `.terraform` 目录。

以下是 `.terraform` 目录的截图：

![](img/d3344b39c888aab0b832625465f837c9_74_0.png)

图 2.11 – Terraform 配置目录

> **重要提示**
> 有关 `init` 命令行的更多信息，请参阅 `https://www.terraform.io/docs/commands/init.html` 中的文档。

一旦初始化步骤完成，我们可以进入下一步，即预览更改。

## 预览更改

下一步是在应用更改之前预览对基础设施所做的更改。为此，请使用 `plan` 命令运行 Terraform。执行时，plan 会自动使用 `terraform.tfvars` 文件来设置变量。要执行它，请启动 `plan` 命令：

```
terraform plan
```

以下输出显示了 terraform plan 命令的执行：

```
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be persisted to local or remote state storage.

------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_network_interface.nic
      id:                                    <computed>
      applied_dns_servers.#:                 <computed>
      dns_servers.#:                         <computed>
      enable_accelerated_networking:         "false"
      enable_ip_forwarding:                  "false"
      internal_dns_name_label:               <computed>
      internal_fqdn:                         <computed>
      ip_configuration.#:                    "1"
      ip_configuration.0.application_gateway_backend_address_pool_ids.#: <computed>
      ip_configuration.0.application_security_group_ids.#:               <computed>
      ip_configuration.0.load_balancer_backend_address_pool_ids.#:       <computed>
      ip_configuration.0.load_balancer_inbound_nat_rules_ids.#:          <computed>
      ip_configuration.0.name:               "bookipconfig"
      ip_configuration.0.primary:            <computed>
      ip_configuration.0.private_ip_address_allocation:                  "dynamic"
      ip_configuration.0.private_ip_address_version:                     "IPv4"
      ip_configuration.0.public_ip_address_id:                           "${azurerm_public_ip.pip.id}"
      ip_configuration.0.subnet_id:                                      "${azurerm_subnet.subnet.id}"
      location:                              "westeurope"

  + azurerm_virtual_network.vnet
      id:                                    <computed>
      address_space.#:                       "1"
      address_space.0:                       "10.0.0.0/16"
      location:                              "westeurope"
      name:                                  "book-vnet"
      resource_group_name:                   "bookRg"
      subnet.#:                              <computed>
      tags.%:                                <computed>

Plan: 7 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform can't guarantee that exactly these actions will be performed if
```

图 2.12 – 执行 terraform plan 命令

在执行 plan 命令期间，该命令会显示将受更改影响的资源的名称和属性。它还显示新资源的数量、将被修改的资源数量以及将被删除的资源数量。

## 应用更改

在验证 `plan` 命令的结果符合我们的预期后，最后一步是实时应用 Terraform 代码以预配并将更改应用到我们的基础设施。

为此，我们将执行 `apply` 命令：

```
terraform apply
```

此命令执行与 `plan` 命令相同的操作，并交互式地询问用户确认我们是否要实施更改。

以下是 `terraform apply` 确认的截图：

![](img/d3344b39c888aab0b832625465f837c9_76_0.png)

图 2.13 – Terraform 中要应用的更改的确认

确认是通过输入 `yes`（或输入 `no` 以取消）给出的，然后 Terraform 将更改应用到基础设施。

## 重要提示

有关 `plan` 命令行的更多信息，请参阅 https://www.terraform.io/docs/commands/plan.html 中的文档。

因此，我们刚刚看到了将要应用到我们基础设施的更改预测。现在，我们将查看如何应用它们。

