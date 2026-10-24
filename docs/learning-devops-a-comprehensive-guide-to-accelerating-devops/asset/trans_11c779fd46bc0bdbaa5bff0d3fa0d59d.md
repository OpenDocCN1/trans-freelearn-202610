

在上一节中，我们了解了如何使用 HashiCorp 的 **Vault** 保护敏感数据。在本节中，我们探讨了 Vault 中的数据加密和解密，并编写了 Terraform 代码，该代码将动态检索存储在 Vault 中的密钥。

在下一章中，我们将介绍 **蓝绿部署** 的概念及其用于减少部署停机时间的模式。然后，我们将学习如何在应用程序以及 Azure 基础设施部署中实现它。

## 问题

- 1. InSpec 的作用是什么？
- 2. 允许您通过命令行下载 InSpec 的包管理器的名称是什么？
- 3. 哪个 InSpec 命令允许您执行 InSpec 测试？
- 4. Vault 的发布者是谁？
- 5. 哪个命令以开发模式启动 Vault？
- 6. 当 Vault 在本地安装时，可以用于生产环境吗？
- 7. 在本地模式下，Vault 的加密数据存储在哪里？

## 进一步阅读

如果您想了解有关使用 InSpec、Vault 和 **Azure 安全开发工具包 (AzSK)** 进行 DevSecOps 的更多信息，以下是一些资源：

- InSpec 文档：`https://www.inspec.io/`
- HashiCorp Vault 文档：`https://www.vaultproject.io/docs/`
- 了解 HashiCorp Vault：`https://learn.hashicorp.com/vault`

# 15 减少部署停机时间

到目前为止，在本书中，我们讨论了 **DevOps** 实践，例如 **基础设施即代码 (IaC)**、**持续集成/持续部署 (CI/CD)** 管道以及不同类型测试的自动化。

在 _第 1 章，DevOps 文化和基础设施即代码实践_ 中，我们了解到这些 DevOps 实践将提高应用程序的质量，从而提高公司的财务收益。我们将进一步深入了解 DevOps 实践，探讨如何在部署期间确保应用程序的持续可用性，以及如何更频繁地将这些应用程序的新版本交付到生产环境中。

通常，我们看到的是，部署需要中断应用程序，例如——基础设施更改或服务停机。而且，我们还看到的是，公司仍然不愿更频繁地交付到生产环境。他们无法在生产环境中测试应用程序，或者正在等待其他依赖项。

在本章中，我们将探讨几种有助于改进应用程序交付流程的实践。我们将从一种在 Terraform 部署期间减少基础设施和应用程序停机时间的方法开始。然后，我们将讨论蓝绿部署的概念和模式，以及如何使用一些 Azure 资源对其进行配置。最后，我们将介绍在应用程序中实现功能标志的详细信息，这将允许您修改应用程序的操作，而无需在生产环境中重新部署它。

您还将学习如何配置 Terraform 代码以减少应用程序停机时间。您将能够使用蓝绿部署配置 Azure 资源，并使用开源组件或 LaunchDarkly 平台在应用程序中实现功能标志。

在本章中，我们将涵盖以下主题：

- 使用 Terraform 减少部署停机时间
- 了解蓝绿部署概念和模式
- 在 Azure 上应用蓝绿部署
- 功能标志简介
- 使用开源框架实现功能标志
- 使用 LaunchDarkly 解决方案

## 技术要求

为了理解本章中将介绍的 Terraform 概念，您需要阅读过 _第 2 章，使用 Terraform 配置云基础设施_。

我们将查看如何在 Azure 中实现蓝绿部署的示例。如果您没有 Azure 订阅，可以在此处创建免费的 Azure 帐户：`https://azure.microsoft.com/en-gb/free/`。

然后，我们将查看如何在 ASP.NET Core 应用程序中使用功能标志的示例。要使用我们的示例，您需要安装 **.NET Core 软件开发工具包 (SDK)**，可以从 `https://dotnet.microsoft.com/download` 下载。

