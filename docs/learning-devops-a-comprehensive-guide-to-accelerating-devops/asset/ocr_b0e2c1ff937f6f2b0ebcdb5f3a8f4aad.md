# # The Terraform life cycle within a CI/CD process

So far, we have seen and executed, _on the local machine_, the various Terraform commands
that allow us to initialize, preview, apply, and destroy infrastructure and to format and
validate Terraform code. When using Terraform locally, in a development context, the
execution life cycle is as follows:

![](bbox=[0.342, 0.215, 0.651, 0.484])

Figure 2.21 – The Terraform CICD process

The following steps explain the sequence in the preceding diagram:

+   1. Code development
+   2. Code formatting with `terraform fmt`
+   3. Initialization with `terraform init`
+   4. Code validation with `terraform validate`
+   5. Planning with `terraform plan`
+   6. Manual verification of Terraform changes on the infrastructure

However, IaC, similar to an application, must be deployed or executed in an **automatic
CI/CD process**. This begins with the archiving of the Terraform code of the team
members. Then, it triggers the CI and executes the Terraform commands that we have
studied in this chapter.

The following is a screenshot of the Terraform life cycle in CI/CD automation: