

In addition to this, Terraform has a large list of built-in functions that can be used to manipulate data or variables. To learn more about these functions, please refer to the official documentation at `https://www.terraform.io/docs/configuration/functions.html`.

Of course, there are many other good practices, but these are to be applied from the first lines of code to ensure that your code is well maintained.

> **Important Note**
> 
> The complete and final code of this example is available at `https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP02/terraform_vars_interp`.

We have written a Terraform configuration, using best practices, which allows us to create a simple cloud infrastructure in Azure that provides a network and a virtual machine. Now, let's take a look at how to run Terraform with our code in order to provision this infrastructure.

## Running Terraform for deployment

With the Terraform configuration written, we now need to run Terraform to deploy our infrastructure.

However, before any execution, first, it is necessary to provide authentication with the Azure SP to ensure that Terraform can manage the Azure resources.

To do this, we can either set the environment variables specific to Terraform to contain the information of the SP created earlier in the _Configuring Terraform for Azure_ section, or we can use the `az cli` script.

The following script exports the four Terraform environment variables in the Linux OS:

```
export ARM_SUBSCRIPTION_ID=xxxxx-xxxxx-xxxxx-xxxx
export ARM_CLIENT_ID=xxxxx-xxxxx-xxxxx-xxxx
export ARM_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxx
export ARM_TENANT_ID=xxxxx-xxxxx-xxxxx-xxxx
```

Additionally, we can use the az cli script with the login command:

```
az login
```

Once authenticated, we can run the Terraform workflow.

In our scenario, we begin with an empty Azure subscription without any Azure resource groups; however, in the real world, our subscription might already contain a resource group.

Before running Terraform, in the Azure portal, check that you do not have a resource group in your subscription, as follows:

![](img/d3344b39c888aab0b832625465f837c9_72_0.png)

Figure 2.9 – No resource group on Azure

To run Terraform, we need to open a command-line Terminal such as CMD, PowerShell, or Bash and navigate to the directory where the Terraform configuration files we wrote earlier are located.

The Terraform configuration is executed in several steps, including initialization, the preview of changes, and the application of those changes.

Next, let’s take a look, in detail, at the execution of these steps, starting with the initialization step.

## Initialization

The initialization step allows Terraform to do the following:

- Initialize the Terraform context to check and make the connection between the Terraform provider and remote service—in our case, this is with Azure.
- Download the plugin(s) of the provider(s)—in our case, it will be the azurerm provider.
- Check the code variables.

To execute the initialization, run the init command:

```
terraform init
```

The following is a screenshot of terraform init:

![](img/d3344b39c888aab0b832625465f837c9_73_0.png)

Figure 2.10 – Executing the terraform init command

As we have gathered during its execution of the preceding command, Terraform does the following:

- It downloads the latest version of the `azurerm` plugin.
- It creates a working `.terraform` directory.

The following is a screenshot of the `.terraform` directory:

![](img/d3344b39c888aab0b832625465f837c9_74_0.png)

Figure 2.11 – The Terraform configuration directory

> **Important Note**
> For more information about the `init` command line, please refer to the documentation at `https://www.terraform.io/docs/commands/init.html`.

Once the initialization step is complete, we can proceed to the next step, which is previewing the changes.

## Previewing the changes

The next step is to preview the changes made to the infrastructure before applying them. To do this, run Terraform with the `plan` command. When executed, the plan automatically uses the `terraform.tfvars` file to set the variables. To execute it, launch the `plan` command:

```
terraform plan
```

The following output shows the execution of the terraform plan command:

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

Figure 2.12 – Executing the terraform plan command

During the execution of the plan command, the command displays the name and properties of the resources that will be impacted by the change. It also displays the number of new resources and the number of resources that will be modified, along with the number of resources that will be deleted.

## Applying the changes

After validating that the result of the `plan` command corresponds to our expectations, the final step is the application of the Terraform code in real time to provision and apply the changes to our infrastructure.

To do this, we will execute the `apply` command:

```
terraform apply
```

This command does the same operation as the `plan` command and interactively asks the user for confirmation that we want to implement the changes.

The following is a screenshot of the `terraform apply` confirmation:

![](img/d3344b39c888aab0b832625465f837c9_76_0.png)

Figure 2.13 – Confirmation of the changes to be applied in Terraform

The confirmation is given by inputting `yes` (or `no` to cancel), then Terraform applies the changes to the infrastructure.

## Important Note

For more information about the `plan` command line, please refer to the documentation at https://www.terraform.io/docs/commands/plan.html.

Therefore, we have just seen a prediction of the changes that will be applied to our infrastructure. Now, we will view how to apply them.

