Create the bucket and enable versioning:

```
```
$ gsutil mb gs://${PROJECT_ID}
$ gsutil versioning set on gs://${PROJECT_ID}
```
```

> Note
> 
> By enabling the versioning capability, we are asking GCS to maintain older versions of objects while newer objects are saved. The bucket name must also be unique throughout Google Cloud, not just within your project.

The Terraform state (managed within GCS) is also different from the Terraform configuration (for example, .tf or .tfvars), which are typically stored in an SCM for version control.

Once we have our bucket set up, before we start using Cloud Build to automate our infrastructure pipeline, let's do a quick test to spin up a Google Compute Engine (GCE) virtual machine (VM).

We'll start with some quick files to get Terraform started.

> Note
> 
> The examples include syntax and versions that worked during the writing of this book. However, changes may need to be made depending on the version of the Terraform CLI tool and Terraform provider for Google Cloud.
> 
> In this example, we are using the environment variable, PROJECT_ID, that we stored in the previous command to generate the following backend.tf file. Terraform does not allow us to use Terraform variables to dynamically specify the backend bucket used for storing the state.

We will reference the bucket that was created in the previous command, to tell terraform where to store and retrieve the state:

```
```
$ mkdir ~/packt/cloudbuild-terraform; cd ~/packt/cloudbuild-terraform
cat << EOF > backend.tf
terraform {
  backend "gcs" {
    bucket     = "${PROJECT_ID}"
    prefix     = "tfstate"
  }
}
EOF
```
```