> Salesforce does not automatically grant access to applications, tabs, pages, objects, and fields deployed using the Migration Tool. So, you will find that before you can see these components, you need to assign your Permission Sets to your developer org user. Doing this is a good way to ensure that these are kept up to date with new components and are valid. In general, testing as a system administrator is not a reliable confirmation of your application working in an end-user scenario. This is why Salesforce has adopted this policy – to make the developer think about which permissions need to be applied, even in a developer org.

![](bbox=[0.14, 0.13, 0.19, 0.17])

# # Developing in developer orgs versus packaging orgs

Now that you have moved out of the packaging org and into a developer org, you will no longer be developing or executing code within the associated namespace context that it provides. If you recall back to the chapter around execution contexts, remember that the debug logs from Apex code executed in the packaging org had two namespaces referenced: the packaging org one and the default one. While everything runs in this namespace implicitly in the packaging org, nothing would ever run in the default namespace.

As you will have noticed, when writing the Apex code in the packaging org, you don't have to prefix Custom Objects or fields with that namespace the way you would have to in a subscriber org where your package was installed. Instead, in the packaging org, this is done implicitly on the whole, which made it easier to export into Source Control, as the target developer org environments do not have the namespace assigned to them (because it can only be assigned once).

When developing and executing code in a developer org, everything runs in the default namespace. Keep in mind the following differences when developing in a developer org:

+   - Apex features that can help implement Application Apex APIs such as the @deprecated annotation and the Version class that are not supported outside of the packaging org and results in compilation failures in the developer org. The main downside here is the lack of ability to use the @deprecated attribute as you want to retire old Apex APIs. Behavior versioning using the Version class is not something I would recommend for the reasons outlined in Chapter 9, Providing Integration and Extensibility.