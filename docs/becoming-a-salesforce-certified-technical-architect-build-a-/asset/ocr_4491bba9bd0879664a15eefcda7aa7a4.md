The typical enterprise integration needs are as follows:

+   - **Get the right information**: Get precise knowledge of a particular piece of information created by different systems and enterprise business processes. This knowledge must be structured in a way that can support other business needs.
+   - **Get that information to the right place**: This requires mechanisms to handle information transactions across heterogeneous environments, which may be based on different technology stacks and reside on different servers/hardware and operating systems.
+   - **Get that information at the right time**: Ideally, this requires distributing the information in real time to reflect the actual state of a particular data entity.
+   - **Flexibility and embracement of change**: This is done to adapt to external factors, such as market demand, customer shift of behavior, new legislation, or a shift in social philosophy.
+   - **Coordinate business processes**: This is a challenging operation that may require modeling the enterprise business's processes, how they are interlinked, and what kind of information they exchange. This may require having a deep understanding of the business and certain knowledge of the enterprise.

So, what makes a good integration architecture? You will find that out in the next section.

# Integration Architecture Design Principles

Like any complex technical architectural topic, there are several considerations and consequences that you need to keep in mind while designing the target integration strategy and architecture. Integration is one of the key areas where _sub-optimal_ designs could have a significant impact on the overall solution.

The main decision points are usually as follows:

+   - **Native integration**: Simply put, if you can develop a single standalone application that can fulfill all business needs on its own, without any need to collaborate with any other application, then you can avoid a lot of complexity driven by integration requirements. However, this is not something you can normally achieve. Several attempts to extend an application so that it includes other functionalities can end up creating a complex system that is hard to manage and maintain, slow to react to market needs, and hard to adjust to meet new business requirements.

This problem exists in the Salesforce world as well, although in a more limited way. Many Salesforce products are natively integrated, with many considered the best in the market, and have the ability to deliver different user experiences. For example, Salesforce communities provide a native solution to exposing a customer portal that is natively integrated with your CRM.