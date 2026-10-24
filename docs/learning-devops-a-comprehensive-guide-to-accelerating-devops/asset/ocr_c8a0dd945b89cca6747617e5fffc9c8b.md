The complete Ansible code that we used to install nginx on the Linux machine is available
here: https://github.com/PacktPublishing/Learning-DevOps-Second-
Edition/blob/main/CHAP08/ansible/playbookdemo.yml. We already
learned about it in Chapter 3, Using Ansible for Configuring IaaS Infrastructure. For the
Ansible inventory, we use a **dynamic inventory** that selects VM hosts based on a specific
Azure resource group and a tag on a VM.

The configuration code of this dynamic inventory is available here: https://github.
com/PacktPublishing/Learning-DevOps-Second-Edition/blob/main/
CHAP08/ansible/inv.azure_rm.yml.

Now that we have all the Terraform and Ansible code, we can write the YAML code of the
Azure Pipelines with the following steps. For this, create a new azure-pipeline.yml
file with the following content:

+   1. The first step is to run Terraform workflow commands with `init`, `plan`,
+   and `apply`:

```
```
- script: terraform init --backend-config backend.
tfvars
    displayName: Terraform init
    workingDirectory: $(Build.SourcesDirectory)/CHAP08/
    terraform
    env:
        ARM_CLIENT_ID: $(AZURE_CLIENT_ID)
        ARM_CLIENT_SECRET: $(AZURE_SECRET)
        ARM_SUBSCRIPTION_ID: $(AZURE_SUBSCRIPTION_ID)
        ARM_TENANT_ID: $(AZURE_TENANT)
        ARM_ACCESS_KEY: $(AZURE_ACCESS_KEY)
- script: terraform plan
    displayName: Terraform plan
    workingDirectory: $(Build.SourcesDirectory)/CHAP08/
    terraform
    env:
        ARM_CLIENT_ID: $(AZURE_CLIENT_ID)
        ARM_CLIENT_SECRET: $(AZURE_SECRET)
        ARM_SUBSCRIPTION_ID: $(AZURE_SUBSCRIPTION_ID)
        ARM_TENANT_ID: $(AZURE_TENANT)
        ARM_ACCESS_KEY: $(AZURE_ACCESS_KEY)
```
```