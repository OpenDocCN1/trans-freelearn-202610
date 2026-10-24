```
}
}
```

In addition to this, Terraform has a large list of built-in functions that can be used to
manipulate data or variables. To learn more about these functions, please refer to the
official documentation at `https://www.terraform.io/docs/configuration/
functions.html`.

Of course, there are many other good practices, but these are to be applied from the first
lines of code to ensure that your code is well maintained.

> **Important Note**
> 
> The complete and final code of this example is available at `https://
> github.com/PacktPublishing/Learning-DevOps-Second-
> Edition/tree/main/CHAP02/terraform_vars_interp`.

We have written a Terraform configuration, using best practices, which allows us to create
a simple cloud infrastructure in Azure that provides a network and a virtual machine.
Now, let's take a look at how to run Terraform with our code in order to provision this
infrastructure.

# # Running Terraform for deployment

With the Terraform configuration written, we now need to run Terraform to deploy
our infrastructure.

However, before any execution, first, it is necessary to provide authentication with the
Azure SP to ensure that Terraform can manage the Azure resources.

To do this, we can either set the environment variables specific to Terraform to contain the
information of the SP created earlier in the _Configuring Terraform for Azure_ section, or we
can use the `az cli` script.

The following script exports the four Terraform environment variables in the Linux OS:

```
export ARM_SUBSCRIPTION_ID=xxxxx-xxxxx-xxxxx-xxxx
export ARM_CLIENT_ID=xxxxx-xxxxx-xxxxx-xxxx
export ARM_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxx
x
export ARM_TENANT_ID=xxxxx-xxxxx-xxxxx-xxxx
```