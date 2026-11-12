
# Cloud Adoption Framework Optimization Phases

### FinOps
Cost management is key to any cloud transformation, as subscription-based services can throw up surprises if not well managed. Regularly evaluating the application needs and consistently right-sizing environments, setting up budgets and alerts, leveraging cloud-native cost management tools to monitor and analyze costs, reviewing various cost-efficient levers, such as cloud commitments and sustained use discounts, and migrating to cloud-native frameworks (e.g., serverless) where possible constitute a strategic FinOps practice.

### Site reliability engineering (SRE)
This step consists of monitoring **key performance indicators** (**KPIs**) to measure cloud service effectiveness, log aggregation and analytics to trace and understand user behavior, **application performance monitoring** (**APM**), security patching, and ensuring high levels of availability and reliability. These factors are extremely critical to the success of any cloud transformation initiative.

### Automating everything
This is also considered part of SRE; the practice of automating everything is considered the highest maturity level of cloud transformation, as this can lead to repeatable and reliable systems owing to fewer human touchpoints and errors. Automating common tasks and workflows, such as infrastructure automation through **infrastructure as code** (**IaC**), the automation of configuration, application deployments, backups, automatic scaling, and tear-downs, are important constituents of this stage.

### Business continuity
In this step, organizations implement robust **business continuity plans** (**BCPs**) and data backups and test them regularly.

### Identifying reuse
Another key element in the optimize phase is the ability to identify patterns and architecture that can be reused across the enterprise. A consistent feedback loop must exist between cloud operations, CCoE, and EARB to identify architecture blueprints and apply best practices across the organization, preventing the need to reinvent the wheel.

Having understood the important phases and constituents of setting up a cloud adoption framework, let us explore how organizations can build the foundation for a hybrid cloud deployment.

# Various hybrid cloud and multi-cloud deployment patterns

For most enterprises, the hybrid cloud deployment model offers the unique flexibility of a public cloud and the control of a private infrastructure. However, unifying a public cloud architecture with a traditional on-premises setup poses challenges primarily because of their inherently different operational, security, and governance frameworks. Since infrastructure, existing DevOps practices, skillsets, workloads, and processes are unique to every enterprise context, a hybrid cloud or multi-cloud deployment strategy must be adapted to specific needs. Hybrid cloud and multi-cloud terminologies are typically overloaded and used inconsistently. **Hybrid cloud** refers to a deployment strategy where an application or workload is deployed across on-premises and public clouds, interconnected through a common network, while **multi-cloud** refers to a deployment strategy where applications or workloads are deployed across multiple public clouds, with or without integrations to on-premises. In this section, we will review some common hybrid cloud deployment patterns.

## Tiered hybrid cloud pattern

The tiered hybrid cloud deployment pattern involves the strategic allocation of workloads and applications across cloud and on-premises private computing environments based on their criticality, performance requirements, and security concerns. This pattern broadly classifies an application or workload into two tiers – **frontend** and **backend**. A frontend that is user-facing runs on modern technology stacks and fluctuates based on demand, whereas a backend is a traditional application or system that serves core business functions that are business critical and yet are harder to re-platform. In this pattern, a frontend takes advantage of flexible and modern public cloud infrastructure, whereas the backend is left as-is in an on-premises environment. This pattern allows organizations to exploit the advantages of each environment for specific sets of tasks, offering a balanced approach to cost, performance, and security.

Some key benefits of this pattern include the following:

*   **Experimentation and innovation**: This pattern allows enterprises with significant investments on-premises to test, experiment, and innovate faster in the cloud while also retaining critical assets within their on-premises environment.
*   **Scalability and resource optimization**: The tiered pattern allows enterprises to scale resources up or down based on demand in a public cloud environment while leveraging existing resources and optimizing on-premises backends. Applications can also be dynamically moved between different tiers, optimizing resource utilization and ensuring optimal performance.
*   **Security and compliance**: Critical and sensitive applications can be safeguarded within the private cloud or on-premises, ensuring compliance with industry regulations, data residencies, and data protection standards. Less-sensitive workloads can be placed in the public cloud while maintaining appropriate security measures.
*   **Cost optimization**: By strategically distributing workloads, organizations can optimize costs across a public cloud and on-premises. Critical backends that are too risky or time-consuming to rehost, replatform, or re-architect can continue to run on-premises or in private clouds to avoid potential data transfer costs, whereas frontends and application components can leverage the cost-effective scalability of public clouds.
*   **Performance optimization**: Tiering workloads based on performance requirements ensures that applications receive the necessary resources to meet performance expectations. This prevents resource contention and latency issues that could arise in a one-size-fits-all cloud approach.

While there are several advantages to this pattern, organizations must be aware of the following challenges and considerations.

*   **Complexity**: Managing multiple tiers across various public cloud environments can be complex, requiring specialized skills and tools. Splitting tiers across public clouds and on-premises cuts across heterogeneous deployment paradigms and, hence, requires a thorough understanding of the workload characteristics, architecture, data flow, and security requirements. Another key consideration is the choice of compatible technologies that can seamlessly operate across public clouds and on-premise environments.
*   **Network latency**: As the tiers are split across public cloud and on-premises, it can introduce network latency, particularly for applications requiring real-time data processing. The meticulous planning of network architecture, interface design, and API gateways are crucial to mitigate this challenge.
*   **Resource allocation**: Allocating resources to workloads running across public clouds and on-premises appropriately among tiers can be challenging, for instance, during peak demand or unexpected workloads. Careful design considerations, such as implementing mechanisms for monitoring resource utilization and dynamic resource allocation, are essential to mitigate this challenge.

## Partitioned multi-cloud pattern

A partitioned multi-cloud pattern leverages a multi-cloud deployment strategy where applications are deployed across multiple cloud environments. This strategy offers enterprises the greatest level of flexibility to deploy workloads that are suited or can operate efficiently in the optimal cloud environment. There are two variants of this pattern:

*   A workload is partitioned and distributed across multiple clouds, allowing application traffic to traverse seamlessly.
