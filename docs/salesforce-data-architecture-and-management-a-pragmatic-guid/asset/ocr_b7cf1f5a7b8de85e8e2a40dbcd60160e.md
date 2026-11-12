Lookup skews can still happen even if multiple objects have a lookup to the same object; for example, objects x, y, and z each have a lookup to object a. If record ID ax1 on object a is the parent record for say 50,000 records on object x, 10,000 records on object y, and 20,000 records on object z, you will still have a lookup skew situation. Lookup skews can be avoided by employing some of the following methods:

**Improving record save performance** – This can be done by doing the following:

+   a) Increasing the performance of your synchronous Apex code – tune your triggers for peak performance by consolidating code into a single trigger object.

+   b) Removing unnecessary workflows, process builders, or flows or consolidating into existing trigger code. Keep in mind that these automations lengthen the lock duration, which can cause increased lock failures. Removing or consolidating workflows into Apex can improve performance.

+   c) Processing only what's required synchronously and moving the rest of the code to asynchronous processing since locks are held for the full duration of the custom code execution.

**Distributing the skew** – Consider adding additional values to the lookup to avoid the skew or leaving it blank, if possible. This is because lookup issues are caused when many records have a lookup relationship to a single record. For example, a business provides quick budgetary quotes to walk-in customers without taking all of their details and creating an account in Salesforce. A generic account record has been created for this purpose. Over time, as quotes get created under this generic account, a lookup skew situation will arise that can then start to impact performance.

**Using a picklist field** – If there are a few lookup values, consider using a picklist value. By using a picklist value, you eliminate the locks associated with lookups and, hence, any locking issues.

As can be seen, something as innocuous as a lookup can lead to serious performance issues. This is a good reminder that we need to consider the consequences of our design and design for scalability and performance.