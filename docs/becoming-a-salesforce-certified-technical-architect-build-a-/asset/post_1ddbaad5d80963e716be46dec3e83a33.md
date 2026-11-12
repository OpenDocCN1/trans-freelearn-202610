
## Accessing the Online Content

If you've already created your account using those instructions, visit pact.link/ctabookwebsite or scan the following QR code to quickly open the website.

![](img/a3b1eecf3c87c814267f2877e8a8be1a_92_0.png)

Figure 3.2 - QR Code to access CTA online resources

Once there, click the Login link in the top-right corner of the page to access the content using your credentials.

In this chapter, you will continue to discover the general architectural concepts that a Salesforce CTA should be familiar with. In today's world, it is rare to come across a single implementation where Salesforce is completely isolated. In most cases, Salesforce will become the heart of the enterprise's business transformation process, which means that it must relate to dozens of other new and existing applications. The cost of integration is sometimes overlooked or underestimated, despite several studies pointing out that around 25-35% of the total project cost would likely be spent on integration.

Coming up with the right integration architecture is crucial to the success of a Salesforce project. Moreover, securing the internal and external integration interfaces is becoming more and more important, especially with the progressive move toward API economy and data monetization. The Salesforce Architect is expected to be able to design a secure and scalable integrated solution.

In this chapter, you will discover general architectural concepts about integration and cryptography. Building that breadth of knowledge will help you dive deeper into Salesforce-specific topics in the chapters to come. In this chapter, we are going to cover the following topics:

- Integration in the enterprise: Understanding the landscape
- Introducing common integration styles
- Discussing different integration tools
- Exploring modern integration approaches
- Cryptography: Understanding the general concepts
- Cryptography algorithm types and use cases

Now, let's explore the first bullet in the list.

### Integration in the Enterprise: Understanding the Landscape

The digital enterprise landscape is continuously becoming more sophisticated. Gone are the days when the enterprise used to have less than 10 systems covering most of its business processes. Today’s enterprises have hundreds, if not thousands, of different applications that are bought, built in-house, or combined. This is in addition to a set of legacy systems that are still surviving the axe. Nowadays, it is very common to find that an enterprise has dozens of websites, multiple instances of ERP systems, and many other departmental applications, in addition to several data warehouses or lakes.

One of the reasons why enterprises end up in such situations is the complexity associated with building business applications. Building a single application that runs all business processes is nearly impossible. Maintaining it, adapting to day-to-day business challenges, and requesting changes are even more challenging. Breaking business functions into smaller chunks of applications has always made sense. It provides the business with enough flexibility and agility to move at the pace they need, rather than being bound by the technical boundaries of a bigger all-in-one solution. Moreover, it gives the business the opportunity to pick and choose the best suite of applications that best serve their needs. This would mean combining the best **customer relationship management (CRM)** system with the best **order management system (OMS)** and **enterprise resource planning (ERP)** solution.

In the past 20 years or so, vendors have offered focused applications for specific core functions. There has been the continuous addition of functionalities to existing applications, which has caused some sort of functionality spillover. For example, several pieces of customer care software have had extensions to include a limited billing functionality due to the difficulty of drawing clear functional separations between systems. For instance, if a customer raises a dispute for a bill, will that be considered something to be handled by customer care or billing applications?

On the other hand, users do not really care about these boundaries or the systems that are behind the scenes. They expect a business function to be executed, regardless of the system or systems involved in delivering it. For example, when a customer places an order online, this will likely require coordination between multiple systems to deliver several functionalities, such as checking the history or credit score of the customer, checking inventory, computing tax, creating the order, fulfilling the order, handling shipment, sending the bill, collecting payment, and more. These processes can span many systems, but from the customer’s perspective, that was a single transaction.

To support these distributed functionalities, which are still expected to work as a coherent business process, these applications need to be integrated in an efficient, secure, scalable, and reliable fashion.

The typical enterprise integration needs are as follows:

