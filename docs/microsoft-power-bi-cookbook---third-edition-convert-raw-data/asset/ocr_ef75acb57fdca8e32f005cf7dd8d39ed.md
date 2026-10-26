| BUSINESS PROCESSES | Executive | Sales | Finance | Marketing | Merchandising | eCommerce | Customer Service | Supply Chain |
|---|---|---|---|---|---|---|---|---|
| Internet Sales | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |  |  |
| Internet Sales Plan | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Reseller Sales | ✓ | ✓ | ✓ |  | ✓ |  |  |  |
| General Ledger | ✓ |  | ✓ |  |  |  |  |  |
| Inventory |  |  | ✓ | ✓ | ✓ | ✓ |  |  |
| Customer Surveys | ✓ | ✓ | ✓ | ✓ |  | ✓ | ✓ | ✓ |
| Customer Service Calls |  |  | ✓ |  |  |  | ✓ |  |
| Shipping |  |  | ✓ |  | ✓ |  |  | ✓ |

Figure 4.1: Stakeholder matrix

In this example, the semantic model contains the four highlighted fact tables: Internet Sales, Internet Sales Plan, Reseller Sales, and General Ledger. When there are multiple stakeholders within a given business function with their own unique needs and use cases, these stakeholders can be added as columns to the stakeholder matrix. In Power BI, there are many options for meeting the unique needs of different stakeholders with the same semantic model and underlying retrieval and data architecture.

# How to build rich and intuitive reports

While report development should be an agile process where feedback is regularly solicited from stakeholders as the report is designed and developed, having a solid planning and design process for a report upfront is extremely beneficial in understanding its scope and requirements.

A single report should not attempt to address unrelated business questions or meet the needs of highly diverse users such as a corporate financial analyst or a store manager. Multi-scope reports can lead to convoluted user experiences and report-level customization that can be difficult to maintain and scale. The report planning and design process should answer the following five questions:

+   1. Who will be accessing this report?
+   2. What are the top priorities of this report in terms of business questions?
+   3. How will the report be accessed and utilized?
+   4. What is the velocity of data required, and does this impact the model?
+   5. Is Power BI the right tool for the report?

Regarding the first of these questions, just like a PowerPoint presentation, report pages should support a single theme and target a specific audience. As per the stakeholder matrix, there are often many disparate users and use cases for a given semantic model. Consider the following:

+   - If identified users have highly disparate needs, choose one user role and address the others separately.
+   - Page-level filtering and row-level security can provide a robust solution for a single team.