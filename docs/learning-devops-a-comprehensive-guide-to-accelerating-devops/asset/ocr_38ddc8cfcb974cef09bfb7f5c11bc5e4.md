```
AZURE_CLIENT_ID: $(AZURE_CLIENT_ID)
AZURE_SECRET: $(AZURE_SECRET)
AZURE_SUBSCRIPTION_ID: $(AZURE_SUBSCRIPTION_ID)
AZURE_TENANT: $(AZURE_TENANT)
ANSIBLE_HOST_KEY_CHECKING: False
```

In this code, the first script installs the Azure plugins for Ansible that are required
for the dynamic inventory.

The second script executes an Ansible playbook and uses the dynamic inventory
configured in the `inv.azure_rm.yml` file. We also use the Azure service
principal credentials as environment variables.

Then, commit and push this file in your Git repository.

The last step is to create and run the Azure pipeline by following these steps:

+   1. In the Azure Pipelines menu, click on **New pipeline**.

+   2. Select the Git repository that contains Packer templates and the YAML pipeline file,
+   and choose the option to use the existing YAML pipeline file.

All the details for this step are explained in the *Creating the full pipeline definition in
a YAML file* section of *Chapter 7, Continuous Integration and Continuous Delivery*.

+   3. After creating the pipeline, inside **Edit** mode, add five variables to store the
+   credentials information of the Azure service principal and the Azure storage access
+   key for the Terraform state backend.

![](bbox=[0.283, 0.59, 0.691, 0.851])

Figure 8.6 – Azure Pipelines Terraform and Ansible variables

These are the variables we used in the YAML pipeline in all of the script steps.