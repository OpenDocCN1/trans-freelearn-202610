This is why, for some time now, security has been included in the DevOps culture by becoming a **development-security-operations (DevSecOps)** culture more broadly. There is nothing outside the ambit of security. Since we are developing at rapid speeds, it makes ample sense to make security part of the process rather than outside it.

The DevSecOps culture or approach is, therefore, the union of developers and operations with the integration of security as early as possible in the implementation and design of projects. The DevSecOps approach is also the automation of compliance and security verification processes in CI/CD pipelines, to guarantee constant security and not slow down application deployment cycles.

Today, the DevOps culture must absolutely integrate security teams but also all security processes, whether on tools, infrastructure, or applications. This is to provide not only better quality but also more secure applications.

In this chapter, we'll focus on the DevSecOps approach. First, we'll see how to test the compliance of an Azure infrastructure using InSpec from Chef. Then, we'll learn how to protect all infrastructure and application secrets with Vault from HashiCorp.

This chapter will cover the following topics:

+   - Testing Azure infrastructure compliance with Chef InSpec
+   - Keeping sensitive data safe with HashiCorp Vault

# ## Technical requirements

In this chapter, we'll see the use of InSpec, which requires **Ruby** version 2.4 or later to be installed on the local machine. To install Ruby according to your specific **operating system** (OS), read this documentation: `https://www.ruby-lang.org/en/documentation/installation/`.

In the _Keeping sensitive data safe with HashiCorp Vault_ section, we'll discuss the integration between Vault and Terraform without looking into the details of Terraform, so I suggest you first read _Chapter 2, Provisioning Cloud Infrastructure with Terraform_.

The complete source code for this chapter is available here:

`https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP14`

Check out the following video to see the Code in Action:

`https://bit.ly/3In9kb2`