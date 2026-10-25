The following figure depicts an example architecture for multi-tenant BI analytics:

![](bbox=[0.1621, 0.1328, 0.8379, 0.5344])

Figure 10.1 – Multi-tenant architecture

The main components of the architecture are as follows:

+   • **Custom namespaces**: These are to organize the users from different tenants.

+   • **Default namespace**: These are for Admin users.

+   • **Amazon SPICE and datasets**: These are specific to a QuickSight account. Access to these resources is managed separately using the QuickSight API, as we learned in _Chapter 3, Preparing Data with Amazon QuickSight_, and _Chapter 9, Managing QuickSight Permissions and Usage_.

+   • **Embedded portal and Identity Store**: This allows users into the QuickSight console. For those use cases that don't need an embedded web portal, the QuickSight web app can be used instead. The embedded portal can be any custom-built web app. Implementation details on how to build web applications are outside the scope of this book.

Now that we have learned the fundamentals of using multi-tenant architectures in QuickSight, in the next section, we will look at a hands-on example.