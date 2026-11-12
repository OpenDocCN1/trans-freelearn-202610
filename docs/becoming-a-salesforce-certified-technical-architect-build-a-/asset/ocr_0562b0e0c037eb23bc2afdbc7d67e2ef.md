You came across a similar requirement before in this scenario, but the channel was different. You decided to develop a custom UI on top of APIs exposed by MuleSoft, which are integrated with Power Sales. The UI is basically a set of **Lightning components**.

The field sales agents can use the same components on their tablets, assuming they are using the **Salesforce mobile app**. The new Salesforce mobile app supports Lightning app pages, which you can logically assume the custom UI will use.

Update your landscape architecture diagram and list of interfaces and move on to the next requirement, which starts with the following:

*Three days after completing the visit, an email survey should be sent to the B2B customer’s primary contact.*

Two different survey templates should be used, one for successfully signed deals and another for lost deals. If the score of the **Field Sales** agent is below 3 out of 10, a **Case** should be automatically created and assigned to the field sales agent’s manager.

This requirement is becoming more common nowadays as more companies are trying to get closer to their clients and show them that their opinion matters. You have three potential ways to solve this requirement:

+   • Develop a custom survey module based on custom objects and automation.
+   • Use a third-party tool such as **SurveyMonkey**.
+   • Use **Salesforce Surveys**.

Salesforce products come with the regular Salesforce promise that they will be updated regularly, and new features will continuously be included. Furthermore, they are products designed for Salesforce, so they integrate with the platform in a more straightforward way than others, and they usually simplify other non-functional requirements, such as data residency.

On some occasions, the Salesforce product will lack certain functionalities, which justifies considering a third party instead. This is not the case here. The mentioned requirement can be fulfilled using Salesforce Surveys.

Similar to other extension products (such as **Salesforce Field Service** and **Vlocity**), you need to know how the product works and have a good understanding of its components. The most common element you need to be familiar with is the data model. Add the key objects you plan to use from Salesforce Surveys to the data model diagram.

**Note**
You can find more information at the following link: `https://packt.link/DWKUy`.