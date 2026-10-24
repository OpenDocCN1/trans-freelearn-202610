# # Summary

In this chapter dedicated to Terraform, we learned that its installation can be done either manually or using scripts.

To apply Terraform, we detailed the different steps of its configuration to provision an Azure infrastructure using an Azure SP.

Additionally, we explained, step by step, its local execution with its main command lines, which are `init`, `plan`, `apply`, and `destroy`, along with its life cycle in a CI/CD process. Finally, we ended this chapter by looking at the protection of the state file in an Azure remote backend.

Therefore, Terraform is a tool that is in line with the principles of IaC. The Terraform code is readable and understandable to users, and its execution integrates very well into a CI/CD pipeline that allows you to automatically provision a cloud infrastructure.

Throughout this book, we will continue to discuss Terraform, with additions about its use with Packer, Azure Kubernetes Services, and downtime reduction.

In the next chapter, we will explore the next step of the IaC, which is configuration management by using Ansible. We will cover its installation, its usage for configuring our provisioned virtual machine, and how to protect secrets with Ansible Vault.

# # Questions

+   1. What is the language used by Terraform?

+   2. What is Terraform’s role?

+   3. Is Terraform a scripting tool?

+   4. Which command allows you to display the installed version?

+   5. When using Terraform for Azure, what is the name of the Azure object that connects Terraform to Azure?

+   6. What are the three main commands of the Terraform workflow?

+   7. Which Terraform command allows you to destroy resources?

+   8. What is the option added to the `apply` command to automate the application of infrastructure changes?

+   9. What is the purpose of the Terraform state file?

+   10. Is it a good practice to leave the Terraform state file locally? If not, what should be done?