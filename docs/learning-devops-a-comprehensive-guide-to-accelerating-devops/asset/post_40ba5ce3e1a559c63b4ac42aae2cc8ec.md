

## Overview of InSpec

InSpec is an open source tool written in Ruby that runs on the command line and is produced by one of the leading DevOps tools, **Chef**, whose website is `https://www.chef.io/`. It allows users writing declarative-style code to test the compliance of a system or infrastructure.

To use InSpec, it’s not necessary to learn a new scripting language; we should already have enough knowledge to write the desired state of the infrastructure resources or the system we want to test.

With InSpec, we can test the compliance of remote machines and data and, since the latest version, it is also possible to test a cloud infrastructure such as Azure, **Amazon Web Services (AWS)**, and **Google Cloud Platform (GCP)**.

After this little overview of InSpec, let’s look at how to download and install it.

## Installing InSpec

We have seen in the _Technical requirements_ section that InSpec needs to have **Ruby (>2.4)** installed on our machine.

InSpec can be installed either manually or via a script, as outlined here:

- **Manually**: This can be done by downloading the package corresponding to our OS from `https://www.chef.io/downloads/tools/inspec`.
- **With a script**: We can install InSpec by executing the commands detailed next in a terminal.

On Windows, we can download and install InSpec using the Chocolatey package at `https://community.chocolatey.org/packages/inspec` with the following command:

```
choco install inspec -y
```

On Linux, use the following script:

```
curl https://omnitruck.chef.io/install.sh | sudo bash -s -- -P inspec
```

The following screenshot shows the installation of InSpec via the **Chocolatey package**:

![](img/d3344b39c888aab0b832625465f837c9_437_0.png)

## Figure 14.1 – InSpec installation on Windows

### Note

For more information on the installation of InSpec for all OSes, refer to this documentation: `https://docs.chef.io/inspec/install/`.

To verify that InSpec has been correctly installed and is working, we run the `inspec --version` command to display its version, and the `inspec` command to display the list of available commands.

The following screenshot shows the execution of these commands:

![](img/d3344b39c888aab0b832625465f837c9_437_1.png)

## Figure 14.2 – Checking the InSpec version and displaying InSpec options

Also, as with many of the tools already detailed in this book, InSpec has been integrated into the **Azure Cloud Shell** tool suite, as shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_438_0.png)

Figure 14.3 – InSpec in Azure Cloud Shell

We have just seen the different ways to install InSpec, and we’ll now see the configuration of Azure for InSpec.

## Configuring Azure for InSpec

Before writing test cases to test the compliance of our Azure infrastructure, we need to create an Azure service principal that has read permission on the Azure resources that will be tested.

To create this Azure service principal, we’ll use the same procedure that we already detailed in the _Configuring Terraform for Azure_ section of _Chapter 2, Provisioning Cloud Infrastructure with Terraform_.

Using the **Azure CLI** tool, we execute the following `az cli` command:

```
az ad sp create-for-rbac -name="<SP name>" -role="Reader" -scopes="/subscriptions/<subscription Id>"
```

This preceding command requires the following parameters:

- `-name` is the name of the Azure service principal to be created.
- `--scopes` is the **identifier (ID)** of the subscription (or other scopes) in which the Azure resources will be present.
- `--role` is the role name that the service principal will have on the specified resource scope.

The execution of this preceding command returns the following three pieces of authentication information relating to the created service principal:

- The client ID
- The client secret
- The tenant ID

> Note
> For more details on service principals, see the following documentation:
> https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest

We’ll see how to use this authentication information when running InSpec, but before it is executed, we need enough knowledge to write InSpec tests. Let’s see how to do this.

## Writing InSpec tests

After installing InSpec and configuring authentication for Azure, we can start using InSpec. To show an example of InSpec tests, we’ll write tests that will check that the Azure infrastructure we provisioned with Terraform in _Chapter 2, Provisioning Cloud Infrastructure with Terraform_, is compliant with the specifications of our Azure infrastructure, which must be composed of the following:

- One resource group named `bookRg`
- One **virtual network (VNet)** with one subnet inside it named `book-subnet`
- One **virtual machine (VM)** named `bookvm`

As a first step in writing our InSpec tests, we’ll create an InSpec profile file.

## Creating an InSpec profile file

To create an InSpec profile file, we’ll generate the test directory structure, then modify the InSpec profile file that was generated.

To perform this manipulation, we create a test folder structure and an InSpec profile that defines some metadata and the InSpec configuration. To create a structure and an InSpec profile file, on your machine, go to the directory of your choice and execute the following command:

```
inspec init profile azuretets
```

This command initializes a new profile by creating a new folder, azuretets, that contains all of the artifacts needed for InSpec tests, with the following:

- Controls (tests)
- Libraries
- A profile file, `inspec.yml`, with some default metadata

Then, we modify this `inspec.yml` profile file with some personal metadata and add the **Uniform Resource Locator (URL)** link from the InSpec-Azure resource pack to the sample code, as follows:

```
name: azuretets
title: InSpec Profile
maintainer: Your name
copyright: Your name
copyright_email: you@example.com
license: All Rights Reserved
summary: An InSpec Compliance Profile
version: 0.1.0
inspec_version: '>= 4.6.9'
depends:
- name: inspec-azure
url: https://github.com/inspec/inspec-azure.git
```

In this code, we have entered some personal information, such as our name, and information about the license type of this code. Then, finally, in the last part of this file, we indicated a dependency with the URL of the InSpec-Azure resource library pack.

In fact, since version 2.2.7 of InSpec, we can use a set of InSpec libraries that use the Azure API and hence allow us to access all Azure resources. From there, the InSpec team creates an Azure resource pack that contains a lot of libraries to test a wide range of Azure resources such as Azure users, Azure Monitor, Azure networking (VNet and subnets), Azure SQL Server, Azure Virtual Machines (Azure VMs), and many other Azure resources.

> Note
> For a complete list of available Azure resources, refer to the InSpec documentation for the InSpec Azure resource pack:
> https://www.inspec.io/docs/reference/resources/#azure-resources

After generating our directories that contain the tests and profile file updates, we’ll write our infrastructure compliance tests.

## Writing compliance InSpec tests

To answer our example specification, we’ll write tests that will verify our provisioned infrastructure contains a resource group, VM, and subnet.

All of the tests we’ll be drafting are located in the controls folder and are written in a Ruby file (.rb) with very simple code that’s human-readable.

To begin, we’ll write a test that checks the existence of the resource group, and for this, we’ll delete the example.rb example file in the controls folder, which is an example of the tests provided in the test templates, and create a new file named resourcegroup.rb that contains the following code content:

```
control 'rg' do
 describe azure_resource_groups do #call the azurerm_resource_groups of Azure Resource Pack
 its('names') { should include 'bookRg' } #test assert
 end
end
```

