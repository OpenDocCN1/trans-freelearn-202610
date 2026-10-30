

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_348_0.png)

Figure 12.9 – Definition of DLP policies

## Note on DLP policies

At the time of writing, this feature is in preview, and the use of these capabilities must be confirmed by an opt-in (announcement to Microsoft). Subsequently, the action groups and also individual actions can be blocked or restricted so that UI flows are disabled if a DLP policy is violated in the flow (prohibited connector or unauthorized combination with another connector).

When the feature comes into general availability, it will only be available for managed environments for which additional security mechanisms exist beyond that. The Further reading section contains a link to get more information about managed environments.

Apart from the actual data, there is also other information worth protecting, such as access data and passwords. We will look at the possibilities for this in the next section.

## Secure credential storage

not been released yet. The *Further reading* section contains the link to the announcement for this. It is strongly recommended to use this service for the storage of sensitive information. Now let’s take a look at additional considerations for governance and compliance.

## Governance and compliance

In Microsoft’s documentation, there is a separate article on the topic of governance, which describes in detail the settings for the Windows operating system (registry settings) to restrict and monitor the functionality of PAD. We want to take the topic of governance a bit further here and look at what options are available to monitor UI flows and thus monitor the overall operations. Furthermore, on the topic of compliance, we will take a look at licensing.

## Monitoring flow execution

Two portals can be used to monitor the execution of flows: the **Power Automate** portal and **Power Automate admin center.**

The **Power Automate** portal provides a more operational monitoring capability. These functions are addressed to those who want to develop the flows, test them, and also monitor their operational functionality. The first view provides various charts, aggregated views, and filtering options (see the following screenshot):

## Figure 12.11 – Monitoring PAD flows

The second view in the Power Automate portal provides a section for monitoring the real-time execution of Power Automate Cloud and UI flows. This shows each execution of the flows and their status: Queued, Running, Succeeded, or Failed. This makes it very easy to monitor which flows are routed to which machine and which flows fail.

The reports in Power Platform admin center are aimed more at administrators who monitor the workload in the context of the environments. To do this, we need to go to Power Platform admin center and go to the Analytics | Power Automate section (see the following screenshot):

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_351_0.png)

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_352_0.png)

## Figure 12.12 – Flow monitoring in Power Platform admin center

There are different tabs available to provide information about the runs, usage, number of flows created and shared, and analysis of the connectors being used in flows. Governance requirements about the monitoring of Power Automate can thus be addressed very well. Another part is licensing, which we will look at in the following section.

## Licensing

In the course of the book, we have already learned about many functionalities of PAD, most of which are available with the free version. This is especially true for single use on a workstation, where flows are started manually and run interactively. Requirements such as scaling, parallel execution, centralized management, and rolling out flows are more typical for use in large enterprises and require the appropriate licensing. There are several license types available for this purpose, which then unlock a number of features and benefits and include the following features:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_354_0.png)

## Figure 12.13 – The Power Platform maturity model

This model can help determine the current state of automation capabilities and, from there, take the next steps, if necessary, to achieve a better level of automation and thus increase the company’s efficiency and productivity. In general, the idea here is not to look at individual applications but to take a holistic view of automation practices across an enterprise to embed them in an overall concept. For each individual automation project, so-called *holistic enterprise automation techniques (HEAT)* techniques can then be used to help design and roll out the solution in the context of the organization and manage the overall life cycle of the solution.

Another important building block is to build a team that addresses the various aspects of an automation solution and includes or builds experts to look at the solution from any facet, such as business content, ALM, infrastructure, security, and governance, as well as operational reporting and analytics. Another term for such a team is **Center of Excellence (CoE)**, and in relation to automation projects, **Automation Center of Excellence**.

In the meantime, many companies have also implemented these concepts in tools and processes. Microsoft has embedded this information into a so-called Automation Kit and, thus, a collection of applications and flows

based on Power Platform. This kit can be installed and configured as a solution in an environment. The basic idea is to use the artifacts and apps installed there to support an automation project from start to finish, including the following aspects:

- Definition of goals
- Review and release of the project
- Development and deployment
- Operational use and monitoring

This also supports different enterprise roles such as CVP, process owner, end user, and others. The following diagram illustrates the life cycle of such a project:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_355_0.png)

Figure 12.14 – Automation project life cycle

The fact that the kit can also be customized and that not all functions necessarily have to be used makes it easy to get started and shows what is possible with this technology. It is thus definitely worth a look and can bring added value and acceleration to automation projects.

## Enterprise automation scenarios with PAD

- The CRM system is implemented with Dynamics 365, and Power Platform with Power Automate is also in use.
- The internal invoicing system is represented by Contoso Invoicing (a desktop application), and the ERP system is accessible via a database (SQL Server).

At this point, we probably already have enough information to sketch an initial idea for the overall process. This could look as follows:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_357_0.png)

Figure 12.15 — Automation project scenario

Now, as always, there are several ways to implement this requirement, but there is a lot to be said for using a UI flow for this:

- The processing should be started individually by a team leader, which can be enabled via a shortcut on the desktop thanks to the advanced features.
- Outlook actions enable us to retrieve and process emails.
- We can extract the relevant information by using the text actions from *Chapter 11* (**Parse text** or **Recognize entities in text**). Of course, it would be beneficial if the email were standardized, making it easier to identify the customer ID.
- The next step would be to call the Dynamics 365 web API, retrieve all opportunities for the customer, and close these. An HTTP action will help us here to accomplish this action (**Invoke web service**).
- From a design perspective, we can outsource the processing for the invoice program to a separate cloud flow, which, in turn, includes the integration of the invoice program using the Desktop Flow connector. For the invoice application, we can then use the concepts for scaling that we learned about in this chapter.
- The ERP database will be updated by using the database actions, which we learned about in *Chapter 11*.

So, we can divide the overall processing into the following sections as subflows:

- `GetEmailMessages`: This should retrieve all emails and create a list of customers we need to feed into our systems.
- `CloseOpportunities`: For each customer, this subflow queries all open opportunities in Dynamics 365 and closes them. This will automatically trigger a cloud flow that listens to the **Opportunity Close** event and starts the processing with the invoicing app.
- `UpdateERP`: This will also take the customer information and create new customer records in the database.
- Subflow `NotifySeller`: This will send out a confirmation to the external seller.

You will notice during the projects that in many places, already known concepts can be reused, as in this case, we have already covered the use of Outlook, information extraction with text actions, as well as database queries in this book. So let’s take a look at a topic that hasn’t been looked at in detail yet: using the **Invoke web service** action to integrate Dynamics 365 from our UI flow.

## Calling web services within PAD

Another very powerful tool within PAD is the **Invoke web service** action. Almost all applications today provide web services as interfaces to trigger processes or work with data, and so does Dynamics 365. However, in an
