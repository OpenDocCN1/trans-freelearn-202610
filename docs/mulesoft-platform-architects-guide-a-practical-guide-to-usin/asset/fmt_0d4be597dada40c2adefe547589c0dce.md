
The preceding architecture shows manual clustering on a runtime plane without registering the server to the control plane on Anypoint Platform.

## Mule runtime plane on-premises and control plane on Anypoint Platform (hybrid)

In this approach, the Mule runtime will be set up on-premises and all the servers will be registered on Anypoint Platform. The Anypoint Platform control plane is outside the firewall of the organization, and there is a need to allow communication between the on-premises servers and the control plane on Anypoint Platform. The on-premises servers will send metadata to the control plane, and this will generate metrics on the control plane such as CPU utilization and memory utilization. With the control plane, you can manage the on-premises servers, such as by starting and stopping the Mule runtime and application and viewing the status of the servers.

The following figure shows how a Mule runtime on-premises will communicate with the control plane on Anypoint Platform.

![Figure 12.12: Mule runtime plane on-premises and control plane on Anypoint Platform (hybrid)](image/B19922_12_12.jpg)

Figure 12.12: Mule runtime plane on-premises and control plane on Anypoint Platform (hybrid)

We have discussed Anypoint clustering, and it is possible to cluster all servers to act as a single unit. It can be easily done from Runtime Manager in Anypoint Platform. `Figure 12.12` shows a Mule Runtime setup with Anypoint clustering with a control plane. With this approach, metadata will leave the organization's data center to generate metrics such as CPU and memory utilization and manage the clusters, servers, and server groups.

## Mule runtime plane on-premises and control plane on Anypoint Platform PCE (fully on-premises)

With this approach, you have the runtime plane and control plane both located in your own data center. We discussed Anypoint Platform PCE earlier, which is available locally, and an application deployed to an on-premises Mule runtime can connect to Anypoint Platform PCE without any public internet connectivity. The following are a few capabilities provided by Anypoint Platform PCE:

*   Anypoint Platform PCE supports using Runtime Manager to manage applications deployed to an on-premises Mule runtime and set up Anypoint clustering, server groups, and so on
*   Anypoint Platform PCE supports using API Manager to manage the life cycle of APIs deployed to an on-premises Mule runtime, such as applying API policies and managing API instances
*   Anypoint Platform PCE supports Anypoint Monitoring and Anypoint Visualizer

![Figure 12.13: Mule runtime plane on-premises and control plane on Anypoint Platform (hybrid)](image/B19922_12_13.jpg)

Figure 12.13: Mule runtime plane on-premises and control plane on Anypoint Platform (hybrid)

In `Figure 12.13`, you can see both the control plane and runtime plane located in the same data center and there is no need for public internet connectivity to establish the communication between the control and runtime planes. With this approach, you need to manage the whole infrastructure networks for the control and runtime planes as both are in your own data center. This solution is mostly used when your organization has strict compliance and security requirements that mean they cannot use a cloud-based solution.

This approach can be useful when your organization has a strict compliance policy where your data or metadata must not leave your organization's data center that is hosted on-premises and you have a strong team for managing underlying infrastructure and networks.

# Running multiple MuleSoft applications on a single port (Mule domain)

A Mule domain allows you to share resources among applications deployed on an on-premises Mule runtime. You may have to share the connector configuration, shared properties, and error handlers among the multiple applications, and domain projects are the only way to share resources among applications.

When deploying Mule on-premises, you can specify global configurations such as default error handling, shared properties, scheduler pools, and connector configuration to be shared across all applications deployed on the same domains. To use Mule, you need to create a domain and then link to it from every application. This means that each app now assigned to your Mule domain will have access to shared resources in that domain.

Domain usage significantly improves performance when you're deploying multiple services to the same Mule on-premises runtime engine. Since domains provide a central point for all shared resources, they make the class-loading process faster, especially since the domain dependencies defined in `POM.xml` are also shared within the domain apps.

The following are the shared configuration can be part of the domain project:

![Figure 12.14: Domain project](image/B19922_12_14.jpg)

Figure 12.14: Domain project

Let's consider deploying applications on an on-premises Mule runtime that has an HTTP listener. Each application must be deployed on a different port, and they must not overlap with other applications' ports. This adds the complexity of managing multiple ports. To solve this problem, we can use a domain project by adding an HTTP listener connection configuration on any port (for example, `8081`). An application can reference the domain project and refer to the HTTP listener configuration from the domain project. This way, all the applications can refer to the domain project and the HTTP listener configuration – you don't have to configure a separate port for each project, and you can easily refer to the connector configuration from the domain project.

![Figure 12.15: Running MuleSoft applications on a single port using a domain project](image/B19922_12_15.jpg)

Figure 12.15: Running MuleSoft applications on a single port using a domain project

In the preceding diagram, each application deployed to the Mule runtime refers to the domain project to use the HTTP listener connector configuration, so the applications can run on a single port, `8081`.

## How to use a domain project in API-led connectivity architecture

As we are aware, there are three layers in API-led connectivity: the experience API, the process API, and the system API. We usually have a requirement not to expose the system and process APIs externally and only the experience API can be called by external consumers.

To achieve this, we will use three domain projects: `exp-domain-project-8081`, `prc-domain-project-8082`, and `sys-domain-project-8083`. Each domain project will have an HTTP listener connector configuration with a different port, as shown in the following table.

| **Layer** | **Domain Project** | **HTTP Listener Configuration – Port** | **Load Balancer** |
| --- | --- | --- | --- |
| Experience API | `exp-domain-project-8081` | `8081` | External load balancer |
| Process API | `prc-domain-project-8082` | `8082` | Internal load balancer (optional) |
| System API | `sys-domain-project-8083` | `8083` | Internal load balancer (optional) |

Table 12.4: Domain project configuration

![Figure 12.16: Domain project with API-led connectivity](image/B19922_12_16.jpg)

Figure 12.16: Domain project with API-led connectivity

In `Figure 12.16`, the experience API running on port `8081` is exposed to the external load balancer, whereas the system and process APIs can be exposed to the internal load balancer, so the experience API can communicate with the process API using an internal URL and the process API can communicate with the system API using an internal URL. If an internal load balancer has been set up in front of the process API and the system API, then an internal load balancer can be used for internal communication.

# Deploying a MuleSoft application on-premises using the Mule Maven plugin
