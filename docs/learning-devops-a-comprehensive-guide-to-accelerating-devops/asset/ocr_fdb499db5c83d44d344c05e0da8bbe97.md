# # Testing Azure infrastructure compliance with Chef InSpec

One of the important practices of DevOps culture is IaC, detailed in _Chapter 1, The DevOps Culture and Infrastructure as Code Practices_, which consists of coding the configuration of an infrastructure and then being automatically deployed via CI/CD pipelines. IaC allows cloud infrastructure to be deployed and provisioned very quickly, but the question that often arises is: _Does the automatically provisioned infrastructure meet functional compliance and security requirements?_

To answer this question, we'll have to write and automate infrastructure tests that will verify the following:

+   - The infrastructure deployed corresponds well to the application and enterprise architecture specifications.
+   - The company's security policies are properly applied to the infrastructure.

These tests can be written in any scripting language that can interact with our cloud provider, and if we have an Azure subscription, we can use—for example—the Azure **command-line interface (CLI)** or Azure PowerShell commands to code the tests of our Azure resources. Also, if we use PowerShell, we can use **Pester** (`https://pester.dev/docs/quick-start/#what-is-pester`), which is a library that allows us to perform PowerShell tests and, combined with Azure PowerShell, allows us to perform infrastructure compliance tests.

> > **Note**
> > To get an example of how to use Pester to test an Azure infrastructure, I suggest you read this article: `https://dzone.com/articles/azure-security-audits-with-pester`. Also, have a look at this blog post: `https://dev.to/omiosec/unit-testing-in-powershell-introduction-to-pester-1de7`.

The problem with these scripting tools is that they require a lot of code to be written. Also, these tools are dedicated to a specified cloud provider, and they require learning a new scripting language.

One of the IaC tools is **InSpec** (`https://www.inspec.io/`), which performs infrastructure compliance tests.

In this section, we'll see in detail the use of InSpec to test the compliance of Azure infrastructure and, to start with its implementation, I will provide you with an overview of InSpec.