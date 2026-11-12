However, the differentiating requirement is the one related to the UI. PUB is looking for a sophisticated and bespoke UI that drives a certain user experience. In this case, your best bet would be a native app. Your proposed solution could be as follows:

> Considering the requirement of having a bespoke, modern, and sophisticated mobile UI, I propose developing this mobile application as a native app. Perhaps we can utilize the mobile app developed by Germany and update it to use the Salesforce mobile SDK, or simply build it from scratch. Customers would still utilize a customer community license to authenticate to Salesforce. The mobile app would utilize the OpenID Connect user-agent flow.

You need to explain the solution from end to end. Do not leave any loose ends, for instance, skipping the part explaining how the users would authenticate to Salesforce. Your solutions must be clear and solid. Note that for customers, you are proposing using Salesforce as the identity provider. This should be fine, considering that the scenario mentioned that Azure AD is mainly used for internal users.

> Note
> Remember to add the customer’s native mobile app and its integration interface to your artifacts.

Moving on to the next paragraph, it is as follows:

PUB currently communicates little with its customers. PUB would like to be able to send regular marketing materials to its customers based on their preferences.

The question that should come to your mind is: What is the most suitable solution in the Salesforce ecosystem for this requirement? You need significant customizations in core clouds to build this capability, and you will still run into different limitations.

An external system is an option, but the most obvious solution and the one that offers the best out-of-the-box integration with the core cloud is Marketing Cloud. Marketing Cloud can be used to segment customers based on their preferences. Your proposed solution could be as follows:

> I am proposing using Marketing Cloud to fulfill this requirement. Marketing Cloud can be used to segment customers based on their preferences. Taking into consideration the regulation requirements, there will be a different Marketing Cloud instance per region. The standard Marketing Cloud Connector can be used to integrate the regional core cloud with its equivalent Marketing Cloud instance. The contact ID in the core cloud will be used as a subscriber key in Marketing Cloud.

There is a continuous growth of cross-cloud architectural concepts. As a CTA, you are expected to have a very solid knowledge of the Salesforce Platform (mostly, Force.com, or what is also known as core clouds). However, you still need to have some familiarity with the common tools and technologies from the Salesforce ecosystem and how they interact with each other.