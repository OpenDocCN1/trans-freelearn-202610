

| Process Category (Hierarchy ID) | Cross-Industry (PCF 7.4) | Customer Products (PCF 7.2.2) | Retail (PCF 7.2.1) |
|---|---|---|---|
| 1.0 | Develop Vision and Strategy | Develop Vision and Strategy | Develop Vision and Strategy |
| 2.0 | Develop and Manage Products and Services | Design and Develop Products and Services | Develop and Manage Customer Experience |
| 3.0 | Market and Sell Products and Services | Market and Sell Products and Services | Market Products and Services |
| 4.0 | Manage Supply Chain for Physical Products | Deliver Products | Merchandise Products and Services |
| 5.0 | Deliver Services | Deliver Services | Deliver Products |
| 6.0 | Manage Customer Service | Manage Customer Service | Deliver Services |
| 7.0 | Develop and Manage Human Capital | Develop and Manage Human Capital | Develop and Manage Human Capital |
| 8.0 | Manage Information Technology (IT) | Manage Information Technology (IT) | Manage Information Technology (IT) |
| 9.0 | Manage Financial Resources | Manage Financial Resources | Manage Financial Resources |
| 10.0 | Acquire, Construct, and Manage Assets | Acquire, Construct, and Manage Assets | Acquire, Construct, and Manage Assets |
| 11.0 | Manage Enterprise Risk, Compliance, Remediation, and Resiliency | Manage Enterprise Risk, Compliance, Remediation, and Resiliency | Manage Enterprise Risk, Compliance, Remediation, and Resiliency |
| 12.0 | Manage External Relationships | Manage External Relationships | Manage External Relationships |
| 13.0 | Develop and Manage Business Capabilities | Develop and Manage Business Capabilities | Develop and Manage Business Capabilities |

Table 6.1 – A comparison of the process categories in the PCF for three selected variants

Based on this comparison, BPMT Dishes, which manufactures and sells its products to customers, could select the Customer Products version. The company could then create its process architecture based on the process categories taken from the PCF. An example of this is shown in Figure 6.6.

## Operating processes

1.0 Develop Vision and Strategy

2.0 Design and Develop Products and Services

3.0 Market and Sell Products and Services

4.0 Deliver Products

5.0 Deliver Services

6.0 Manage Customer Service

## Management and support processes

7.0 Develop and Manage Human Capital

8.0 Manage Information Technology (IT)

9.0 Manage Financial Resources

10.0 Acquire, Construct, and Manage Assets

11.0 Manage Enterprise Risk, Compliance, Remediation, and Resiliency

12.0 Manage External Relationships

13.0 Develop and Manage Business Capabilities

Figure 6.6 – A top-level model for process architecture based on APQC’s PCF

The next step would be to describe what happens in each process category. In our example, we’ll focus on the area related to customer service, because that’s where the area related to handling complaints is. It would be a process group named **Service products after sales**, with hierarchy ID 6.3:

6.1 Develop customer care/ customer service strategy

6.2 Plan and manage customer service operations

6.3 Service products after sales

6.4 Manage product recalls and regulatory audits

6.5 Evaluate customer service operations and cu...

Figure 6.7 – Example decomposition of the process category

![](img/a4c90ee3d142c33743706d37c65779c2_187_0.png)

As shown in Figure 6.7, our process group not only has a name and hierarchy ID, but also a description that helps us better understand its scope. In addition, for our 6.3 process group, there are also lower-level elements that provide additional information: five processes (6.3.1 Register products, 6.3.2 Process warranty claims, 6.3.3 Manage supplier recovery, 6.3.4 Manage customer self-service materials, and 6.3.5 Service products) and more than 30 low-level elements (activities and tasks), such as 6.3.2.3 Investigate warranty issues and 6.3.2.3.1 Define issue.

As you can see, APQC’s PCF contains a real wealth of elements describing processes. For example, the taxonomy for the Customer Products version contains almost 2,000 elements at the five levels, of which more than 1,800 contain descriptions. The PCF is an invaluable aid for identification, documentation, standardization, and process improvement purposes, as well as for many other applications, such as benchmarking and organizing content.

Given all this, it should come as no surprise that the PCF is the most popular process library in the world. However, it is not the only option available. Telecommunications companies frequently use Enhanced Telecommunications Operations Map (eTOM), companies for which logistics is an important aspect of operations use Supply Chain Operations Reference (SCOR), and IT departments use Information Technology Infrastructure Library (ITIL). In addition, reference process libraries can be provided alongside IT systems (e.g., ERP from SAP) or by consulting companies.

Many of those sources of process knowledge can also be used by AI, so it is technically possible to use GenAI solutions to prepare first drafts of the process architecture based on those process libraries. Or, perhaps even better, AI-enabled tools can be used to make sure that no important group of stakeholders or other business concept that should be covered in a process architecture is forgotten.

However, so far, there is no way to automatically create a good process architecture that people will be using without involving employees. Therefore, if you are interested in automating your processes and want to make sure you do not miss something important, it makes sense to create some kind of process architecture before you start the technical part of automation.

For many organizations, a process library such as the PCF is also very useful for making sure that the company will not spend months trying to reinvent the wheel and making common mistakes. Sometimes, it is also easier to make everyone agree on one global standard approach to processes than to reach a consensus internally, especially if the organization has a strong silo culture.

For some organizations, adapting the process library to their needs is not sufficient for ensuring that employees feel that the process architecture is tailored to their needs. The next section will show how to handle such situations so that we can have better employee acceptance and keep the benefits of using global standards.

## How to build your own process architecture

In this section, we will learn how to build a custom process architecture. We will begin by discussing when it makes sense to do so, and then we will see what building a process architecture could look like for our fictional company.

### Why does it make sense to create a custom process architecture?

As mentioned previously, for some organizations, a process architecture tailored to their needs is a very compelling option. As in the previous cases, this also involves a multi-level structure of connected models. Typically, they are created during workshops with organization employees. As you may expect, it takes more time than adopting the PCF, but if done correctly, it can help build a stronger process culture in the organization.

For those who want to benefit from the best practices available and have the custom look and feel of the process architecture, a hybrid approach is also possible. A process architecture can be created based on input from workshops, but with additional verification of completeness based on reference processes such as APQC’s PCF. This helps to speed up the whole process and improve its quality.

The construction of the process architecture often benefits from an analysis of the organization’s strategy. This is because processes should ensure the operationalization of this strategy, as well as consider the stakeholders and the products and services through which the organization creates value for them.

Interestingly, often, it turns out that, within an organization, there are frequently many conflicting ideas about through which processes value is created and for whom, who the organization’s stakeholders are, and how the organization intends to meet those expectations.

A common vocabulary is also a very frequent problem for many organizations. This can lead to situations where different units do not have a common definition of who the customer is! You can easily imagine how harmful this can be for important initiatives.

Discussions about different aspects of building the process architecture help develop a common understanding of how the organization works and how its processes fit into the overall value creation ecosystem. This is simply invaluable. The outcome of the architecture work itself—for example, a set of dozens or hundreds of identified and described processes documented in the form of interconnected models—is very useful, but the most important thing is to ensure that these models are the result of agreements between all stakeholders.

It is possible to prepare a process architecture very quickly via a small group of specialists from the BPM CoE, but the real value is an architecture that results from a common understanding and agreement between people involved in process management in the organization. This acts as a contract that summarizes the way of working and provides a common view of the organization that has been agreed upon.

