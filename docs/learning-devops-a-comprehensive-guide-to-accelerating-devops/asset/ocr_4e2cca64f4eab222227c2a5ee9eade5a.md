Check out the following video to see the Code in Action:
https://bit.ly/3hcvrVH

# # Reducing deployment downtime with Terraform

In Chapter 2, Provisioning Cloud Infrastructure with Terraform, we detailed the use of
Terraform by looking at its commands and life cycle and put it into practice with an
implementation in Azure.

One of the problems with Terraform is that, depending on the infrastructure changes
that need to be implemented, Terraform may automatically destroy and rebuild certain
resources.

To fully understand this behavior, let's look at the output of this following Terraform
execution, which provisioned an Azure Web App in Azure and has been modified with a
name change:

![](bbox=[0.1152, 0.4469, 0.8496, 0.7641])

Figure 15.1 – Terraform downtime

Here, we can see that Terraform will destroy the web app and then rebuild it with the new
name. Although destruction and reconstruction are done automatically, while Terraform
is destroying and rebuilding the web app, the application will be inaccessible
to users.