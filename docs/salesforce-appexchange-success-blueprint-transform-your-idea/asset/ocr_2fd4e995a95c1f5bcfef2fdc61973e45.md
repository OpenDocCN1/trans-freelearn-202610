flow version installed from a package. For ISVs, manage versions differently based on whether you use 1GP or 2GP packaging processes.

2 https://help.salesforce.com/s/articleView?id=sf.flow_considerations_limit.htm&type=5

+   - Customers can deactivate flows in managed packages. If a new, active version of a flow is included in the next release, it will be created and executed in the customer’s org, potentially causing unexpected behavior. If you have logic that must not be deactivated, consider using a code-based approach.
+   - If there’s a possibility that a customer might decide to deactivate a flow, it’s best not to set it as IP-restricted. When these flows turn back on by themselves after an upgrade, it can confuse customers. Additionally, this can complicate the process of implementing a solid release management strategy, which we will delve into in *Chapter 7*.

By offering flow templates and overridable flows, ISVs can empower customers to make their own customizations and meet their specific business needs, enhancing the value of the ISV’s application.

# Blending Flow and Apex