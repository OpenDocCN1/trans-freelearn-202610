
- Understand the process of opportunity closure and what activities are typically executed before and after an opportunity closure.

Similarly, you need to fully understand the capabilities offered for service and marketing. Make yourself familiar with other general functionalities, such as email alerts, multilingual support, the translation workbench, multi-currency support, flows, flow actions, time-based workflows, profiles, permission sets, App builder, validation rules, custom settings, custom metadata types, reports, analytics, and so on.

On the programmatic side, you need to have a good understanding of the platform’s available capabilities. Hands-on experience is an advantage. Make yourself familiar with the structure of Apex and its keywords. Take a look at some examples of Apex classes, triggers, LWCs, and Visualforce pages.

> Note
> You can learn more about Apex at the following link: https://packt.link/8Ux8K.

Another way to handle requirements that cannot be met with out-of-the-box capabilities is via third-party applications. You are going to cover that next.

## Augment Your Solution with the Right External Applications

After concluding that the out-of-the-box capabilities are not enough to fulfill the desired functionality, you can consider a programmatic solution or even an external one. In some cases, the decision is easy to make. In other cases, it may turn out to be a complicated build or buy decision.

In real life, build versus buy decisions are not easy to make. You need to consider the following points:

- Cost and time-to-value: The cost of a custom-built solution could become unpredictable depending on many factors, including complexity and resource availability. Moreover, a custom-developed solution tends to require much more effort to design, plan, test, and deploy.

On the other hand, the cost of buying an external solution can be easily calculated and forecasted. They are easier to deploy and less error-prone.

- Maintenance: Salesforce is offered as Software as a Service (SaaS). It is fully managed software where most of the maintenance overhead is handled by the vendor. The core cloud has three yearly releases. Each includes updates that could impact custom-developed code built on top of the core cloud.

Fully custom-developed solutions or solutions built on top of open source frameworks come with even more maintenance overhead, including patching newly discovered security threats. Third-party solutions are not exempt from such challenges. However, they have other companies looking after them, which reduces the overall maintenance cost and overhead for their clients.

- **Scalability**: The tools are only as good as the people using them. The quality of the custom-developed solution is hugely impacted by the team designing and developing it. There are several solutions that operate as expected under normal circumstances but fall apart whenever they are challenged to scale up or get extended.

Software development houses have frameworks, methodologies, regulations, and processes tailored to deliver high-quality, scalable, configurable, and extendable software products.

- **Quality and peace of mind**: Most enterprises are looking for battle-tested solutions, especially for mission-critical processes. A product that has been successfully delivered several times to other customers, especially in the same industry, can give a high level of confidence.

It is not easy to calculate the total software cost of ownership. You are not expected to do so during the CTA review board, but assuming that buying a product is always more expensive than developing it is a mistake that a CTA should not fall into.

On the other hand, avoid being hesitant to propose a custom solution developed on top of the force.com platform when it is technically reasonable to do so.

Note the following examples of build versus buy decisions:

- Avoid proposing to build a custom CPQ solution. There are plenty of great solutions available on the market, including one from Salesforce itself (Salesforce CPQ).

- Avoid proposing to build a custom Field Service solution. Such solutions are complex and will take years to mature if custom developed. There are plenty of capable Field Service solutions on the market including Salesforce Field Service.

- Avoid proposing to build a custom solution for complex **contract lifecycle management (CLM)** requirements. Salesforce comes with simple capabilities in this field out of the box, and you can surely invest time and money in developing a custom solution either on top of the force.com platform or completely off-platform. But it will take considerable time and effort to get a custom-developed product to the level of maturity expected by an enterprise. There are several mature products on the market, such as DocuSign CLM.

- You can propose building a custom LWC to use as a data entry tool to increase users’ efficiency.

- You can extend out-of-the-box Salesforce capabilities such as opportunity management and case management with custom modules using additional objects, fields, flows, workflows, Apex, and other platform tools.

When you propose an AppExchange or third-party solution, you are still expected to clearly explain how you see it can be utilized to deliver the desired functionality. You are also expected to mention the name of the proposed product. Make sure you are familiar with some popular products from the Salesforce ecosystem. You will see examples in the mini hypothetical scenario next.

## Introducing the Solution Architecture Domain Mini
### Hypothetical Scenario: Packt Visiting Angels

The following mini scenario describes a challenge with a particular client. The scenario is tuned to focus on challenges related to solution architecture specifically. But as you would expect, there are also requirements related to other domains, such as data and security.

As you did in the previous chapters, you will go through the scenario and create a solution step by step. You should try the suggested solutions yourself unless you are already familiar with the described feature. You are also encouraged to try different solutions that could fulfill the requirements.

Remember that the solutions listed are not necessarily the only possible solutions. Alternate solutions are acceptable as long as they are technically correct and logically justified.

## Scenario

Packt visiting angels (PVA) is a home clinical trial company that bridges the gap between clinical research organizations (CROs) and clinical trial volunteers. PVA provides various sets of services, including home visits and site nurse services. They operate across five different European countries (the UK, France, Germany, Italy, and the Netherlands).

## Current Situation

PVA has a sales operation team that deals with CROs. They take over from the marketing team, who gather and qualify information about potential deals via multiple sources.

The sales operations team monitors qualified deals, particularly high-value deals (with a total amount above five million euros). It also handles various follow-up activities, such as creating quotes, managing contracts, notifications for other internal team members, follow-up tasks, and scheduling wrap-up meetings.

Once the deal is closed, the nurses get assigned to the project. The nurses should follow an optimized schedule to visit as many sites as possible, considering their skills, location, and the shortest possible route.

PVA's business is growing rapidly, and they are looking to introduce more automation to increase their internal teams' efficiency using Salesforce as their central CRM solution.

## Requirements

PVA has shared the following requirements:

- Leads are normally gathered via three channels. These are web forms, third-party providers, and being manually created by the marketing team. PVA wants to ensure that the gathered leads are unique, where no more than one lead per company, per drug, and per country should be open at the same time.

- Leads that are not created manually should automatically be assigned to a country’s marketing team based on the geographic location (country). Leads can have only one associated country.

- Once the lead is assigned, it goes into multiple stages. At each stage, a set of activities is expected to be completed by the lead owner.

- Once the lead is qualified, it must be converted into an opportunity and automatically handed over to the country’s sales operations team. One of the team members (a sales operations agent) should pick up the opportunity to develop it further.

- The sales operations agent will follow up with the deal. The agent should be able to create quotes and negotiate them with the client.

- Once a quote is accepted, the agent updates the opportunity to the contracting stage. Once that is done, the system should automatically generate a contract record using the opportunity information.

- The agent should ensure the contract’s data is accurate, select the right terms and conditions, and send the contract to the client for an online signature. The contract should be presented in the local language of the target country.

- The client should receive a request to sign the document online. Once the document is signed, the contract should be made active, and the opportunity should be updated to closed-won.

- The agent should update lost opportunities to the closed-lost stage manually and set a reason for the loss.

- Once the opportunity is closed-won, a project that contains key information derived from the opportunity should be created automatically. Only the nurses in the target country and their managers should be able to view the project details.

- The solution should provide the nurses with their daily optimized visit schedule. This schedule should aim at giving the nurses the opportunity to visit as many sites as possible during their working hours.
