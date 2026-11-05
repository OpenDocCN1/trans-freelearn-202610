
## Installing Grid Control

The latest version of Grid Control can be downloaded from OTN at:
http://www.oracle.com/technology/software/products/oem/index.html

Grid Control documentation including installation, configuration, deployment modes, and so on can be found at: http://download.oracle.com/docs/cd/B16240_01/doc/nav/portal_booklist.htm

Once Grid Control is installed and configured, the SOA-related targets can be added. The prerequisite is to install a Management Agent (Agent) to the server that hosts the SOA Suite. More details on getting started with Grid Control SOA Management is available at the Oracle Technology Network (OTN) website: http://download.oracle.com/docs/cd/B16240_01/doc/em.102/e12650/toc.htm

## Home page

After downloading and installing Grid Control, the first task is to view the home page of Grid Control. To view Grid Control home page on your local browser:

- 1. Navigate to http://<your_server_name>:<server_port>/em.
- 2. Login as sysman.sysman. Use the password you set during the installation.

Following is the Enterprise Manager 10g Grid Control entry point or home page. The home page rolls up all the important information (starting in the upper-left corner):

![](img/7529bbd615e859608c21b965162348b2_25_0.png)

- **All Targets Status**: This section shows the total number of monitored Oracle targets. The pie chart shows their overall SLA availability and corresponding open alerts. This summary page can be rolled up for any target type.

Click on the **View** dropdown and observe the different target types in this enterprise. This is a subset of the target types supported by Grid Control.

- **All Target Alerts**: Violations of thresholds for all metrics for the monitored targets, separated into **Critical** and **Warning** categories.
**Errors** are Agent-related problems (Agent unreachable, cannot upload, metric collection error, and so on).

- **All Targets Policy Violations**: Over 300 Oracle best practices (for example, open port on Application Server) are coded in as policies in Grid Control. The Agent checks these policies daily and flag the violations.

- **Target Search**: Quickly access any target by simply typing in part of the target name.

- **Critical Patch Advisories for Oracle Homes**: Push any security alerts that are vital to your systems. Set up a connection to meta link and automatically download patch information for monitored Oracle homes.

- **Deployments Summary**: Enterprise Manager collects information on your entire configuration including software, patches (OS and Oracle software level), hardware, and so on. This is a rollup of all of the hosts, software, and OS(s) in your enterprise.

## Oracle BPEL Process Manager overview

*Business Process Execution Language (BPEL), short for Web Services Business Process Execution Language (WS-BPEL) is an executable language for specifying interactions with Web Services. Processes in Business Process Execution Language export and import information by using Web Service interfaces exclusively. Source: http://en.wikipedia.org/wiki/Business_Process_Execution_Language*

Major vendors such as Oracle, Microsoft, and IBM are onboard with the BPEL standard, which has become the orchestration standard of choice. Oracle acquired a small company called Collaxa in 2005, which had an implementation of the BPEL standard, then renamed the product to BPEL Process Manager and made it the foundation of the SOA Suite. This also became the strategic technology product under the Fusion Middleware umbrella, as well as the Fusion Applications umbrella. Oracle Applications Unlimited (E-Business Suite, PeopleSoft, Siebel, Retek, and so on) use several different workflow technologies such as Oracle Workflow, but the roadmap for all these workflow technologies is to converge into the common BPEL standard, based on the Oracle BPEL Process Manager product. In short, this technology has huge implications not only for the overall middleware market, but also for the packaged applications market. Oracle, with its huge installed base in both the markets, especially after the acquisition of BEA in 2008, is focused on expanding the already large adoption of BPEL within its customer base.

BPEL Process Manager 10.1.2 was the first release after the Collaxa acquisition. Currently, the product is available as part of the Oracle SOA Suite 10.1.3.x. This is a fairly mature release of product, and has enjoyed widespread adoption among customers interested in implementing an SOA strategy. This is also the foundation of the next generation Oracle Fusion Applications.

## Oracle SOA Suite overview

Oracle SOA Suite 10.1.3.x consists of three components – Oracle BPEL Process Manager, **Oracle Enterprise Service Bus (OESB)**, and **Oracle Web Services Manager (OWSM)**. Additionally, it also includes **Oracle Service Bus (OSB)**, **Oracle Business Activity Monitoring (BAM)**, and the Rules Engine.

Oracle built an Enterprise Service Bus (OESB) as the plumbing layer to frontend the BPEL-based business processes. The Service Bus is used as the mediator layer to transform messages in different formats to common canonical formats, and route to the backend BPEL processes, or other services. It is a step up from the point-to-point **Enterprise Application Integration (EAI)** methodology that involves frequent changes at both ends of the integration.

Oracle SOA Suite 10.1.3.x is a single installable, and is extremely easy to install and configure with Oracle JDeveloper. As the common IDE to develop BPEL-based business processes, and OESB-based services it provides a great platform for developers and architects to implement SOA projects.

## Oracle Service Bus overview

> *Oracle Service Bus is a proven, lightweight and scalable SOA integration platform that delivers low-cost, standards-based integration for high-volume, mission critical SOA environments. It is designed to connect, mediate, and manage interactions between heterogeneous services, legacy applications, packaged applications and multiple enterprise service bus (ESB) instances across an enterprise-wide service network. Oracle Service Bus is a core component in the Oracle SOA Suite as a backbone for SOA messaging. Source: http://www.oracle.com/technologies/soa/service-bus.html*

With the acquisition of BEA, the AquaLogic Service Bus (ALSB) joined the Oracle family as well. ALSB was renamed to Oracle Service Bus (OSB) post acquisition. On July 1, 2008, Thomas Kurian, the Senior Vice President of Fusion Middleware, announced the strategic roadmap for Oracle middleware and BEA products. He announced that OSB would be the strategic service bus going forward, with its wide customer adoption, and advanced product capabilities.

## Summary

With a single management solution to manage SOA services and infrastructure, and products, Grid Control reduces the complexity of an SOA environment. Grid Control is an enterprise-wide solution, so care must be taken to make it into a strategic product. The installation and deployment is not a trivial task. However, it is imperative to set up the management framework to meet the needs of the enterprise.

Grid Control has traditionally been viewed as a database management solution, but this book will highlight the necessity of using this solution for SOA environments.

In this chapter, we looked at some fundamental concepts of the target products, as well as the management solution. In particular, we discussed Enterprise Manager Grid Control as an important management solution. Briefly, we discussed the installation, documentation, and architecture of Grid Control. Then we discussed the SOA products BPEL PM and OSB. We introduced the basic concepts for both products, and a brief background for each.

This was simply an overview of these products. A good administrator should have basic working knowledge of Oracle BPEL PM, SOA Suite, and Service Bus. This book will focus on the next step, which is to manage these products from Grid Control.

The next few chapters will walkthrough hands-on exercises to set up and perform management tasks.
