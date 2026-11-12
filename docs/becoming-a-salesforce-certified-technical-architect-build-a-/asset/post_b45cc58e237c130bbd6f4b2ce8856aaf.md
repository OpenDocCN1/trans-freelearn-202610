
### The actors and licenses diagram should look as follows:

| Role | License |
|------|---------|
| Trader | Partner Community License |
| Call Service Agent | Service Cloud License<br>Service Cloud Voice |
| Call Service Manager | Service Cloud License<br>Tableau CRM |
| Consumer | Customer Community License |
| Sales Agents | Sales Cloud License |
| Top Management | Sales and Service Cloud License<br>Tableau CRM |

- Raise payment and inventory-related support cases
- Log in to trader portal
- Receive email alert when an order is placed
- List product on website
- View consumer value indicator reports and dashboards
- View 360-degree consumer data
- Handle cases via phone and email channels
- Place an order
- Monitor and manage support team
- Visit public website
- Raise a case
- Own the trader's onboarding process
- View and search the FAQ
- View reports to track the performance trends of items
- Review purchased item
- Log in to consumer portal

### Figure 7.10 – Actors and licenses (final)

Finally, the data model diagram should look as follows:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_268_0.png)

### Figure 7.11 – Data model (final)

That concludes this scenario. Remember that your diagrams are tools to help you make the end-to-end solution crisp and clear. They are not the targets themselves, but tools that can help you design and document a better solution. They add a lot of value to your presentation.

## Summary

In this chapter, you have dived into the details of the Salesforce data architecture domain. You learned what a CTA must cover and the extent of detail. You then discovered some interesting principles that a Salesforce data architect needs to master and had an under-the-hood look at the causes, impact, and mitigation strategies for LDV objects.

You then tackled a mini hypothetical scenario that focused on data architecture, and you solutioned it and created some engaging presentation pitches. You identified several LDV objects and prepared a comprehensive mitigation strategy based on the business’s nature and shared requirements.

If you are now part of a Salesforce implementation project, or you are administrating a running instance, you can practice the process of identifying LDVs. Start by analyzing the data model diagram (or create it if you still do not have one!) and use the tools and techniques you covered in this chapter to identify LDVs. Do your calculations and share your observations, concerns, and the proposed solution with your stakeholders. Remember to make your solution crisp and clear; it must cover the topic from end to end, and it must logically explain why it is the right solution from your perspective.

You also covered several other topics in the mini scenario, including selecting the right reporting tool and explaining how to get the data into it. You then created a comprehensive data migration strategy based on the shared requirements, ensuring data integrity and minimizing disruption to daily operations.

In the next chapter, you will learn about the fourth domain you need to master: solution architecture.

## Creating a Lean Solution Architecture

In the digital world, the term “solution architecture” refers to the process of analyzing client needs, understanding the core of their problems, and determining the appropriate solution to provide measurable benefits to the business.

The solution architect is likely the most business-aware architect on a project. To pass the CTA review board, you must possess both the technical skills and the ability to interpret business needs and craft an end-to-end solution architecture. Your solution must consider the capabilities and limitations of the platform.

The detailed principles of lean architecture are beyond the scope of this book. But in short, it is all about eliminating waste by achieving the following:

- It defines and follows standards to reduce the time wasted on recurring challenges and discussions. This should also aim to reduce overall solution inconsistencies.
- It ensures you are establishing an environment that enables smooth feature development. A poorly architected solution could face many difficulties in scalability and extensibility.
- It ensures that all processes are creating value for the customer, reducing wasted time and effort spent on activities that yield no added value.
- It introduces a well-established mechanism to document the key solution artifacts, reducing the time and effort wasted on extensive documentation with low business value.

As a CTA, you are expected to have strong hands-on experience with the platform and use that knowledge while designing a solution. Salesforce is a platform that enables architects to easily try things out. This is a powerful capability that should be fully utilized.

> I always used to say to my mentees that you cannot just pull a solution out of the air. You have to roll up your sleeves and try things out on the ground. You are lucky to deal with a platform that allows you to try out 70-80% of the features before committing to a proposal.

You have already come across principles and activities related to solution architecture in the last three chapters. In this chapter, you will dive deeper into these principles and cover the following main topics:

- Understanding what you should be able to do as a Salesforce solution architect
- Introducing the solution architecture domain mini hypothetical scenario: Packt visiting angels
- Selecting the appropriate functionalities to build your solution and presentation

## Understanding the Role of a Salesforce Solution Architect

A solution architect is usually the person that forms a bridge between business and technology. This architect will focus on creating the technical vision for solving a specific business problem. As a CTA, you are expected to lead and support both Salesforce and non-Salesforce solution architects during an implementation.

> Note
> According to the Salesforce online documentation, the CTA candidate should meet a specific set of objectives that can be found at the following link: https://packt.link/7kjFw.

By the end of this chapter, you should be able to do the following:

- Select the right combination of declarative and programmatic functionalities
- Augment your solution with the right external applications

Now have a closer look at each of these objectives.

## Select the Right Combination of Declarative and Programmatic Functionalities

It might sound simple, but you need to keep in mind that every suggestion you come up with as a part of your CTA review board solution should be clear and justified. The Salesforce Core Cloud comes with plenty of different capabilities and out-of-the-box functionalities.

While designing your solution, you should start considering configurable functionalities first. Then, transition to programmatic solutions only when the configurations fall short of meeting the desired functionality. Configuring an existing, mature system is usually much quicker than developing software from scratch. This is one of the main reasons enterprises buy readymade software solutions rather than developing them themselves.

You might find out that the configurable functionality delivers a sub-optimal user experience or leaves some questions unanswered. In this case, feel free to propose a programmatic solution. After all, the programmatic capabilities (such as Apex, **Lightning Web Components (LWCs)**, Visualforce pages, and others) of the platform are there for a reason.

Keep in mind that point-and-click development is quicker and less error-prone than code-based development. So, it should be considered first. You need to be up to date with the latest capabilities of some of the core platform tools, such as Salesforce Flows. Flows have recently evolved and matured well, and they are becoming a popular tool to build automation with clicks rather than code. But there are still many other functionalities that would require a programmatic solution of some sort: an LWC, a Visualforce page, an Apex-based trigger, a Batch job, or a custom Apex web service.

To select the right combination of configurable versus programmatic functionalities, you must have deep hands-on experience with the platform's capabilities. You are encouraged to try every single configurable solution proposed in this book. Make yourself familiar with the key sales, service, and marketing processes. For example, in sales, you need to understand the following:

- Understand the different mechanisms to import leads to the platform, including the import wizard.
- Understand the different out-of-the-box capabilities for lead deduplication and for deduplication in general, regardless of the object.
- Understand how other products could integrate with the core cloud to capture and import leads.
- Understand the typical activities included in lead management business processes, such as lead auto-responding, lead nurturing, lead assignments, and more.
- Understand some fundamental principles behind planning and setting up marketing campaigns.
- Have hands-on experience working with the standard Salesforce sales and marketing object, and familiarity with how to utilize additional products such as Marketing Cloud and Pardot to execute marketing campaigns.
- Understand how to set up recurring and automatic tasks related to leads and other objects.
- Understand how to use the `Product`, `PriceBook`, and `PriceBookEntry` objects and how to use them to model the items/services to sell.
- Have practical experience in the entire lead-to-opportunity conversion process, including the details on how the opportunities get assigned upon conversion. Furthermore, you need to understand how a sales rep can use the `Opportunity` object and supporting objects such as the quote to drive the sales process.
