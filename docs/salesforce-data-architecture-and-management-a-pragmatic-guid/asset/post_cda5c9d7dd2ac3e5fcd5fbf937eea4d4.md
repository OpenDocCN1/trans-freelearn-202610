

- Group membership locks: There are a few operations that cause an org-wide lock. This means that you cannot use parallel processing in this case because once a lock has been acquired on the group membership tables, no other updates can be made unless the lock is released. Some of these operations are as follows:
  - Adding users who are assigned to roles
  - Changing user roles
  - Adding a role to the role hierarchy or territory to the territory hierarchy
  - Adding/removing members from the public or personal groups, roles, territories, and queues
  - Changing the owner of an account to a new owner while this new owner has a different role than the original account record owner. This applies to accounts that have a community role or portal roles associated with them

Ensure that your code executes in such a way that the lock on group membership objects is minimal and other code is not causing the locks to be prolonged. When group membership type operations are executing, the run must be in serial mode due to the org-wide record locking when these operations are run.

- Overlapping runs: Multiple running jobs that overlap can cause unintended lock contention on objects, code, and workflow rules that Salesforce runs as part of the job. Ensure that there is sufficient time between jobs so that the previous one has run completely before the subsequent job.

- Roll-up summary fields: When records on the detail side are being updated, the master record is also locked to ensure that the roll-up summary is updated. This can cause lock contention and, if possible, these should be removed before loading data. If that is not possible, then consider using summary reporting on the detail object if it has all the data that's required or a report that spans the master and detail objects. If that is not an option, then order your master object records and avoid having the same master record in multiple batches.

As seen, there are many considerations for efficiently using the Bulk API compared to the other APIs and, usually, the complexities associated with the Bulk API are abstracted by third-party applications that leverage the API. Nonetheless, the Bulk API is the API of choice for data migrations and integrations when large volumes of data are under consideration.

## The REST API

The REST API is a web services API that can be used to interact with data in Salesforce. It is considered a lightweight API that uses the request and response framework and is widely used for web and mobile applications.

This API uses the same underlying Salesforce data model for accessing data that the SOAP API uses and unlike the Bulk API (also RESTful based), which has its own limits, the limits for the REST API are the same as the SOAP API.

REST API operations are performed by interacting with Uniform Resource Identifiers (URIs) that access Hypertext Transfer Protocol (HTTP) methods such as GET, HEAD, and POST. You can use these URLs to perform queries, update records, or run describe operations on objects. Figure 8.3 shows the critical steps of how a REST-based application interacts and accesses resources within Salesforce:

![](img/782d45700e093b3d82b348c0ccc8ccba_346_0.png)

Figure 8.3 – Critical steps to access REST resources using an external application

The REST API is a widely used API that is secure and is used for lightweight applications that are not data intensive. When used with OAuth as the authentication mechanism, it is more secure than SOAP as it eliminates the need to store any credentials and the token in the client app.

## The Streaming API

The Streaming API is used for custom integrations or near real-time applications and is not meant for data migrations. This API is based on Event-Driven Architecture (EDA), meaning that data updates are published to a messaging bus. Subscribers to those updates can consume the event on their end. This approach is different than the SOAP or REST API approaches where data is pulled from the system.

In EDA, messages or events are published to a central location; in this case, it’s the **Enterprise Messaging Bus (EMS)** and systems that are interested in those events subscribe to it. Whenever an event is published, it is published on the bus and all consumers that subscribed to it can use it. This type of architecture decouples systems that produce these events from systems that consume these events, leading to a scalable architecture. Figure 8.4 shows how EDA works:

![](img/782d45700e093b3d82b348c0ccc8ccba_347_0.png)

Figure 8.4 – Event-Driven Architecture

The enterprise messaging bus is part of the technology on which the Streaming API is based. This is called **CometD** and uses the messaging bus to publish and subscribe to events. The Streaming API has multiple flavors; two of the most commonly used ones are **Platform Events** and **Change Data Capture (CDC)**. The earlier flavors of the Streaming API were **Generic** and **PushTopic**, both of which required code for publishing or subscribing to an event. With Platform Events and CDC, there is more flexibility as they are supported both by code and other declarative features in Salesforce, such as Salesforce Flow or Process Builder.

### CometD

Visit the CometD website at CometD.org to learn about the features and detailed documentation about CometD.

In this section, we reviewed the four most commonly used APIs from a data perspective, learning about optimal use cases for them and how they work. We discussed in detail the Bulk API because that is the API of choice when dealing with large volumes of data, a topic that we covered extensively in *Chapter 7, Working with Large Volumes of Data*.

## Summary

Data migration is an exciting topic because regardless of whether you are migrating 1,000 records or 1,000,000 records, there are certain things that you must keep in mind for smooth data migration. Best practices related to the different phases of data migration that you learned will help you to conduct a successful migration without running into any major obstacles. The key thing is to spend time doing proper analysis before jumping into the execution phase of the data migration. That will save you a lot of headaches and wasted effort down the road.

You also learned about the standard Salesforce tools available to us for data migration along with their constraints. As mentioned earlier, for simple migrations, these may be sufficient, but for complex migrations, more sophisticated third-party tools are available that make the job much easier and smoother.

We concluded the chapter by discussing APIs that are provided by Salesforce. Although there are a lot of other APIs that Salesforce provides, we covered the ones that are pertinent to data migrations and integrations. You learned about the general architecture of each API and how to use them, alongside a detailed discussion on the Bulk API. We also covered EDA during our review of the Streaming API and how the Streaming API supports scalability and near real-time integrations.

Congratulations on getting through the last chapter! I hope that you had fun reading it while also learning new concepts and techniques that you can use to add value to your organization.

## Questions

1. **Precision Printing (PP) has recently acquired another company and is looking to migrate CRM data into their main Salesforce instance and decommission the legacy CRM system. The legacy system has 7 million records. What API should be used to load this data into Salesforce? (Select one answer)**

   - a) REST
   - b) Streaming
   - c) SOAP
   - d) Bulk

2. **The Salesforce team at Centauri Logistics is looking to integrate an external text messaging-based notification system with Salesforce. A text message will be sent to the customer as the shipment is received at a location. Drivers scan a location-specific barcode as they arrive at the location. The team anticipates other systems in the future may be interested in this data as well. Which API should be used for this integration? (Select one answer)**

   - a) Streaming
   - b) Bulk
   - c) SOAP
   - d) Metadata

3. **The team at PP has been hard at work and has designed a solution to load 7 million records into Salesforce. They start the run but immediately run into failed records with locking errors. The team is confused because the API is being run in parallel mode. What can the data architect suggest to fix these errors? (Select one answer)**

   - a) They should run the load in serial mode.
   - b) They should ensure that parent and child records are in the same batch.
   - c) They should switch over to use the SOAP API.
   - d) They should open a case with Salesforce requesting that support allocates more CPU and memory resources to the server.

