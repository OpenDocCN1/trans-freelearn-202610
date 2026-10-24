# ## 2. Then, we'll execute Terraform with these commands:

```
terraform init
terraform plan
terraform apply
```

Here is a quick summary of the details of these commands:

+   - The `terraform init` command initializes the context and downloads all necessary providers.

+   - The `terraform plan` command displays a preview of all changes that will be applied by Terraform.

+   - The `terraform apply` command applies all changes on the infrastructure and displays the output values.

> > **Note**
> > To learn all the details of the main Terraform commands and the Terraform life cycle, read _Chapter 2_ of this book, _Provisioning Cloud Infrastructure with Terraform_.

The following screenshot shows the execution of the `apply` command:

![](bbox=[0.118, 0.524, 0.85, 0.83])

Figure 14.16 – Terraform sensitive output