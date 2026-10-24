

## 任何一段 Terraform 代码都由相同的语法模型组成

任何一段 Terraform 代码都由相同的语法模型组成，Terraform 对象的语法由四个部分组成：

- `resource` 或 `data` 块的类型
- 要管理的资源名称（例如，`azurerm_resource_group`）
- 内部 Terraform ID（例如，`rg`）
- 对应于资源实际属性的属性列表（即，`name` 和 `location`）

> Important Note
> 有关 Terraform 语法的更多文档可在 https://www.terraform.io/docs/configuration-0-11/resources.html 上找到。

此代码使用 `azurerm_resource_group` Terraform 资源，将预配一个名为 `bookRg`、存储在 West Europe 位置的资源组。

2. 然后，我们将编写网络部分的代码：

```hcl
resource "azurerm_virtual_network" "vnet" {
    name = "book-vnet"
    location = "West Europe"
    address_space = ["10.0.0.0/16"]
    resource_group_name = azurerm_resource_group.rg.name
}

resource "azurerm_subnet" "subnet" {
    name = "book-subnet"
    virtual_network_name = azurerm_virtual_network.vnet.name
    resource_group_name = azurerm_resource_group.rg.name
    address_prefix = "10.0.10.0/24"
}
```

在这段用于网络部分的 Terraform 代码中，我们创建了一个名为 book-vnet 的 VNet 的代码，并在其中创建了一个名为 book-subnet 的子网。

如果我们仔细查看这段代码，可以看到，对于资源之间的依赖关系，我们没有使用明确的 ID，而是使用对 Terraform 资源的指针。

VNet 和子网是资源组的属性，使用 `${azurerm_resource_group.rg.name}`，这告诉 Terraform VNet 和子网将在资源组之后创建。至于子网，它通过使用 `${azurerm_virtual_network.vnet.name}` 值依赖于其 VNet；这是显式依赖概念。

现在，让我们编写虚拟机的 Terraform 预配代码，它由以下内容组成：

- 一个网络接口
- 一个公共 IP 地址
- 一个用于诊断启动（启动信息日志）的 Azure 存储对象
- 一个虚拟机

带有 IP 配置的 **网络接口** 的示例代码如下：

```hcl
resource "azurerm_network_interface" "nic" {
  name = "book-nic"
  location = "West Europe"
  resource_group_name = azurerm_resource_group.rg.name
  ip_configuration {
    name = "bookipconfig"
    subnet_id = azurerm_subnet.subnet.id
    private_ip_address_allocation = "Dynamic"
    public_ip_address_id = azurerm_public_ip.pip.id
  }
}
```

在这段 Terraform 代码中，我们使用了一个 azurerm_network_interface 块（https://www.terraform.io/docs/providers/azurerm/r/network_interface.html）。在其中，我们配置了网络接口的名称、区域、资源组和带有动态 IP 地址的 IP 配置。

公共 IP 地址的代码，该 IP 地址位于我们刚刚创建的子网中，如下所示：

```hcl
resource "azurerm_public_ip" "pip" {
  name = "book-ip"
  location = "West Europe"
  resource_group_name = "${azurerm_resource_group.rg.name}"
  public_ip_address_allocation = "Dynamic"
  domain_name_label = "bookdevops"
}
```

在这段 Terraform 代码中，我们在 https://www.terraform.io/docs/providers/azurerm/r/public_ip.html 上使用了一个 azurerm_public_ip 块。在其中，我们配置了 IP 地址的动态分配和 DNS 标签。

存储帐户的代码，我们用于启动诊断日志，如下所示：

```hcl
resource "azurerm_storage_account" "stor" {
  name = "bookstor"
  location = "West Europe"
  resource_group_name = azurerm_resource_group.rg.name
  account_tier = "Standard"
  account_replication_type = "LRS"
}
```

在这段 Terraform 代码中，我们在 https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview 上使用了一个 azurerm_storage_account 块。在其中，我们配置了名称、区域、资源组和存储类型，在我们的情况中，是 Standard LRS。

> Important Note
> 存储帐户的文档可在 https://docs.microsoft.com/en-us/azure/storage/common/storage-account-overview 上找到。

