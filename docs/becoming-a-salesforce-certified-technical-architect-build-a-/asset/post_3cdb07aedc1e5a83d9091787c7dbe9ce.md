
Lookup skews are hard to spot. Reviewing the data model to understand the object relationships and then comparing it with the data usage patterns and the record volume in each object is a good approach to start with.

You typically focus more on objects with a vast number of records and those with a high number of concurrent insert and update operations.

## Ownership Skew

Ownership skew happens when the same user owns several records of the same object type. All records must have an owner in Salesforce, and sometimes, a business process might park records or assign them by default to a single specific user. However, this could have an impact on performance due to the activities required to calculate the sharing records. Salesforce uses Share Records of Share Objects to control access and visibility when sharing a private object. For example, records of the object AccountShare are used to grant access to records from the Account object (if the OWD of the Account object is set to private). These sharing records can be created in multiple ways, including Apex-managed sharing.

Ownership changes are considered expensive transactions in Salesforce. Every time a record owner is updated, the system removes all sharing records that belong to the previous owner and parent users in the role hierarchy. In addition, the system removes sharing records associated with other users, which are created by sharing rules or other sharing mechanisms, such as manual sharing.

You can minimize the risk of ownership skew by distributing record ownership to multiple users or by ensuring the assigned owner does not have a role and, therefore, does not belong to the role hierarchy. You can also ensure that the assigned owner belongs to a role at the top of the hierarchy.

## Determining the Data Migration Approach, Tools, and Strategy

Data migration is a typical exercise for Salesforce projects. While it is an extensive topic, you are expected to explain your data migration strategy’s high-level details during the CTA review board. You will go into far more detail in real life and include many other factors in your decision-making.

## Key Topics to Consider in Your Data Migration Strategy

During the review board, you need to keep an eye on crucial topics such as the following:

- The record count is crucial, that is, how many objects you are going to migrate and the number of records in each object.

- The number and type of *involved systems* and the mechanism of extracting the data from them. You might need to assume that the data will be provided in a specific format and based on a shared template to limit the risk from your end.

- You need to determine which data migration approach works best for you. The two key approaches are the big bang and ongoing accumulative approaches.

- You can derive other approaches from the key approaches, such as a local big bang executed sequentially across different regions/business units, an ongoing migration approach where more than one live system is maintained and the delta of the data is migrated regularly, and so on.

- You need to consider the different *governor limits* that could be impacted, such as the daily inbound API calls limit.

- Triggers and workflows execute specific logic based on specific criteria. They also take time and CPU power and consume governor limits while executing, which could be a risk when loading a massive amount of data. As a best practice, you are advised to turn them off or provide a mechanism to bypass them during the data load.

- Use the Bulk API if possible. This could be a simple switch if you are using a modern ETL tool.

- Use the Deferred sharing calculation feature described earlier in the *LDV Mitigation Tools* section. Then, activate sharing rule calculation once you have finished loading the data.

- When loading child records, try to group them by their parent ID.

Moreover, you need to propose the right tool based on your assumptions, experience, and logic.

## Data Backup and Restore

Salesforce announced that effective July 31, 2020, data recovery, as a paid feature, would be deprecated and no longer available as a service. However, based on customers’ feedback, Salesforce decided to reinstate its data recovery service. In Autumn 2021, Salesforce announced a new built-in platform-native backup and restore capability.

Be prepared to explain how your solution will fulfill data backup and restore requirements (if specified in the scenario).

You can review *Chapter 2, Core Architectural Concepts: Data Life Cycle*, to find out more about the different approaches and tools to achieve this.

## Introducing the Data Architecture Domain Mini Hypothetical Scenario—Packt Online Wizz

The following mini scenario describes a challenge with a particular client. The scenario has been tuned to focus on challenges related to *data architecture* specifically. However, this domain is tightly related to the *security domain—sharing and visibility* in particular. Therefore, you will still notice a considerable amount of sharing and visibility requirements. There will be other scenarios in later chapters that also have dependencies on the data architecture domain.

You will go through the scenario and create a solution step by step. To get the most out of this scenario, it is recommended to read each paragraph, try to solve the situation yourself, then come back to this chapter, go through the suggested solution, compare it with yours, and take notes.

Remember that the solutions listed here are not necessarily the only possible solutions. Alternate solutions are acceptable as long as they are technically correct and logically justified.

## Scenario

**Packt Online Wizz (POW)** is an online shopping portal where traders offer different items to consumers. POW is currently dealing with over 100,000 different traders. POW currently estimates that they have over 2 million unique products for sale, organized into 150 different categories. According to their current website traffic, more than 5 million visitors visit their website every day. They place around 150,000 orders every day. Each order contains an average of five items.

## Current Situation

The current website allows consumers to place their orders anonymously or as registered users. They currently have 3 million registered users. The website also supports all major online payment methods, such as PayPal and credit cards. POW shares the following information:

- Once an order has been placed, it can be fulfilled via either POW or the traders themselves, depending on the preferences provided by the traders for each of their products. Orders are usually fulfilled in no more than two weeks.

- POW stores the bank account details of all its traders. Once the item is shipped to the consumer (either by POW or the traders), POW automatically calculates its commission, deducts it from the collected payment amount, and deposits the remainder into the trader’s bank account.

POW has a global support center containing 2,000 support agents and 100 managers operating 24/7 across phone and email channels. POW shares the following information:

- The support center handles roughly 20,000 cases per day.

- Each support manager leads a team of 20 support agents. Cases are available for all support teams.

POW has a global ERP system to handle billing, inventory management, and order returns. They use a bespoke application as an item master and run their custom website on AWS. They utilize MuleSoft as an integration middleware between all systems.

## Requirements

POW has shared the following requirements:

- The company would like to provide a convenient self-service experience to its consumers, allowing them to raise support tickets and inquiries.

- POW would also like to expose a rich set of searchable FAQs to their consumers to help deflect as many cases from their call center as possible.

- Registered consumers should be able to view their purchase history for up to four years.

- Registered consumers should be able to review an item they have purchased. These reviews should be publicly available to both registered and non-registered website visitors.

- The support agents should have a 360-degree view of the consumer who raised a specific case. This should include the customer’s history from the past four years, other cases that were raised in the past four years, order returns, and any product reviews from the customer.

- The support agents should be able to see the overall consumer value indicator, which shows the value of this consumer across their entire history of engagement with POW. This indicator should take into consideration several factors, such as purchased products, monthly spending amount, purchase quantities, and the customer’s social media influence factor. The social media influence factor is a value POW gathers and updates daily from three different providers.

- Traders should be able to view their customers’ purchase history for up to four years.

- Traders should be able to log in to the POW trader’s portal to view their current pending orders. They should also receive an email notification once a new order is placed. All emails should be sent to a specified corporate email address.

- Traders should be able to raise support tickets for themselves to resolve issues related to payments and inventory-related queries.

- POW’s top management should be able to run reports and dashboards to track the performance trends of items across different categories. The dashboards should include information such as the number of items sold, returns, and the total weekly value of sold items in a particular period.

- Historically, POW used a custom-developed solution to manage consumers’ complaints. POW is looking to decommission this system and replace it with a new solution. The current system contains more than 200 million records, including some that are still undergoing processing.

POW is concerned about the system’s overall performance and would like to get clear recommendations from you, as an architect, about designing the solution to ensure it has the best possible performance.
