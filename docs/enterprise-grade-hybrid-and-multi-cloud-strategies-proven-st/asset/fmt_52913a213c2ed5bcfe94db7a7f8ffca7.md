
*   Prescriptive cloud architecture frameworks from major cloud service providers – AWS, Google Cloud, and Azure
*   Private and hybrid cloud strategies

# Building a framework for a cloud-ready enterprise

Established enterprises typically have an existing data center with a significant infrastructure, technology, and software footprint and it is not practical to start on a clean slate. In the earlier chapters, we discussed how leadership plays a critical role in spearheading and chartering the course for a successful digital transformation and the importance of vision, will, and mindset shift. Now that we have established the need for an enterprise-wide digital transformation led by cloud technologies, it is important to redefine and architect the foundational building blocks to set up the IT organization for success. This brings us to the fundamental need for a prescriptive framework, regardless of the choice of a platform or provider. Several enterprises fall into the trap of adopting quick and dirty, one-off transformations that often fall flat due to the lack of a strong foundation, leading to undesirable outcomes in complexities, effort, and costs. For instance, consider Acme Corp., which invested in a modern data analytics solution without a strong data foundation, triggering huge complexities and costs in redesigning the data integration and semantic layers while still not having access to the right data to the right persona at the right time.

In this section, we will explore a provider-agnostic framework that helps redefine and architect a cloud-ready enterprise, regardless of the enterprise deployment platform. This is an opinionated framework that I have created, advocated, and prescribed for enterprise architects and IT leaders that follows and aligns with some common enterprise architecture frameworks. A typical enterprise IT footprint often comprises a combination of on-premises data centers with mission-critical applications, including legacy applications, business functions supporting SaaS, and possibly a mix of private and public cloud services. A common challenge that enterprises face in this model is getting to a cloud-native state, while also ensuring business continuity. According to the `Cloud Native Computing Foundation` (`CNCF`), “*cloud-native technologies empower organizations to build and run scalable applications in modern, dynamic environments such as public, private, and hybrid clouds. Containers, service meshes, microservices, immutable infrastructure, and declarative APIs exemplify this approach.*” *[2]*. The cloud-native approach enables enterprises to build loosely coupled systems that are resilient, manageable, and observable, while also delivering high-impact changes frequently and predictably.

It is also a common practice for enterprises to start building net-new applications following a cloud-native architecture. However, without a cloud foundation and an ecosystem to support it, this can lead to a siloed, tough-to-integrate with existing on-premises systems and applications or worse yet result in increased complexity and effort. It will also prove prudent to broadly classify and identify applications into three primary buckets:

*   **Legacy applications**: These are applications that serve primary business functions within the enterprise and are mission-critical to business. These often require a longer roadmap to fully transform to the cloud and are best suited to run within the existing on-premises environment.
*   **Cloud-readiness**: These are applications that are architected and built using traditional software development methods but have the potential to be replatformed or refactored to run in a cloud environment.
*   **Cloud native**: These are applications that are typically born in the cloud or applications re-architected from scratch to be cloud native.

Here are some primary objectives when architecting a cloud-ready enterprise:

*   Improve business agility and speed to market through increased development velocity and developer productivity
*   Reduce the IT effort and time to support business functions and growth – think infrastructure scaling, patching, DevOps, automation, upgrades, and maintenance
*   Ensure application availability, scale, and resilience as table stakes
*   Support an environment for innovation, prototyping, and rapid experimentation of new ideas with modern technology
*   Ensure application portability and reduce dependency on underlying infrastructure and platform through the adoption of open standards and open technologies

**Figure 8.1** describes the core constituents and the layers of an enterprise hybrid cloud framework:

![Figure 8.1 – Layers of an enterprise hybrid cloud framework](image/B18893_08_1.jpg)

Figure 8.1 – Layers of an enterprise hybrid cloud framework

Now, let’s review each of these layers in detail.

## Cloud foundation layer

This is the most important foundational layer and comprises a combination of infrastructure and platform services. The primary objective of this layer is to decouple the underlying core infrastructure from application services to ensure portability and platform-agnostic deployments. Some key architectural components to consider within the cloud foundation layer are as follows:

*   Network infrastructure modernization
*   Compute infrastructure modernization
*   Storage infrastructure modernization
*   Cloud-native application design principles

### Network infrastructure modernization

Regardless of a private cloud, public cloud, or multi-cloud strategy, the most critical factor to a successful digital transformation is network infrastructure. Unfortunately, this is also often the most overlooked aspect in several enterprises as businesses strive to deliver new products and services to their customers as efficiently as possible. In a traditional approach, the network perimeter was confined with a predictable boundary and fortified, following a single window access policy. This was much easier to construct, control, secure, and manage. However, in the digital era, network boundaries have expanded to cover a multitude of needs and opened up multiple entry and access points. For instance, introducing a SaaS service to meet a specific business need, adopting one or more public cloud platforms, corporate associates working out of global offices and remote locations, IoT devices connecting to corporate networks over modern protocols such as 5G, third-party access for suppliers and partners, deploying computing devices at the edge or store locations – all of these contribute to the blurring of network perimeter pronouncing a need for a network infrastructure modernization. The business value of the enterprise network has never been higher as the focus on digital transformation intensifies.

Traditional networks were fundamentally built on a premise that involves a physical network, routers, switches, hardware-based load balancers, network security devices, and protocol standards, often defined by individual network or device vendors. The data and management planes are tightly coupled in a traditional network infrastructure, necessitating huge efforts to device configurations whenever a change is warranted on the network. In contrast, modern cloud-based infrastructure relies on `software-defined networking` (`SDN`) principles, abstracting away the physical network infrastructure. `SDN` decouples the control plane from the data plane and enables network administrators to propagate network configurations and changes in near real time.

`SDN` enables a key capability where network administrators can operate and manage networks such as software using standard `SDN` APIs. Some key capabilities that `SDN` enables for enterprises are as follows:
