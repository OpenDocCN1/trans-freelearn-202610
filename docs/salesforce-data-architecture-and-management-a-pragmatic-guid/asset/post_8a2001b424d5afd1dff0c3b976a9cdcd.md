
For enterprises, architectural needs vary depending on the needs and stage of the issue the enterprise is trying to solve. Over several years, technology professionals have realized this and have delineated architecture into multiple domains. Since our discussion topic is the role of a data architect, it would be prudent to discuss the various domains of architecture because they are interlinked and exert constraints over each other.

In the subsequent sections, we will look at why data architecture is important as well as explore the different types of architects, their roles, and some of the deliverables they are responsible for producing and maintaining. This will give us a greater understanding of the differences in responsibilities and an appreciation of how the type of architect fits into the architectural puzzle of an enterprise.

## Exploring architecture roles

Architects have been around for as long as business systems. They may have had different titles over the years, such as systems analyst, technology analyst, and the like, but fundamentally, they have always designed systems that are scalable and efficient. Today, the titles have become more refined and there are multiple types of architects specializing in their own domains. Understanding each of these is important because it will help us have better communication with them and enable us to bring other stakeholders on the same page as well.

## Business architect

**Business architects** are concerned with identifying how value can be created for internal and external stakeholders of the enterprise. This usually involves producing strategy documents, process maps, capability maps, and commonly used business terms within the enterprise or the industry in which the enterprise operates.

## Data architect

**Data architects** are the key topic of this book. Of course, they are mainly concerned with data: how data is organized, how it is moved internally or externally, and how it can be made accessible to users when it is needed. Data architects create data models and make decisions on how data will be stored, consumed, and archived. Some data architects will also analyze and communicate how insights and intelligence can be derived from data that can be useful to stakeholders. Typical deliverables include data models, data definitions, the flow of data, and integrations.

## Solution architect

A solution architect is concerned with designing systems that deliver business value to stakeholders. Working closely with Business Architects (BAs), they ensure that the solution will provide the business value that the business requires. One of the key responsibilities of solution architects is to identify the most optimal technology that will meet business requirements. This can include packaged software such as Salesforce or designing custom solutions from the ground up. Deliverables for this type of architecture include functional and technical design documents, system landscape diagrams, and the actual software that's delivered in line with the design that the architect puts forward.

## Domain architects

These are architects that have expertise in a particular domain of technology. For example, Information Security (IS) architects, technology architects, and cloud architects have very specific expertise in their respective areas and are considered domain architects. A Salesforce Certified Technical Architect (CTA), although required to have a broad knowledge of certain areas, are experts in implementing Salesforce solutions.

## Information security architect

The IS architect has been becoming more important as data integrations between systems become common and cybersecurity becomes critical for enterprises to protect their data and, by extension, their reputation. IS architects are responsible for designing, implementing, and maintaining security solutions that can protect the organization's network and hardware. They regularly conduct different types of testing to identify vulnerabilities and proactively fix them before a security incident materializes. In cases where the organization's security is breached, they work on a root cause analysis and identify fixes to remediate those vulnerabilities.

## Technology architect

A technology (or infrastructure) architect is concerned with the physical infrastructure needed to enable organizations to deliver their software applications to their stakeholders. Servers, networks, and cloud-based Infrastructure as a Service (IaaS) fall into this category. Their responsibilities include the following:

- Designing and implementing the infrastructure solutions needed by organizations
- Supporting the hardware needs of projects in an enterprise
- Monitoring production environments to ensure that they are running optimally by monitoring certain attributes such as throughput, latency, and redundancy, among others

Deliverables include network diagrams, server-to-server diagrams, and others. With the advent of the cloud, another type of architect that's gaining traction is the cloud architect.

## Cloud architect

These architects are mainly concerned with an enterprise's cloud strategy and its implementation. Like technology architects, they are concerned with implementing cloud solutions that can support software that runs on the cloud. The key difference between technology and cloud architects is that traditionally, the former has been more focused on on-premises systems whereas the latter has been focused primarily on cloud solutions. Salesforce, for example, runs entirely on the cloud and although customers don't have to be concerned with how that cloud is implemented, maintenance schedules and security patches, Salesforce has internal resources that focus on maintaining the cloud infrastructure, ensuring that it can meet the needs of its customers. Cloud architects are also change agents as they must be able to effectively communicate the benefits of using the cloud and be knowledgeable enough to address the questions and concerns that people not so familiar with cloud technologies have.

Let's look at why data architecture is important and what its benefits are.

## Why is data architecture important?

A common mistake that rookie software developers make is to jump in and start writing code, thinking that the design can be modified later to fit requirements. Sometimes, they may refer to this as Agile thinking and approach. However, this is a fallacy as the data model, once defined, will be used as the foundation upon which other required business functionality is built; it is usually not straightforward to make changes to it quickly.

Let's take an example of an integration that pushes data from an external webinar system into a Salesforce Campaign Member object. Here, the webinar system takes the amount of time a person watched the live webinar and records it as a percentage. The developer has created a custom field of the number type called Webinar % Attended on the Campaign Member object. This field will only accept a number and therefore rejects any letters or special characters.

When the integration is run for the first time, it errors out because the webinar system is sending the percentage attended with the special character, %, and Salesforce therefore rejects it. This is a simple example to demonstrate how the data model needs to be well thought out and designed, not only considering the business requirements, but also understanding system functionality and limitations. In this case, the business doesn't need the values in the **Webinar Completed** field to make any calculations, so changing the field type to **Text** will work. In most cases, though, a change like this would require a thorough analysis of impacts on existing functionality and integrations and this can be a time-consuming effort.

Moreover, owing to the large number of systems that are now used in an organization, the sharing of data between these systems (that is, integration) has become crucial. Proper data architecture can avoid costly mistakes such as integrating two systems only to find out at a later stage in the project that there are technical limitations on how often the data can be moved between the two systems.

> Important note
>
> In architecture, decisions made earlier affect decisions made later. For example, should a Team Member-related list, intended to record team members working on the lead, be a lookup or a master-detail relationship? These are important design decisions to consider upfront as a change during build, or worse, when the application is in production, can be costly. For example, a decision to change from a multi-select picklist to a single picklist when the app is in production can lead to data loss. Consequently, our earlier decision affects decisions we will have to make surrounding the subsequent data loss.

Another consideration for data architecture is **data security**. Nefarious individuals or state-sponsored actors understand the value of data, and they look to exploit vulnerabilities for their own advantage. Data architecture ensures that the proper security processes and techniques are applied to the data when it’s at rest or in transit. Modern data architecture also needs to conform to data protection regulations such as **General Data Protection Regulation (GDPR)** and **California Consumer Protection Act (CCPA)**.

Organizations recognize the value of data and derive insights from it. Moving data from one system to another is not done in vain, rather, there are clear business objectives that are to be achieved. The most common of these objectives is decision making based on hard data. Proper data architecture will take into account how data from a multitude of systems can be brought together to meet the operational and analytical requirements of the business. An intense topic of interest these days, which we will discuss in the upcoming chapters, is how to achieve that single, unified view of the customer commonly referred to as Customer 360, and data architecture plays a key role in that.
