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