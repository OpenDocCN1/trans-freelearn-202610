

In the last section, we saw how to protect sensitive data with **Vault** from HashiCorp. In this section, we looked at data encryption and decryption in Vault and wrote Terraform code that will dynamically retrieve the secrets stored in Vault.

In the next chapter, we'll present the concept of **blue-green deployment** with its patterns for reducing deployment downtime. Then, we'll learn how to implement it in an application as well as in the deployment of Azure infrastructure.

## Questions

- 1. What is the role of InSpec?
- 2. What is the name of the package manager that allows you to download InSpec via the command line?
- 3. Which InSpec command allows you to execute InSpec tests?
- 4. Who is the publisher of Vault?
- 5. Which command starts Vault in development mode?
- 6. When Vault is installed locally, can it be used for production?
- 7. In local mode, where is Vault's encrypted data stored?

## Further reading

If you want to know more about DevSecOps with InSpec, Vault, and **Secure DevOps Kit for Azure (AzSK)**, here are some resources:

- InSpec documentation: `https://www.inspec.io/`
- HashiCorp Vault documentation: `https://www.vaultproject.io/docs/`
- Learn about HashiCorp Vault: `https://learn.hashicorp.com/vault`

# 15 Reducing Deployment Downtime

So far in this book, we have discussed **DevOps** practices such as **Infrastructure as Code** **(IaC)**, **continuous integration/continuous deployment (CI/CD)** pipelines, and the automation of different types of tests.

In _Chapter 1, The DevOps Culture and Infrastructure as Code Practices_, we saw that these DevOps practices will improve the quality of applications and thus improve the financial gain of a company. We will now go deeper into DevOps practices by looking at how to ensure the continuous availability of your applications even during your deployments, and how to deliver new versions of these applications more frequently in production.

Often, what we see is that deployments require your applications to be interrupted by—for example—infrastructure changes or service shutdowns. Moreover, what we also see is that companies are still reluctant to deliver more frequently in production. They are not equipped to test the application in the production environment, or they are waiting for other dependencies.

In this chapter, we will look at several practices that will help you improve application delivery processes. We'll start with a way to reduce the downtime of your infrastructure and applications during Terraform deployments. Then, we will discuss the concept and patterns of blue-green deployment and how to configure it with some Azure resources. Finally, we will present the details of implementing a feature flag in your application, which will allow you to modify the operation of an application without having to redeploy it in production.

You will also learn how to configure Terraform code to reduce application downtime. You'll be able to configure Azure resources with blue-green deployment and implement feature flags in your applications with either an open source component or the LaunchDarkly platform.

In this chapter, we will cover the following topics:

- Reducing deployment downtime with Terraform
- Understanding blue-green deployment concepts and patterns
- Applying blue-green deployments on Azure
- Introducing feature flags
- Using an open source framework for feature flags
- Using the LaunchDarkly solution

## Technical requirements

In order to understand the Terraform concepts that will be presented in this chapter, you need to have read _Chapter 2, Provisioning Cloud Infrastructure with Terraform_.

We will look at an example of how to implement blue-green deployment in Azure. If you don't have an Azure subscription, you can create a free Azure account here: `https://azure.microsoft.com/en-gb/free/`.

Then, we will look at an example of how to use feature flags in an ASP.NET Core application. To use our example, you will need to install the **.NET Core software development kit (SDK)**, which can be downloaded from `https://dotnet.microsoft.com/download`.

For code editing, we used the free **Visual Studio Code (VS Code)** editor, which is available for download here: `https://code.visualstudio.com/`.

The complete source code for this chapter can be found at `https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP15`.

Check out the following video to see the Code in Action:
https://bit.ly/3hcvrVH

## Reducing deployment downtime with Terraform

In Chapter 2, Provisioning Cloud Infrastructure with Terraform, we detailed the use of Terraform by looking at its commands and life cycle and put it into practice with an implementation in Azure.

One of the problems with Terraform is that, depending on the infrastructure changes that need to be implemented, Terraform may automatically destroy and rebuild certain resources.

To fully understand this behavior, let's look at the output of this following Terraform execution, which provisioned an Azure Web App in Azure and has been modified with a name change:

![](img/d3344b39c888aab0b832625465f837c9_465_0.png)

Figure 15.1 – Terraform downtime

Here, we can see that Terraform will destroy the web app and then rebuild it with the new name. Although destruction and reconstruction are done automatically, while Terraform is destroying and rebuilding the web app, the application will be inaccessible to users.

To solve this problem of downtime, we can add the Terraform create_before_destroy option, as follows:

```hcl
resource "azurerm_app_service" "webapp" {
  name = "MyWebAppBook1" #new name
  location = "West Europe"
  resource_group_name = "${azurerm_resource_group.rg-app.name}"
  app_service_plan_id = "${azurerm_app_service_plan.serviceplan-app.id}"
  app_settings = {
    WEBSITE_RUN_FROM_PACKAGE = var.package_zip_url
  }
  lifecycle { create_before_destroy = true }
}
```

By adding this option, Terraform will do the following:

- First, Terraform creates a new web app with a new name.
- During the provisioning of the new web app, it uses the **Uniform Resource Locator (URL)** for the application package in ZIP format that's provided in the app_settings property. Use WEBSITE_RUN_FROM_PACKAGE to launch the application.
- Then, Terraform will destroy the old web app.

Using the Terraform create_before_destroy option will ensure the viability of our applications during deployments.

However, be careful, as this option will only be useful if the new resource that's being created allows us to have the application running very quickly at the same time as it's provisioning so that a service interruption doesn't occur.

In our example of a web app, this worked when we used the WEBSITE_RUN_FROM_PACKAGE property of the web app. For a **virtual machine (VM)**, we can use a VM image created by **Packer**. As we saw in _Chapter 4, Optimizing Infrastructure Deployment with Packer_, Packer contains information regarding the VM applications that have already been updated inside the VM image.

> Note
> For more information on the `create_before_destroy` option, please view the following Terraform documentation: https://www.terraform.io/language/meta-arguments/lifecycle.

We have just seen that, with Terraform and IaC, it is possible to reduce downtime during deployments in the case of resource changes.

We will now look at the concepts and patterns of a practice called **blue-green deployment**, which allows us to deploy and test an application in production with great confidence.

## Understanding blue-green deployment concepts and patterns

Blue-green deployment is a practice that allows us to deploy a new version of an application in production without impacting the current version of the application. In this approach, the production architecture must be composed of **two identical environments**; one environment is known as the **blue** environment while the other is known as the **green** environment.

The element that allows routing from one environment to another is a router—that is, a load balancer.

The following diagram shows a simplified schematic of a blue-green architecture:

![](img/d3344b39c888aab0b832625465f837c9_467_0.png)

Figure 15.2 – Blue-green architecture

As we can see, there are two identical environments—the environment called **blue**, which is the current version of the application, and the environment called **green**, which is the new version or the next version of the application. We can also see a router, which redirects users' requests either to the blue environment or the green environment.

