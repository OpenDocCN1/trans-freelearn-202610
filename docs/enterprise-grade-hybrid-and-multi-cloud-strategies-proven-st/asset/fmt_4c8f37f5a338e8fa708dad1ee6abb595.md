
- **Hybrid cloud flexibility**: Azure Stack supports a hybrid cloud strategy, allowing organizations to run workloads locally and in the Azure public cloud. This flexibility is valuable for scenarios where certain workloads need to remain on-premises due to regulatory requirements, data residency concerns, or specific performance needs.
- **Integrated security and compliance**: Azure Stack inherits the security features and compliance certifications of Azure. This ensures that workloads running on Azure Stack benefit from the same level of security controls, identity management, and compliance standards as those in the Azure public cloud.
- **App services for containers**: Azure Stack supports containers through **Azure Kubernetes Service** (**AKS**) and **Azure Container Instances** (**ACI**). This allows organizations to modernize applications using containerized architectures while maintaining the flexibility to run them on-premises.
- **Edge and disconnected scenarios**: Azure Stack is suitable for edge computing scenarios where low-latency processing is crucial. It allows organizations to deploy Azure services in remote locations or areas with limited connectivity, ensuring a consistent computing experience.

The following are some key considerations:

- **Hardware compatibility and requirements**: Deploying Azure Stack requires compatible hardware that meets Microsoft’s specifications. Organizations need to ensure that their existing infrastructure or new investments align with Azure Stack’s requirements.
- **Operational complexity**: While Azure Stack simplifies certain aspects of hybrid cloud operations, managing a hybrid environment can still be complex. Organizations need to invest in training and resources to effectively operate and maintain Azure Stack.
- **Cost considerations**: Organizations should carefully assess the TCO of deploying and maintaining Azure Stack. This includes hardware costs, licensing fees, and ongoing operational expenses.

## Summary

In this chapter, we discussed how enterprises with significant investments in their on-premises IT infrastructure can adopt cloud computing to accelerate and transform their business objectives. We explored how organizations can build the foundations of a hybrid cloud through an opinionated and vendor-agnostic framework, along with the core layers that constitute the framework. We also explored the cloud-native principles that organizations must consider while designing and developing modern applications in the cloud.

We then explored the different cloud architecture frameworks and the various pillars advocated by three major cloud service providers – AWS, Google Cloud, and Azure. Finally, we discussed some of the unique private cloud and hybrid cloud strategies and offerings that can be adopted by enterprises.

In the next chapter, we will discuss the various facets of digital transformation.

## Further reading

To learn more about the topics that were covered in this chapter, take a look at the following references:

1. [https://www.mckinsey.com/~/media/McKinsey/Industries/Electric%20Power%20and%20Natural%20Gas/Our%20Insights/Traditional%20company%20new%20businesses%20The%20pairing%20that%20can%20ensure%20an%20incumbents%20survival/Traditional-company-new-businesses-VF.pdf](https://www.mckinsey.com/~/media/McKinsey/Industries/Electric%20Power%20and%20Natural%20Gas/Our%20Insights/Traditional%20company%20new%20businesses%20The%20pairing%20that%20can%20ensure%20an%20incumbents%20survival/Traditional-company-new-businesses-VF.pd)
2. [https://landscape.cncf.io/](https://landscape.cncf.io/)
3. [https://martinfowler.com/bliki/StranglerFigApplication.html](https://martinfowler.com/bliki/StranglerFigApplication.html)
4. [https://www.seagate.com/files/www-content/our-story/trends/files/idc-seagate-dataage-whitepaper.pdf](https://www.seagate.com/files/www-content/our-story/trends/files/idc-seagate-dataage-whitepaper.pdf)
5. [https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
6. [https://cloud.google.com/architecture/framework](https://cloud.google.com/architecture/framework)
7. [https://azure.microsoft.com/en-us/solutions/cloud-enablement/well-architected/#reliability](https://azure.microsoft.com/en-us/solutions/cloud-enablement/well-architected/#reliability)

# 9 Facets of Digital Transformation

When is the right time to develop a cloud strategy? For some enterprises, the answer is as we adopt the cloud, while for several others, it is still unclear. In its *Cloud Strategy Cookbook*, Gartner advises organizations to develop a cloud strategy even before embarking on a cloud transformation initiative *\[1\]*. The cookbook recognizes the fact that a one-size-fits-all approach to the cloud is insufficient, considering the diverse needs and business objectives of modern-day enterprises. As modern technology continues to blur the line between business and IT, it is important to align the organization’s cloud strategy closely with business objectives, to maximize the value derived from cloud investments and drive tangible outcomes. As organizations continue to adopt cloud computing –regardless of the deployment model, IaaS, PaaS, or SaaS – it is crucial to design and develop a digital strategy that will serve as the foundation for future business goals. The tools, processes, and technologies that supported a traditional IT ecosystem predominantly within the confines of a data center can no longer support the scale, diversity, complexity, and needs of a cloud-based ecosystem. Embarking on a cloud journey brings a paradigm shift to enterprise IT across functions. One fundamental example is the shared responsibility model of cloud where the cloud provider assumes the responsibility of managing and securing a portion of the stack while the consumer is responsible for the rest. This in itself is a major shift in enterprise IT operations, as the organization would now have little or no control over a significant portion of the infrastructure or application stack. Further, this also means that top-of-the-stack responsibility is guided and restricted as well, with a defined framework. For instance, most organizations today have some variant of a SaaS application in the IT ecosystem – think Microsoft 365 for office apps, Google Workspace for online productivity, Workday for HR, or Salesforce for CRM. All of these applications run within a cloud environment that is beyond the control of a traditional IT organization. This means that the organization has just expanded its IT footprint beyond the confines of the traditional data center, warranting a robust strategy for network connectivity, integration, application security, data security, and governance.

In this chapter, we will explore the various facets of digital transformation that can help set the foundation and tone for an organization’s cloud strategy. Specifically, we will explore the following topics:

- How enterprises can build a security-first cloud foundation to ensure overall enterprise security including network, data, application, and asset management
- Learning to rethink infrastructure in a hybrid cloudscape with a focus on infrastructure modernization
- Strategies for building cloud-native applications with a focus on application modernization
- Leveraging cloud-native design to modernize data ecosystem
- Learning how to build a cloud governance model to optimize cloud infrastructure for costs, performance, and resource efficiency and maximize the value of cloud investments

## Building a security-first cloud architecture
