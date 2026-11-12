
Update your actors and licenses diagram, your landscape architecture diagram, and your integration interfaces accordingly. Now, look at the next requirement:

POW would also like to expose a rich set of searchable FAQs to their consumers to help deflect as many cases from their call center as possible.

Salesforce Knowledge is a powerful tool you can use to build an internal or external knowledge base. Community users can be configured to have read access to specific article types using profiles and permission sets. Note that the org must have at least one Salesforce Knowledge license to enable and set up Salesforce Knowledge. Your proposed solution could be as follows:

> To fulfill this requirement, I propose using Salesforce Knowledge. I will control the community user's access to specific article types using profiles and permission sets.
> This new feature will be part of the new Salesforce Customer Community that will be created for POW.

Update your landscape architecture and move to the next requirement, which is as follows:

Registered consumers should be able to view their purchase history for up to four years.

According to the scenario, there are 150,000 daily orders, with an average of five items in each. This means there are nearly 4.5 million orders every month and 22.5 million order line items. In four years, those numbers will become massive, at around 216 million orders and more than a billion order line items. These two tables are categorized as LDV objects on two fronts, that is, the monthly data growth and the total number of records per year.

You are also still unclear on the system that will be used for order fulfillment. Considering the amount of data, you could propose an off-platform solution, such as using a custom Heroku solution, an extension to the ERP, or a completely new system.

However, according to the scenario, orders are usually fulfilled in no more than two weeks. This means you can use an on-platform order fulfillment solution to handle open orders and use an archiving mechanism to move fulfilled orders off the platform. The number of open orders stored on-platform at any given time would not result in creating an LDV object.

There are several order management solutions on AppExchange. Salesforce also has its own solution called Salesforce Order Management, which was introduced back in 2019. The scenario itself does not dive into the details of how the orders will be fulfilled, which means that even if you do not fully know how Salesforce Order Management works, you can still propose and assume its capabilities are good enough for POW.

Delivering this functionality using an on-platform solution will allow you to cover other upcoming requirements. Therefore, it is recommended to skim through the scenario at least once before you start the solution. This will help you build an early idea about some key parts of the potential solution.

You can still propose an off-platform solution, but you need to ensure that it can still fulfill all of POW’s requirements.

> **Note**
>
> Be careful of shifting more duties than you should to off-platform applications. This could indicate you do not fully understand the platform's value and capabilities, nor the benefits of building your solution on top of a flexible platform such as Force.com, which offers native integration with other CRM functionalities.
>
> In short, when proposing an off-platform solution, make sure it adds value and solves a problem that cannot be optimally fulfilled using on-platform capabilities.

For this requirement, you can choose a solution such as the following:

- POW’s website is used to capture a massive number of orders every day. I made some calculations and found out that this will be nearly 4.5 million orders and 22.5 million order line items every month. In four years, there will be nearly 216 million orders and more than a billion order line items. That is a massive amount of data to keep in Salesforce.

- I thought of using an off-platform order management solution to handle this, but this would mean we lose a lot of the out-of-the-box functionalities available on the Salesforce Platform. We also know that users are normally fulfilled in no more than two weeks. Therefore, I am proposing using Salesforce Order Management to handle the order fulfillment requirements. In addition, I propose introducing an archiving mechanism where all fulfilled orders are copied to Heroku and deleted from Salesforce.

- I thought of using Heroku Connect for the data sync, but I know that it does not provide a mechanism for partially syncing data, and I only need to sync the orders that have been fulfilled. Therefore, I propose using an ETL tool such as Informatica to sync the data. It will run on a scheduled basis, picks all fulfilled orders from Salesforce and copies them to Heroku, and then deletes all successfully synced orders. The sync job includes both orders and order line items.

- We could use MuleSoft for this, but considering the amount of data that needs to be moved, I prefer to use an ETL tool designed for this type of heavy-duty data transfer rather than MuleSoft, which is essentially an ESB.

- Once the records are in Heroku, I will expose them back to the community users using Salesforce Connect and External Objects. Users will be authenticated using a named principal/service user. I will develop a Lightning component to display the External Objects as this will allow me to control which records to show to the user. I will determine the retrieved records using SOQL statements in the component's controller, ensuring that every query has a where statement that limits the retrieved records to the currently logged-in user ID.

I will authenticate to Heroku using an OAuth 2.0 web server flow. I will use named credentials in Salesforce to set up the Salesforce Connect authentication mechanism.

The community users will be able to view the open orders on one page and all previously fulfilled orders on another page.

Again, remember that this is one suggested solution; it is not the only possible solution. What you need to deliver is a solution that works, fulfills all requirements, and is technically correct. You also need to justify your solution and explain the rationale behind selecting it. You will need to defend your solution during the Q&A section. This would not be difficult if you are confident in your logic. While explaining such topics, keep an eye on the timer. The preceding statement should be covered in no more than 180 seconds.

Now, update your diagrams, including your data model (so that it includes the External Objects), and move on to the next requirement, which starts with the following line:

Registered consumers should be able to review an item they have purchased.

These reviews should be publicly available to both registered and non-registered website visitors. You have nearly 2 million products. You do not have a clear idea of the average number of reviews, so you can make some assumptions. You also do not have additional requirements for the product review capability, apart from the fact that it should be publicly available for registered and unregistered customers.

Typically, product reviews allow other users to rate the review, and probably even respond to it. However, POW has not requested such a feature. You are free to make your own assumptions here.

The next question in line is whether there is an out-of-the-box feature that fulfills this requirement, or do you need to consider a custom solution? And what kind of assumptions do you need to make to avoid having an LDV object?

Your proposed solution could be as follows:

To fulfill this requirement, I propose using Salesforce Chatter. Chatter posts can be made visible to unauthenticated community users via an API by setting the value of the Give access to public API requests on Chatter flag (via community preferences) to true. Registered users can post reviews related to a particular product.

The product OWD will be set to public read for external users. I will enable the unauthenticated community users to view the products by developing a Lightning component that uses an Apex controller operating without sharing considerations (using the without sharing keyword).

Considering 2 million products, this could end up with many additional Chatter records being created by reviews, likes, and comments. I am assuming a limit of three reviews per product. I am also assuming that POW is fine to remove reviews
