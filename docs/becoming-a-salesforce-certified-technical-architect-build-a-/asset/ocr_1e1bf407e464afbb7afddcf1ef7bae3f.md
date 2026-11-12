There are object-level security mechanisms that control which objects are accessible for a particular user license. For example, the license granted for a particular user does not allow access to the opportunity object. In that case, all the higher-level security mechanisms will adhere to this restriction, which means that you would not be able to grant a user access to the opportunity via profiles or permission sets.

If you continue moving up the chart, you will come across the record-level security mechanisms, which control access to the records of a specific object—assuming that the user has access to the object from the lower object-level layer. This starts with the org-wide defaults (OWD), which set the default access levels for object records. For example, the OWD of the accounts could be private, public read-only, or public read-write. This defines the users’ access level for account records they do not own, assuming they have access to the account object.

Role hierarchies, sharing rules, and other sharing mechanisms can be used to grant users access to more records or higher privilege levels. For example, you can use criteria-based sharing rules to grant users in specific roles access to account records that meet certain criteria.

There are other factors that impact these tools and functionalities, such as the structure of the data model and the relationship types between the objects.

At the very top of the diagram, you can see the restriction rules, which is a functionality that has been recently introduced by Salesforce. You define restriction rules by specifying the record and user criteria that must be met to activate the rule. Once the criteria are met, the records that the user would have access to via the other mechanisms (such as OWD, sharing rules, manual sharing, and others) are filtered. This functionality provides a means to restrict access to specific records for some users. It is particularly useful with objects such as tasks, events, and contracts.

**Restriction rules** are available for the standard task, event, contract, timesheets, and timesheet entries objects, in addition to custom objects and External Objects.

Note You can learn more about restriction rules at the following link: `https://packt.link/bnsbu`.

The existence of this functionality should not be misunderstood or misused as an excuse to introduce a less efficient data control mechanism. This functionality is not meant to replace any of the other functionalities but to extend them to cover use cases that were difficult to control in the past. CTAs are expected to understand the full breadth of the sharing mechanisms available on the platform and make the best use of them.

**Scoping rules** are another functionality that you should be aware of. This is not included in *Figure 6.2* because it does not extend or restrict the user’s accessibility to records but simplifies it. Scoping rules control the records that the users see by default. However, the user is not restricted from accessing other records. Scoping rules are to help the users focus on a default set of records while still enabling them to look up other records if needed.