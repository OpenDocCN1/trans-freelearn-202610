| EmployeeKey | Full Name | Manager | Sales | Start Date | End Date | Status |
|---:|---|---|---:|---|---|---|
| 290 | Amy Alberts | Stephen Jiang | $732,078.4446 | 15/11/2011 |  | Current |
| 289 | David Campbell | Roger Hamilton | $3,729,945.3501 | 29/12/2010 |  | Current |
| 284 | Garrett Vargas | Roger Hamilton | $3,609,447.2163 | 29/12/2010 |  | Current |
| 291 | Jae Pak | Amy Alberts | $8,503,338.6472 | 29/12/2011 |  | Current |
| 283 | Jillian Carson | Roger Hamilton | $10,065,803.5429 | 29/12/2010 |  | Current |
| 288 | José Saraiva | Roger Hamilton | $5,926,418.3574 | 29/12/2010 |  | Current |
| 282 | Linda Mitchell | Roger Hamilton | $10,367,007.4286 | 29/12/2010 |  | Current |
| 296 | Lynn Tsoflias | Syed Abbas | $1,421,810.9252 | 28/12/2012 |  | Current |
| 281 | Michael Blythe | Roger Hamilton | $9,293,903.0055 | 29/12/2010 |  | Current |
| 286 | Pamela Ansman-Wolfe | Roger Hamilton | $3,325,102.5952 | 29/12/2010 |  | Current |
| 295 | Rachel Valdez | Amy Alberts | $1,790,640.2311 | 28/12/2012 |  | Current |
| 292 | Ranjit Varkey Chudukatil | Amy Alberts | $4,509,888.933 | 29/12/2011 |  | Current |
| 287 | Shu Ito | Roger Hamilton | $6,427,005.5556 | 29/12/2010 |  | Current |
| 272 | Stephen Jiang | Brian Welcker | $1,092,123.8562 | 4/08/2010 | 30/04/2012 |  |
| 294 | Syed Abbas | Stephen Jiang | $172,524.4515 | 12/10/2012 |  | Current |
| 293 | Tete Mensa-Annan | Roger Hamilton | $2,312,545.6905 | 30/04/2012 |  | Current |
| 285 | Tsvi Reiter | Roger Hamilton | $7,171,012.7514 | 29/12/2010 |  | Current |
| **Total** |  |  | **$80,450,596.9823** |  |  |  |

Figure 12.23: Reseller sales by employees and their status without considering SCD

What if the business needs to only show sales values for employees when their status is **Current**? In that case, we would have to factor the SCD into the equation and filter out **Stephen’s** sales values. Depending on the business requirements, we might need to add the **Status** column as a filter in the visualizations, while in other cases, we might need to modify the measures by adding the **Start Date**, **End Date**, and **Status** columns to filter the results. The following image shows the results when we use visual filters to take out **Stephen’s** sales:

![](bbox=[0.177, 0.657, 0.822, 0.904])