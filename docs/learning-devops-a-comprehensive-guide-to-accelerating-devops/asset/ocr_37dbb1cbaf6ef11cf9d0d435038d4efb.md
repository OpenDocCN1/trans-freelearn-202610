In this Terraform code for the network part, we create the code for a VNet, book-vnet, and in it, we create a subnet called book-subnet.

If we look at this code carefully, we can see that, for the dependencies between the resources, we do not put in clear IDs, but we use pointers on the Terraform resources.

The VNet and subnet are the property of the resource group with ${azurerm_resource_group.rg.name}, which tells Terraform that the VNet and subnet will be created just after the resource group. As for the subnet, it is dependent on its VNet with the use of the ${azurerm_virtual_network.vnet.name} value; it's the explicit dependence concept.

Now, let's write the Terraform provisioning code of the virtual machine, which is composed of the following:

+   - A network interface
+   - A public IP address
+   - An Azure Storage object for the diagnostic boot (boot information logs)
+   - A virtual machine

The sample code for the **network interface** with the IP configuration is as follows:

```
```
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
```

In this Terraform code, we use an azurerm_network_interface block (https:// www.terraform.io/docs/providers/azurerm/r/network_interface. html). In it, we configure the name, region, resource group, and IP configuration with the dynamic IP address of the network interface.