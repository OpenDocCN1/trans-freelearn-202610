![](bbox=[0.1172, 0.0898, 0.8516, 0.3477])

# Figure 9.28 – Bash step parameters

# The configured script looks like this:

```
```
export ARM_CLIENT_ID="$ (ARM_CLIENT_ID) "
export ARM_CLIENT_SECRET="$ (ARM_CLIENT_SECRET) "
export ARM_TENANT_ID="$ (ARM_TENANT_ID) "
export ARM_SUBSCRIPTION_ID="$ (ARM_SUBSCRIPTION_ID) "
terraform init -backend-config="backend.tfvars"
terraform apply -var "imageversion=$(Build.BuildNumber)" 
-var "dockerhub-username=$(dockerhub_Username) " --auto-
approve
```
```

# This script performs three actions, which are done in the following order:

+   A. Exports the environment variables required for Terraform.

+   B. Executes the `terraform init` command.

+   C. Executes `terraform apply` to apply the changes, with the two `-var` parameters, which are our Docker Hub username as well as the tag to be applied. These parameters allow the execution of a container with the new image that has just been pushed to Docker Hub.