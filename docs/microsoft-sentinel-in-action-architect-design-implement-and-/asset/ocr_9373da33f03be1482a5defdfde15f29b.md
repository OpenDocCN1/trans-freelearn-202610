# Planning a workspace

While it is easy to just go and create a workspace, it is better to plan out the workspace configuration beforehand to avoid having to perform reworking later. Some of the aspects to consider include the following:

+   - **The name of the workspace**: This must be unique across all of Azure. It should be descriptive enough to show what service this workspace provides just by looking at the name. It is recommended that your company name and the word `Sentinel` are used in the name.
+   
+   If this raises concerns that the name will make it a bigger target for bad actors, use whatever naming convention that makes sense and meets corporate standards. For detailed information on Azure naming best practices, use this link: https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging.

+   - **The subscription the workspace belongs to**: It is recommended that a separate subscription is created just for Microsoft Sentinel use to limit access to it. If this is not feasible, choose an appropriate subscription to use.

+   - **The location of the workspace**: The workspace should reside in the same location as the resources feeding it to prevent egress charges that would occur if you sent the data from one location to another. Large companies will most likely have resources in many different locations, in which case it would make sense to use the location that has the most resources and has the lowest price for the workspace. Keep in mind that there may be laws in place that denote where the data must reside.
+   
+   For more information on Log Analytics pricing, see https://azure.microsoft.com/en-us/pricing/details/monitor.

+   - **Which resource group will the workspace reside in**: It is recommended that all the Microsoft Sentinel resources reside in the same resource group, although that is not a hard and fast rule. If it makes more sense to have all your workspaces in one resource group, Microsoft Sentinel in another, and the workbooks in a third resource group, then do that. This will not affect the performance of Microsoft Sentinel at all.

+   - **Which pricing tier to use**: If the subscription being used to house the workspace has had a workspace created in it before April 2, 2018, or if the subscription is part of a Microsoft Enterprise Agreement that was in place prior to February 1, 2019, you will continue to have access to the legacy pricing tiers. Otherwise, only **Per GB (2018)** is shown, and data is charged for ingestion and retention per GB. The legacy options are as follows: