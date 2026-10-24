4. Finally, in the Azure portal, we can see the generated VM image in the Azure resource group.

![](bbox=[0.143, 0.145, 0.869, 0.453])

Figure 8.5 – The Packer image on Azure

In this section, we learned how to create Packer images automatically using pipelines in Azure DevOps. In the next section, we will continue with the IaC pipeline to execute Terraform and Ansible commands inside automatic pipelines.

# # Running Terraform and Ansible in Azure Pipelines

After the creation of the Packer pipeline, we will now create the pipeline for provisioning an Azure VM that uses the image created with Packer and then configures this VM with Ansible. For this demonstration, we will use Ansible to install nginx on this VM.

The Terraform configuration that is used in this code will provision new Azure resource group, a virtual network with a subnet, and a Linux VM with the tag role as the webserver value.

This complete Terraform code is available here: `https://github.com/ PacktPublishing/Learning-DevOps-Second-Edition/tree/main/ CHAP08/terraform`. I will not explain it, since it’s the same code that we learned in _Chapter 2, Provisioning Cloud Infrastructure with Terraform_.