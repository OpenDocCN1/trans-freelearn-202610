
## The case for a data and AI platform

In the previous section, we introduced IBM’s prescriptive approach to operationalizing AI in your enterprise, starting with making data access simple and organizing data into a trusted foundation for advanced analytics and AI. Now, let’s look at how to make that a reality.

From an implementation perspective, there are many existing and established products in the industry, some from IBM or its partners and some from competitors. However, there is a significant overhead to making all these existing products work together seamlessly. It also gets difficult when you look at hybrid cloud situations. Not every enterprise has the IT expertise to integrate disparate systems to enable their end users to collaborate and deliver value quickly. Reliably operating such disparate systems securely also stretches IT budgets and introduces so much complexity that it distracts the enterprise from achieving their business goals in a timely and cost-efficient fashion.

Due to this, there is a need for a data and AI platform that provides a standardized technology stack and ensures simplicity in operations. The following diagram shows what is typically needed in an enterprise to enable AI and get full value from their data. It also shows the different personas in a typical enterprise, with different roles and responsibilities and at different skill levels, all of which need to tightly collaborate to deliver on the promise of trusted AI:

![](img/50e1dac0750b69f2b8e8efff000eb13b_34_0.png)

Figure 1.7 – Silos hinder operationalizing AI in the enterprise

Most enterprises would have their data spread across multiple data sources, some on-premises and some in the public cloud and typically in different formats. How would the enterprise make all their valuable data available for their data scientists and analysts in a secure manner? If data science tooling and frameworks cannot work with data in place, at scale, they may even need to build additional expensive data integration and transformation pipelines, as well as storing data in new data warehouses or lakes. If a steward is unable to easily define data access policies across all data in use, ensuring that sensitive data is masked or obfuscated, from a security and compliance perspective, it will become unsafe (or even illegal) to make data available for advanced analytics and AI.

The following diagram expands on the need for different systems to integrate, and users to collaborate closely. It starts with leaders setting expectations on the business problems to address using AI techniques. Note that these systems and tasks span both the development and operational production aspects of the enterprise:

![](img/50e1dac0750b69f2b8e8efff000eb13b_35_0.png)

Figure 1.8 – Typical cross-system tasks and cross-persona interactions

This flow is also circular since the systems need to account for feedback and have a clear way of measuring whether the implementation has met the objectives stated. If the consumers of the data do not have visibility into the quality of data or can't ensure the data is not stale, any AI that's built from such data would always be suspect and would therefore pose an increased risk. Fundamentally, the absence of a foundational infrastructure that ties all these systems together can make implementing the AI ladder practice complicated, or in some cases, impossible. What is needed is a reliable, scalable, and modern **data and AI platform** that can break down these silos, easily integrate systems, and enable collaboration between different user personas, even via a single integrated experience.

## Summary

In this chapter, you learned about market dynamics and IBM's Data and AI portfolio, were provided with an overview of IBM's prescriptive approach to AI, known as *the AI ladder*, and learned how IBM offerings map to the different rungs of the ladder. In the next chapter, we will provide a thorough introduction to Cloud Pak for Data, IBM's Data and AI platform that enables enterprises to implement *the AI ladder* anywhere in a modular fashion while leveraging modern cloud-native frameworks.

## The case for a data and AI platform – recap

In the previous chapter, we introduced IBM's prescriptive approach to operationalizing data and AI in your enterprise, and subsequently, we introduced you to the broad capabilities of Cloud Pak for Data. In this section, we will outline how Cloud Pak for Data powers the implementation of the AI Ladder practice in your enterprise.

Cloud Pak for Data, as an open platform, breaks down silos between different products, enabling them to be manifested as cloud-native services on top of the same OpenShift Kubernetes platform. Cloud Pak for Data enables such services to easily integrate through a common control plane and, importantly, provides a personalized, integrated user experience for end users to collaborate naturally, without even being aware of product or service boundaries.

On this one platform, capabilities such as data virtualization help modernize data access for the hybrid cloud and data can be easily accessed in place across clouds. Data cataloging and governance services can piggyback on data virtualization and other data sources to deliver on broader governed data access plane solutions, which in turn facilitates trust in data that is made available to data science, analytics reporting, and machine learning services. On the same platform, operations teams can deploy ML models at scale, enabling model monitoring with bias detection to support continuous model evolutions.

Development, experimentation, and production deployments can be supported on the same platform, promoting collaborations between teams of users with varying roles and responsibilities to work closely. Authentication and authorization, customized in the control plane to comply with enterprise regulations, enable seamless access across all services. Consistent APIs and concepts such as user groups, API keys, and tokens enable the development of custom applications and solutions in a standard manner.

Administration and operations are simplified through operators powered by Red Hat OpenShift Kubernetes. Portability of deployment across on-premises and public cloud infrastructure is assured, enabling bursts into the cloud when needed. Cloud-native practices enable continuous delivery and integration for enterprise applications and solutions. Security controls and operational auditing are built into the platform to ensure adherence to compliance requirements in the enterprise. The deployment and management of multiple tenants on the same set of shared compute resources also make Cloud Pak for Data much more cost-effective for an enterprise.

## Overview of Cloud Pak for Data

In Chapter 1, The AI Ladder: IBM’s Prescriptive Approach, you were introduced to IBM’s prescriptive methodology to AI adoption, the AI ladder, and how enterprises can embrace this framework through IBM’s modern multi-cloud data and AI platform, Cloud Pak for Data. The concept of an integrated platform to manage data, analytics, and AI services is not new – there has always been a demand for an end-to-end offering spanning the different rungs of the AI ladder. Enterprises addressed this need through custom integrations by often tapping into global consulting organizations for resources and expertise.

With the advent of public cloud and service-oriented architectures, it is now becoming commonplace to procure pre-integrated software platforms. These platforms minimize the integration effort while still providing the flexibility to meet specific business requirements.

Cloud Pak for Data was launched on May 29, 2018, leveraging Kubernetes technology, which allowed the platform to be deployed on any public cloud and/or private cloud in enterprise data centers. This enabled clients to leverage cloud-native benefits such as easy provisioning, scaling, simplified management, and seamless upgrades, while sticking to their infrastructure of choice: the private cloud or any public cloud such as AWS, Azure, or IBM Cloud. It’s also worth highlighting that the data and AI services underpinning the platform have been tried and tested by thousands of enterprise customers over the past few decades. So, while the integrated platform is new and modern, the capabilities and functionality behind it have been proven at scale. This is critical for enterprise customers who cannot afford to make risky investments as they are looking to modernize their data and AI workloads. The following diagram illustrates the various releases of Cloud Pak for Data since its launch:

![](img/50e1dac0750b69f2b8e8efff000eb13b_39_0.png)

Figure 2.1 – IBM Cloud Pak for Data journey: 8 releases since launch
