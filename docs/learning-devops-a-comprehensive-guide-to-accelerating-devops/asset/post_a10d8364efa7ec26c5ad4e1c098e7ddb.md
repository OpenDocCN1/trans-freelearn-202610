

# So, here is the execution of Terraform:

![](img/d3344b39c888aab0b832625465f837c9_88_0.png)

Figure 2.24 – The terraform init command with backend configuration

In this execution, we can view the export of the ARM_ACCESS_KEY environment variable, along with the Terraform init command that determines the backend configuration with the -backend-config option.

With this remote backend, the state file will no longer be stored locally but on a storage account, which is a shared space. Therefore, it can be used at the same time by several users. At the same time, this storage account offers security to protect the sensitive data of the state file and the possibility of any backups or restorations of the state files, which are both essential and critical elements of Terraform, too.

> **Note**
>
> The entire source code of this chapter is available at `https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP02`, and the final Terraform code is located inside the `terraform_vars_interp` folder.

## Summary

In this chapter dedicated to Terraform, we learned that its installation can be done either manually or using scripts.

To apply Terraform, we detailed the different steps of its configuration to provision an Azure infrastructure using an Azure SP.

Additionally, we explained, step by step, its local execution with its main command lines, which are `init`, `plan`, `apply`, and `destroy`, along with its life cycle in a CI/CD process. Finally, we ended this chapter by looking at the protection of the state file in an Azure remote backend.

Therefore, Terraform is a tool that is in line with the principles of IaC. The Terraform code is readable and understandable to users, and its execution integrates very well into a CI/CD pipeline that allows you to automatically provision a cloud infrastructure.

Throughout this book, we will continue to discuss Terraform, with additions about its use with Packer, Azure Kubernetes Services, and downtime reduction.

In the next chapter, we will explore the next step of the IaC, which is configuration management by using Ansible. We will cover its installation, its usage for configuring our provisioned virtual machine, and how to protect secrets with Ansible Vault.

## Questions

- 1. What is the language used by Terraform?
- 2. What is Terraform’s role?
- 3. Is Terraform a scripting tool?
- 4. Which command allows you to display the installed version?
- 5. When using Terraform for Azure, what is the name of the Azure object that connects Terraform to Azure?
- 6. What are the three main commands of the Terraform workflow?
- 7. Which Terraform command allows you to destroy resources?
- 8. What is the option added to the `apply` command to automate the application of infrastructure changes?
- 9. What is the purpose of the Terraform state file?
- 10. Is it a good practice to leave the Terraform state file locally? If not, what should be done?

## Further reading

If you want to know more about Terraform, here are some resources:

- The official Terraform documentation: https://www.terraform.io/
- Terraform download and installation information: https://www.terraform.io/downloads.html
- Terraform Azure provider: https://www.terraform.io/docs/providers/azurerm/index.html
- The official Azure documentation for Terraform: https://docs.microsoft.com/en-us/azure/terraform/terraform-overview
- Book: Terraform Cookbook: https://www.packtpub.com/product/terraform-cookbook/9781800207554
- Getting Started with Terraform, Second Edition: https://www.packtpub.com/networking-and-servers/getting-started-terraform-second-edition
- Online learning for Terraform: https://learn.hashicorp.com/terraform

## 3 Using Ansible for Configuring IaaS Infrastructure

In the previous chapter, we talked about provisioning an Azure cloud infrastructure with Terraform. If this infrastructure contains **virtual machines** (VMs), once they’ve been provisioned, it is necessary to configure their systems and install all the necessary middleware. This configuration will be necessary for the proper functionality of the applications that will be hosted on the VM.

There are several **Infrastructure as Code** (IaC) tools available for configuring VMs and the most well-known are Ansible, Puppet, Chef, SaltStack, and PowerShell DSC. Among them, Ansible from Red Hat (`https://www.ansible.com/overview/it-automation`) stands out for its many assets, as follows:

- It is declarative and uses the easy-to-read YAML language.
- Ansible only works with one executable.
- It does not require agents to be installed on the VMs to be configured.
- A simple SSL/WinRM connection is required for Ansible to connect to remote VMs.
- It has a template engine and a vault to encrypt/decrypt sensitive data.
- It is idempotent.

The main uses cases of Ansible are as follows:

- Configuring a VM with middleware and hardening, which we will learn about in this chapter
- Infrastructure provisioning, such as Terraform, but using YAML configuration
- Security compliance to test that the system or network configuration conforms to the enterprise requirements

In this chapter, we will learn how to install Ansible, and then use it to configure a VM with an inventory and a playbook. We will also learn how to protect sensitive data with Ansible Vault before discussing how to use a dynamic inventory in Azure.

The following topics will be covered in this chapter:

- Installing Ansible
- Creating an Ansible inventory
- Executing the first playbook
- Executing Ansible
- Protecting data with Ansible Vault
- Using a dynamic inventory for an Azure infrastructure

### Technical requirements

To follow along with the chapter, you must meet the following technical requirements:

- To install Ansible, we need an OS such as Red Hat, Debian, CentOS, macOS, or any of the BSDs. For those who have Windows, you can install the **Windows Subsystem for Linux (WSL)**; refer to the documentation at `https://docs.microsoft.com/en-us/windows/wsl/install-win10`.
- Python 2 (version 2.7) or Python 3 (version 3.5+) must be installed on the machine that runs Ansible. You can download it here: `https://www.python.org/downloads/`. For more information about the Ansible requirements, refer to the documentation here: `https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#control-node-requirements`.
- An Ansible playbook uses YAML configuration files, so any code editor would work; however, we will be using Visual Studio Code as it is very suitable. You can download it here: https://code.visualstudio.com/.
- Most of this chapter will not focus on a particular cloud provider, except for the last section on Azure. We will need an Azure subscription for this, which we can get for free from here: https://azure.microsoft.com/en-us/free/.
- To run the Ansible dynamic inventory for Azure, we need to install the Azure Python SDK: https://docs.microsoft.com/en-us/azure/python/python-sdk-azure-install?view=azure-python.
- The complete source code for this chapter is available here: https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP03.

Check out the following video to see the Code in Action:
https://bit.ly/3HdwbVc.

## Installing Ansible

Before we start using Ansible, we must know which OS we can use it on and how to install and configure it. Then, we must learn about some of the concepts surrounding the artifacts that it needs to operate.

In this section, we will look at how to install Ansible on a local or server machine and how to integrate Ansible in Azure Cloud Shell. Then, we will talk about the different elements or artifacts that make up Ansible. Finally, we will configure Ansible.

To get started, we will learn how to download and install Ansible with an automatic script.

## Installing Ansible with a script

Unlike Terraform, Ansible is not multiplatform and can only be installed on Red Hat, Debian, CentOS, macOS, or any of the BSDs. You install it using a script that differs according to your OS.

For example, to install the latest version on Ubuntu, we must run the following script in a Bash Terminal:

```bash
sudo apt-get update
sudo apt-get install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt-get install ansible
```

