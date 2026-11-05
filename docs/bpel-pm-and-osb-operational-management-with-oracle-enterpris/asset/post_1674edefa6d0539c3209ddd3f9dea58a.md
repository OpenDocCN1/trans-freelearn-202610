
## Customer support

Now that you are the proud owner of a Packt book, we have a number of things to help you to get the most from your purchase.

## Errata

Although we have taken every care to ensure the accuracy of our contents, mistakes do happen. If you find a mistake in one of our books — maybe a mistake in text or code — we would be grateful if you would report this to us. By doing so, you can save other readers from frustration, and help us to improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/support, selecting your book, clicking on the errata submission form link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata added to any list of existing errata. Any existing errata can be viewed by selecting your title from http://www.packtpub.com/support.

## Piracy

Piracy of copyright material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works in any form on the Internet, please provide us with the location address or website name immediately so that we can pursue a remedy.

Please contact us at copyright@packtpub.com with a link to the suspected pirated material.

We appreciate your help in protecting our authors, and our ability to bring you valuable content.

## Questions

You can contact us at questions@packtpub.com if you are having a problem with any aspect of the book, and we will do our best to address it.

## 1 Grid Control, BPEL, and OSB Overview

Management of hardware, software, and application components is important. In bad economic times, it is critical to get the maximum return from software and hardware investments, and for that an enterprise needs a sound management strategy. A management solution not only helps increase returns on existing investments, but also controls the cost of owning these investments. Enterprises today cannot be successful without such a solution.

In the SOA world, management is even more critical, as it governs distributed services and service infrastructures. The value from SOA investments cannot be realized without a sound management strategy. Operational management is becoming the centerpiece of the SOA Governance framework in most enterprises. Unless there is visibility and control on production environments, SOA project costs will exceed the accrued benefits. Management cannot be a bolt on after the implementation; it has to be a strategic part of the overall solution. It is beneficial to start thinking about the management strategy upfront, and implement it in conjunction with the SOA integrations. Further, all aspects of management need to be analyzed, and the right ones need to be implemented for the SOA integrations in the enterprise. Some of the key management areas are infrastructure monitoring, synthetic monitoring, configuration management, deployment automation, and business process execution monitoring. All of these need to be considered for the success of SOA projects.

In this chapter, we will provide a brief overview of the following products that will be covered throughout this book:

- Grid Control
  - Overview
  - Architecture and deployment
  - Download and install
  - Home page
- Oracle BPEL Process Manager overview
- Oracle SOA Suite overview
- Oracle Service Bus overview

### Grid Control

Enterprise Manager Grid Control provides a single interface for managing thousands of disparate targets such as operating systems, databases, application servers, and packaged applications. It determines overall target health, tracks target inventory, automates target deployments, and manages service levels. All the historic monitoring information is stored in a database, and aggregated over time. This is complemented with a reporting and dashboard framework, which provides a summary to administrators as well as IT managers. Grid Control supports monitoring and management of a wide variety of hardware, software, and applications, and is really the tool of choice for Oracle products. It has capabilities for monitoring third-party products as well, either natively, or through plugins and connectors. The Grid Control framework can be extended using either plugins or connectors. This is made possible by a software development kit that comes out of the box with Grid Control. A good example of a plugin is if you want to monitor a specific hardware component that is not supported by Grid Control, you can extend the framework and add logic to monitor your specific hardware component. A connector, on the other hand, can be used to integrate Grid Control with other systems such as incident management systems (for example, Remedy Helpdesk or Oracle Business Activity Monitoring (Oracle BAM)).

Most chapters in this book cut across multiple versions of Grid Control. Concepts such as service-level management, infrastructure management, service-level dashboards, web application management, and configuration management are uniform across multiple Grid Control versions. That being said, this book will focus on Grid Control version 10.2.0.5, also referred to as 10gR5. This version was released on Linux platforms in February 2009, ported to other platforms during the rest of the year. Most of the chapters are valid for the prior version of Grid Control 10.2.0.4 (or 10gR4) as well. The notable exception is the third section that deals with managing the Oracle Service Bus. Enterprise Manager functionality for managing BPEL Process Manager is part of Grid Control 10.2.0.3 and above. The Oracle Service Bus management functionality was introduced with Grid Control 10.2.0.5.

### Architecture and deployment

Grid Control has a three-tier architecture. The main components are the Oracle Management Server (OMS), the Management Agents (Agent), and the Oracle Management Repository (OMR). The central web-based console is available through popular browsers. The components are detailed as follows:

- **Oracle Management Server (OMS):** This is the central engine, which accumulates information from all the distributed Management Agents. It provides the data for the web-based console, and persists the data in the Management Repository. Further, the OMS also executes the alerting rule logic, and the notification logic, as well as sending instructions to the agent for management tasks such as deployment.
- **Management Agent:** The agent is a lightweight C-program running on a server or host. This program typically collects all the monitoring and configuration information for the host, and the other targets on that host such as database, application server, SOA suite, and so on. The agent is also suitable for running operations at the host level through scripts. This capability is useful for taking corrective action (when a threshold is violated), or performing complex patching, or other deployment tasks. There is also the option of having remote agents, the only drawback being that the host metrics will not be collected.
- **Oracle Management Repository (OMR):** The repository is a preconfigured Oracle database that comes packaged with Grid Control. No separate license is needed for the OMR. The OMS contains current and historical monitoring and configuration information from various agents. It comes with prebuilt schemas and PL/SQL procedures and functions to aggregate the data over different time periods.
- **Central web console:** The web console provides a window to all the rich information in the OMR. Each target type (for example, application server) has a home page with monitoring, alerting, configuration, and other specific information. The home page displays summary information across multiple targets. Reports and dashboards can be used to create custom views.

This book avoids going into deep discussions on deployments that are already addressed by product documentation or white papers. A basic knowledge of installing and configuring the Grid Control components are assumed. More information on architecture and deployment best practices is available on the Enterprise Manager pages on the Oracle Technology Network (OTN) at: http://www.oracle.com/technology/products/oem/ent_mgr/arch_dep.html

The following simple architecture diagram shows the main Grid Control components and their relationships:

![](img/7529bbd615e859608c21b965162348b2_24_0.png)
