# Setting up our first Consul cluster

The hour arrived! At the beginning of this chapter, there were instructions for creating credentials for an AWS account, as well as for downloading Terraform. Now, you finally get to put all of that work to good use!

> **Note**
> 
> This book covers aspects of Consul for both server and client operations. HashiCorp also offers a managed Consul service on **HashiCorp Cloud Platform (HCP)**, which may be desirable when deploying within your own production environment. However, in order to understand the server operation, we'll be operating on our own. For more information about HashiCorp’s hosted Consul service, visit `https://portal.cloud.hashicorp.com/`.

All of the code for this section is in the following GitHub repository:

https://github.com/PacktPublishing/Simplifying-Service-Management-with-Consul

You can either fork the repository or just download it and play along. We're going to be building our system for each chapter with code in that repository, with the relevant code in the appropriate chapter folder.

The following sections include a review of the Packer and Terraform code that we'll be using to create our Consul cluster. It isn't necessary to understand every aspect of the code, but I wanted to include explanations for completeness. As you proceed, however, take careful note of the commands indicated by the `$` symbol.

# Building the image

We're going to start by building our base image. In order to keep us all on the same page—quite literally—regarding the image being used and the operation of Consul, we're going to be building an image together using Packer. But don't worry—all of the code you need is in the repository. Within the ch2 folder, you'll find another folder called Image-Creation. I know—very inventive. Here's the code you'll need to get to this folder:

```
```sh
$ cd $github_root/Consul-Up-and-Running/ch2/Image-Creation
```
```

There are a few key items in here to be aware of. Your variables for the build are all defined in `variables.pkr.hcl`, as illustrated in the following code snippet:

```
```hcl
variable "owner" {
  type        = string
```
```