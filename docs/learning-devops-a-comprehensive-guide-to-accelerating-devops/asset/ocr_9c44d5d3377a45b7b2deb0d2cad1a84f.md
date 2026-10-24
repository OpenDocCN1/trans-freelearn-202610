Nexus is a high-performance and widely used enterprise repository, but it requires effort to install and maintain it. This is not the case for SaaS package managers such as Azure Artifacts, which we’ll look at now.

# # Azure Artifacts

Azure Artifacts is one of the services provided by Azure DevOps. We already looked at it in the previous chapter, and we will also cover it again later, in the _Using Azure Pipelines for CI/CD_ section of this chapter. It is hosted in the cloud, and therefore allows private package feeds to be managed.

The packages supported today are NuGet, npm, Maven, Gradle, Python, and also universal packages. The main difference compared with Nexus is that in Azure Artifacts, the feed is not by package type, and one feed can contain different types of packages.

One of the advantages of Azure Artifacts is that it is fully integrated with other Azure DevOps services such as Azure Pipelines, which allows for the management of CI/CD pipelines, as we will see shortly.

In Azure Artifacts, there is also a type of package called a **universal package** that allows the storing of all types of files (called a **package**) in a feed that can be consumed by other services or users.

Here is an example of an Azure Artifacts feed containing several types of packages, in which we can see one NuGet package, one npm package, one universal package, one Maven package, and one Python package:

![](bbox=[0.162, 0.58, 0.838, 0.882])

Figure 7.4 – Azure Artifacts packages