对于代码编辑，我们使用了免费的 **Visual Studio Code (VS Code)** 编辑器，可以在此处下载：`https://code.visualstudio.com/`。

本章的完整源代码可在 `https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP15` 找到。

查看以下视频，观看代码实际运行情况：
https://bit.ly/3hcvrVH

## 使用 Terraform 减少部署停机时间

在第 2 章，使用 Terraform 配置云基础设施中，我们详细介绍了 Terraform 的使用，探讨了其命令和生命周期，并通过在 Azure 中的实施进行了实践。

Terraform 的一个问题是，根据需要实施的基础设施更改，Terraform 可能会自动销毁并重建某些资源。

为了充分理解这种行为，让我们查看以下 Terraform 执行的输出，该执行在 Azure 中配置了 Azure Web 应用，并已通过名称更改进行了修改：

![](img/d3344b39c888aab0b832625465f837c9_465_0.png)

图 15.1 – Terraform 停机时间

在这里，我们可以看到 Terraform 将销毁 Web 应用，然后使用新名称重建它。虽然销毁和重建是自动完成的，但在 Terraform 销毁和重建 Web 应用期间，应用程序对用户不可访问。

为了解决这个停机问题，我们可以添加 Terraform create_before_destroy 选项，如下所示：

```hcl
resource "azurerm_app_service" "webapp" {
  name = "MyWebAppBook1" #new name
  location = "West Europe"
  resource_group_name = "${azurerm_resource_group.rg-app.name}"
  app_service_plan_id = "${azurerm_app_service_plan.serviceplan-app.id}"
  app_settings = {
    WEBSITE_RUN_FROM_PACKAGE = var.package_zip_url
  }
  lifecycle { create_before_destroy = true }
}
```

添加此选项后，Terraform 将执行以下操作：

- 首先，Terraform 使用新名称创建一个新的 Web 应用。
- 在配置新 Web 应用期间，它使用 app_settings 属性中提供的 ZIP 格式应用程序包的 **统一资源定位符 (URL)**。使用 WEBSITE_RUN_FROM_PACKAGE 启动应用程序。
- 然后，Terraform 将销毁旧的 Web 应用。

使用 Terraform create_before_destroy 选项将确保在部署期间我们的应用程序的可行性。

但是，请注意，只有当正在创建的新资源允许我们在配置的同时非常快速地运行应用程序，从而不会发生服务中断时，此选项才有用。

在我们的 Web 应用示例中，当我们使用 Web 应用的 WEBSITE_RUN_FROM_PACKAGE 属性时，这奏效了。对于 **虚拟机 (VM)**，我们可以使用由 **Packer** 创建的 VM 映像。正如我们在 _第 4 章，使用 Packer 优化基础设施部署_ 中所见，Packer 包含有关 VM 映像中已更新的 VM 应用程序的信息。

> 注意
> 有关 `create_before_destroy` 选项的更多信息，请参阅以下 Terraform 文档：https://www.terraform.io/language/meta-arguments/lifecycle。

我们刚刚看到，使用 Terraform 和 IaC，在资源更改的情况下，可以减少部署期间的停机时间。

我们现在将探讨一种称为 **蓝绿部署** 的实践的概念和模式，它允许我们以极大的信心在生产环境中部署和测试应用程序。

## 了解蓝绿部署概念和模式

蓝绿部署是一种允许我们在不影响当前应用程序版本的情况下在生产环境中部署新版本应用程序的实践。在此方法中，生产架构必须由 **两个相同的环境** 组成；一个环境称为 **蓝色** 环境，而另一个环境称为 **绿色** 环境。

允许在环境之间进行路由的元素是路由器——即负载均衡器。

以下图表显示了蓝绿架构的简化示意图：

![](img/d3344b39c888aab0b832625465f837c9_467_0.png)

图 15.2 – 蓝绿架构

如我们所见，有两个相同的环境——称为 **蓝色** 的环境，即应用程序的当前版本，以及称为 **绿色** 的环境，即应用程序的新版本或下一个版本。我们还可以看到一个路由器，它将用户的请求重定向到蓝色环境或绿色环境。