- **Get the right information**: Get precise knowledge of a particular piece of information created by different systems and enterprise business processes. This knowledge must be structured in a way that can support other business needs.
- **Get that information to the right place**: This requires mechanisms to handle information transactions across heterogeneous environments, which may be based on different technology stacks and reside on different servers/hardware and operating systems.
- **Get that information at the right time**: Ideally, this requires distributing the information in real time to reflect the actual state of a particular data entity.
- **Flexibility and embracement of change**: This is done to adapt to external factors, such as market demand, customer shift of behavior, new legislation, or a shift in social philosophy.
- **Coordinate business processes**: This is a challenging operation that may require modeling the enterprise business's processes, how they are interlinked, and what kind of information they exchange. This may require having a deep understanding of the business and certain knowledge of the enterprise.

So, what makes a good integration architecture? You will find that out in the next section.

### Integration Architecture Design Principles

Like any complex technical architectural topic, there are several considerations and consequences that you need to keep in mind while designing the target integration strategy and architecture. Integration is one of the key areas where _sub-optimal_ designs could have a significant impact on the overall solution.

The main decision points are usually as follows:

- **Native integration**: Simply put, if you can develop a single standalone application that can fulfill all business needs on its own, without any need to collaborate with any other application, then you can avoid a lot of complexity driven by integration requirements. However, this is not something you can normally achieve. Several attempts to extend an application so that it includes other functionalities can end up creating a complex system that is hard to manage and maintain, slow to react to market needs, and hard to adjust to meet new business requirements.

This problem exists in the Salesforce world as well, although in a more limited way. Many Salesforce products are natively integrated, with many considered the best in the market, and have the ability to deliver different user experiences. For example, Salesforce communities provide a native solution to exposing a customer portal that is natively integrated with your CRM.

Salesforce communities offer a very good sharing model and an easy way to control the look and feel of the community itself. It makes sense to favor that over technology, then figure out a way to integrate it with Salesforce in a secure and compliant way. Moreover, using the native integration keeps the doors open for using other natively integrated features in the future.

- **Simplicity:** This goes beyond the integration architecture. Simplicity is an aim that architects and developers should always strive for. Avoid complicated solutions as much as possible and always keep the golden 80-20 rule in mind.

Fulfilling 80% of use cases using a simplified architecture should be preferred over targeting a solution that covers 100% of use cases using an over-complicated architecture. Keep your integration code simple and tidy. In some use cases, you may still need to introduce some complex modules to deliver highly sophisticated functionalities.

- **Application dependencies:** Integrated applications should have minimal dependencies on each other. This allows solutions to evolve independently without being tied to each other’s roadmaps. It also allows you to replace an application completely without it impacting the other integrated systems.

Tightly coupled applications have many dependencies on each other and rely on many assumptions regarding how each of them works. When an application is modified, the assumptions could change, which would, in turn, break the integration.

In a loosely coupled integration, the integration interface is specific enough to deliver a particular functionality but generic enough to allow for a change if needed.

- **Timing:** Ideally, the integration architecture should aim to minimize the duration between the moment an application is sending data and another application is receiving it. The integration architecture should aim to share small chunks of data as frequently as possible, rather than waiting to exchange a huge block of data that may not necessarily be related.

Data sharing latency should be taken into consideration while designing the architecture. The longer a data exchange process takes, the more likely it will become more complex and prone to other challenges, such as a change in the data’s state. Bulk data exchanges can still be used for the right use cases, such as archiving operational data.

- **Synchronous versus asynchronous:** In a synchronous process, a procedure waits until all its sub-procedures finish executing. However, in an integrated environment, where the integrated applications might be on different networks or might not necessarily be available at the same time, you may find more use cases where the procedure does not have to wait for all its sub-procedures to conclude. It simply invokes the sub-procedure and then lets it execute asynchronously in the background, making use of the multi-threading ability available in many of today’s applications.
