+   - The CRM system is implemented with Dynamics 365, and Power Platform with Power Automate is also in use.
+   - The internal invoicing system is represented by Contoso Invoicing (a desktop application), and the ERP system is accessible via a database (SQL Server).

At this point, we probably already have enough information to sketch an initial idea for the overall process. This could look as follows:

![](bbox=[0.127, 0.301, 0.872, 0.508])

Figure 12.15 — Automation project scenario

Now, as always, there are several ways to implement this requirement, but there is a lot to be said for using a UI flow for this:

+   - The processing should be started individually by a team leader, which can be enabled via a shortcut on the desktop thanks to the advanced features.
+   - Outlook actions enable us to retrieve and process emails.
+   - We can extract the relevant information by using the text actions from *Chapter 11* (**Parse text** or **Recognize entities in text**). Of course, it would be beneficial if the email were standardized, making it easier to identify the customer ID.
+   - The next step would be to call the Dynamics 365 web API, retrieve all opportunities for the customer, and close these. An HTTP action will help us here to accomplish this action (**Invoke web service**).