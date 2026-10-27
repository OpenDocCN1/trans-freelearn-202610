
- **Data Retention:** This option allows you to configure the number of days data should be retained in the Log Analytics workspace. The default number for Log Analytics is 31 days; however, when Microsoft Sentinel is also enabled on the Log Analytics workspace, the default free amount is 90 days. If you choose to increase beyond 90 days, you will be charged a set fee per **gigabyte (GB)** per month.

In the next section, we will look at how we calculate the costs involved in data ingestion and retention for Microsoft Sentinel and Log Analytics.

## Calculating the cost of data ingestion and retention

Many organizations need to retain security log data for longer than 90 days, and budget for it to ensure they have enough capacity based on business needs. For example, if we consider the need to keep data for 2 years, with an average daily ingestion rate of 10 GB, then we can calculate the cost of the initial ingestion and analysis, then compare this to the cost of retention. This will provide an annual cost estimate for both aspects.

The following table shows the cost for ingesting data into Log Analytics and analyzing that data in Microsoft Sentinel. This price includes 90 days of free retention:

| | Per day | Per month | Per year total | 2 year total |
|---|---|---|---|---|
| Azure Sentinel | $20 | $600 | $7,200 | $14,600 |
| Log Analytics | $23 | $690 | $8,280 | $16,790 |
| Total | $43 | $1,290 | $15,480 | $30,960 |

Figure 3.12 – Example pricing table for Microsoft Sentinel

The following table shows the amount of data being retained past the free 90 days included in the preceding pricing, based on ingesting 10 GB per day:

| | Day 91 | Day 120 | Day 270 | Day 330 | Day 365 | Day 730 |
|---|---|---|---|---|---|---|
| Total Data Size (GB) | 910 | 1,200 | 2,700 | 3,300 | 3,650 | 7,300 |
| Retention Costs (per month) | $91 | $120 | $270 | $330 | $365 | $730 |

Figure 3.13 – Example pricing table for retention costs

Now, if we add these together, we can see the total cost of the solution over a 12-month period, shown in the following table:

|  | Data Size (GB) | Log Analytics | Azure Sentinel | Data Retention | Total Cost (per month) |
|---|---|---|---|---|---|
| Month 1 | 300 | $690 | $600 | $0 | $1,290 |
| Month 2 | 600 | $690 | $600 | $0 | $1,290 |
| Month 3 | 900 | $690 | $600 | $0 | $1,290 |
| Month 4 | 1,200 | $690 | $600 | $120 | $1,410 |
| Month 5 | 1,500 | $690 | $600 | $150 | $1,440 |
| Month 6 | 1,800 | $690 | $600 | $180 | $1,470 |
| Month 7 | 2,100 | $690 | $600 | $210 | $1,500 |
| Month 8 | 2,400 | $690 | $600 | $240 | $1,530 |
| Month 9 | 2,700 | $690 | $600 | $270 | $1,560 |
| Month 10 | 3,000 | $690 | $600 | $300 | $1,590 |
| Month 11 | 3,300 | $690 | $600 | $330 | $1,620 |
| Month 12 | 3,600 | $690 | $600 | $360 | $1,650 |
| Annual Totals | 3.6 TB | $8,280 | $7,200 | $2,160 | $17,645 |
| 2Year Total | 7.2 TB | $16,560 | $14,400 | $8,820 | $39,780 |

Figure 3.14 – Example pricing table to compare sizing options

Based on these examples, the total cost of running Microsoft Sentinel, ingesting 10 GB per day, and retaining data for 2 years would be $39,780. Data retention accounts for 22% of the cost.

> Note
> These prices are based on the current rates applicable to the US East Azure region, and figures are rounded for simplicity. Actual data usage may fluctuate each month.

You can carry out your own calculations using the Azure pricing calculator, which will account for the commit tiers, retention, and provide a combined cost for both Log Analytics and Microsoft Sentinel. Go to this URL and modify the settings to see the different pricing: https://azure.microsoft.com/en-us/pricing/calculator/.

The following solutions may be considered as an alternative for long-term storage, outside of Log Analytics:

- **Azure Blob Storage**: You can create a query in Azure Monitor to select the data you want to move from the Log Analytics workspace and point it to the appropriate Azure storage account. This allows for filtering of the information by type (or select everything), and only moving data that is about to come to the end of its life, which is the limit you have set for data retention in the Log Analytics workspace. Once you have the query defined, you can use **Azure Automation** to run PowerShell and load the results as a CSV file, then copy to **Azure Blob Storage**. With this solution, data can be stored for up to 400 years! For further information, see this article: https://docs.microsoft.com/en-us/azure/azure-monitor//powershell-samples.
- **Azure Data Explorer (ADX)**: The most common method of using extended storage and archive of additional security data is Azure Data Explorer, which enables up to 100 years of extended storage. With this solution, you can gain great optimization of both the storage and compute required to ingest, catalog, analyze, and present the information, all using the same **Kusto Query Language (KQL)** queries you are using in Microsoft Sentinel. When configuring this service, you can choose between storage-optimized and compute-optimized implementation depending on the need of the security team that will carry out the investigations. During quiet times, you can optimize for storage retention, then when you need to carry out in-depth investigations you can increase the compute capacity on demand to ensure responsive search queries. There is an average cost saving of one-tenth of the price to store this data in Microsoft Sentinel. You can use the online calculator for your own value calculations: https://dataexplorer.azure.com/AzureDataExplorerCostEstimator.html.

As you can see, there are several options available to store the data in alternative locations, both for extended archive/retention and for additional analysis with alternative tools. We expect Microsoft will increase the number of options available.

This chapter will cover the following topics:

- Introduction to TI
- Understanding STIX and TAXII
- Choosing the right intel feeds for your needs
- Implementing TI connectors

## Introduction to TI

Due to the complex nature of cybersecurity and the sophistication of modern attacks, it is difficult for any organization to keep track of vulnerabilities and the multiple ways that an attacker may compromise a system, especially if cybersecurity is not the focus of the organization. Understanding what to look for and deciding what to do when you see a system anomaly, or another potential threat, is complex and time-consuming. This is where TI comes in handy.

TI is critical in the fight against adversaries and is now integrated with most security products; it provides the ability to set a list of indicators for detection and blocking malicious activities. You can subscribe to TI feeds to gain knowledge from other security professionals in the industry and create your own indicators that are specific to the environment you are operating in.

If you are new to this topic, there are some new keywords and abbreviations to learn:

- **Threat indicators**: This is a list of suspicious or known malicious entities, such as IP addresses, URLs, and files. They are used to alert when suspicious usage of internal files or addresses is discovered in your environment.
- **Indicators of Compromise (IoCs)**: This indicates uniquely known behaviors and activities that show signs of potential malicious intent or an actual breach. These are available as open source and some as paid-for services.
- **Alert definitions**: By combining multiple threat indicators and IoCs, you can build an alert definition that only triggers in the right context. This will reduce alert fatigue from overloading the SOC with too many false positives.
- **Malware Information Sharing Project (MISP)**: This is an open source **Threat Intelligence Platform (TIP)** and a set of open standards for threat information sharing. MISP covers malware indicators, fraud, and vulnerability information. Read more here: https://www.misp-project.org/.