以及包含先前创建的网络接口 ID 的 Ubuntu 虚拟机的代码，如下所示：

```hcl
resource "azurerm_linux_virtual_machine" "vm" {
  name = "bookvm"
  location = "West Europe"
  resource_group_name = azurerm_resource_group.rg.name
  vm_size = "Standard_DS1_v2"
  network_interface_ids = ["${azurerm_network_interface.nic.id}"]
  storage_image_reference {
    publisher = "Canonical"
    offer = "UbuntuServer"
    sku = "16.04-LTS"
    version = "latest"
  }
  ...
}
```

在这段 Terraform 代码中，我们在 https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/linux_virtual_machine 上使用了一个 azurerm_linux_virtual_machine 块。在其中，我们配置了名称、大小（Standard_DS1_V2）、对 network_interface Terraform 对象的引用以及虚拟机操作系统的类型（Ubuntu）。

所有这些代码部分都与之前的部分完全相同，使用显式依赖来指定资源之间的关系。

> Important Note
> 此完整源代码可在 https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP02/terraform_simple_script 上找到。

我们刚刚创建了一个完整的 Terraform 脚本，允许我们预配一个小型 Azure 基础设施。但是，与任何语言一样，关于文件分离、应用清晰可读的代码以及最后使用内置函数，都有良好的实践。

## 遵循一些 Terraform 良好实践

我们刚刚查看了用于预配 Azure 基础设施的 Terraform 代码示例，但查看编写 Terraform 代码的一些良好实践也是有用的。

### 通过文件分离获得更好的可见性

执行 Terraform 代码时，`execution` 目录中所有带有 `.tf` 扩展名的配置文件都会自动执行；在我们的示例中，我们有 `provider.tf` 和 `main.tf`。将代码分离到多个文件中是很好的，以便提高代码的可读性和可演进性。

使用我们的示例脚本，我们可以通过以下方式将其分离来做得更好：

- `Rg.tf`：包含资源组的代码。
- `Network.tf`：包含 VNet 和子网的代码。
- `Compute.tf`：包含网络接口、公共 IP、存储和虚拟机的代码。

> Important Note
> 带有单独文件的完整代码可在 `https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP02/terraform_separate_files` 上找到。

### 敏感数据的保护

必须小心 Terraform 配置中的敏感数据，例如密码和访问权限。我们已经了解到，对于对 Azure 的访问身份验证，没有必要将它们留在代码中。此外，在我们关于虚拟机管理员帐户的示例中，请注意虚拟机管理员帐户的密码已在此 Terraform 配置中明确指定。为了解决这个问题，我们可以使用强大的密钥管理器来存储密码，例如 Azure Key Vault 或 HashiCorp Vault，并通过 Terraform 获取它们。

## 使用变量和插值函数动态化配置

编写 Terraform 配置时，重要的是从一开始就考虑到，托管应用程序的基础设施通常对所有阶段都是相同的。但是，只有某些信息会因阶段而异，例如资源名称和实例数量。

为了给代码提供更多灵活性，我们必须在代码中使用变量，步骤如下：

1. 通过在全局 Terraform 代码中添加以下示例代码来声明变量。或者，我们可以将其添加到另一个文件（例如 `variables.tf`）中，以便更好地阅读代码：

```hcl
variable "resource_group_name" {
    description ="Name of the resource group"
}
variable "location" {
    description ="Location of the resource"
    default ="West Europe"
}
variable "application_name" {
    description ="Name of the application"
}
```

2. 在另一个名为 `terraform.tfvars` 的 `.tfvars` 文件中，使用 `variable_name=value` 语法实例化它们的值，类似于以下内容：

```hcl
resource_group_name ="bookRg"
application_name ="book"
```

3. 在代码中使用这些变量，使用 `var.<name of the variable>`；例如，在资源组 Terraform 代码中，我们可以编写以下内容：

```hcl
resource "azurerm_resource_group" "rg" {
    name = var.resource_group_name
    location = var.location
    tags {
        environment = "Terraform Azure"
    }
}
```

