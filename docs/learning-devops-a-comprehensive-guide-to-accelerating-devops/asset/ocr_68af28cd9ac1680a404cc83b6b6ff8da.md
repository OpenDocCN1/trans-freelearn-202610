# # Protecting the state file with a remote backend

When Terraform handles resources, it writes the state of these resources in a Terraform state file. This file is in JSON format and preserves the resources and their properties throughout the execution of Terraform.

By default, this file, called `terraform.tfstate`, is created locally when the first execution of the `apply` command is executed. Then, it will be used by Terraform each time the `plan` command is executed in order to compare its state (written in the state file) with that of the target infrastructure. Finally, it will return a preview of what will be applied.

When using Terraform in an enterprise, this locally stored state file poses many problems:

+   - Knowing that this file contains the status of the infrastructure, it should not be deleted. If deleted, Terraform might not behave as expected when it is executed.
+   - It must be accessible at the same time by all members of the team who are handling resources on the same infrastructure.
+   - This file can contain sensitive data, so it must be secure.
+   - When provisioning multiple environments, it is necessary to be able to use multiple state files.

With all of these points, it is not possible to keep this state file locally or even to archive it in an SCM.

To solve this problem, Terraform allows this state file to be stored in a shared and secure storage called the **remote backend**.

> > **Important Note**
> > Terraform supports several types of remote backends; the full list is available at https://www.terraform.io/docs/backends/types/remote.html.

In our case, we will use an **azurerm remote backend** to store our state files with a storage account and a blob for the state file.

Therefore, we will implement and use a remote backend in three steps:

+   1. The creation of the storage account
+   2. The Terraform configuration for the remote backend
+   3. The execution of Terraform with the use of this remote backend