There are a couple of examples of tools like this:

+   - Skaffold (https://skaffold.dev/) is a Kubernetes continuous development tool that is meant for Kubernetes users to run both in their local environment and further in their software delivery pipeline. To do this, users must execute the `skaffold` binary from the same directory in which there is a `skaffold.yaml` configuration file.
+   - Terraform (https://www.terraform.io/) is an infrastructure-as-code tool that is meant for users to utilize to create resources across multiple infrastructure providers using a consistent language. Usually, the `terraform` CLI should be executed from the working directory in which there are `.tf` configuration files, though it also has support for passing in `--chdir` as an argument that allows you to change the working directory.

For these scenarios, users can utilize the `dir` field to automatically set the working directory for a specific build step. There are two options in which you can use dir:

+   - Relative path
+   - Absolute path

Specifying `dir` as a relative path is the more common use case. Let's create a build that continues the work we did in our prior build and runs a couple of build steps in a new `cloudbuild.yaml` file:

```
$ cd .. && mkdir generate-validate-terraform && cd generate-validate-terraform
$ PROJECT_ID=$(gcloud config get project)
$ ZONE=us-central1-a
$ cat > cloudbuild.yaml EOF<<
steps:
- name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
  id: 'Generate terraform for existing VMs'
  entrypoint: 'sh'
  args:
  - '-c'
  - |
    apt-get install google-cloud-sdk-config-connector
    gcloud beta resource-config bulk-export \
    --resource-format=terraform \
    --resource-types=ComputeInstance \
    --project=$PROJECT_ID \
    --path=tf
- name: 'hashicorp/terraform:1.0.0'
```