| Product Category | Internet Sales | Internet Sales in USD |
|---|---:|---:|
| **Accessories** | **700,759.96** | **$640,920.1111** |
| Bike Racks | 39,360.00 | $35,934.55 |
| Bike Stands | 39,591.00 | $35,628.6896 |
| Bottles and Cages | 56,798.19 | $52,340.3834 |
| Cleaners | 7,218.60 | $6,352.5092 |
| Fenders | 46,619.58 | $41,974.101 |
| Helmets | 225,335.60 | $209,433.4812 |
| Hydration Packs | 40,307.67 | $35,307.8438 |
| Tires and Tubes | 245,529.32 | $223,948.5534 |
| **Bikes** | **28,318,144.65** | **$25,107,749.7623** |
| Mountain Bikes | 9,952,759.56 | $8,954,853.1688 |
| Road Bikes | 14,520,584.04 | $12,599,394.0018 |
| Touring Bikes | 3,844,801.05 | $3,553,502.5943 |
| **Clothing** | **339,772.61** | **$306,157.5785** |
| Caps | 19,688.10 | $18,674.1168 |
| Gloves | 35,020.70 | $31,022.1741 |
| Jerseys | 172,950.68 | $157,333.8088 |
| Shorts | 71,319.81 | $62,692.697 |
| Socks | 5,106.32 | $4,665.6082 |
| Vests | 35,687.00 | $31,769.1769 |
| **Total** | **29,358,677.22** | **$26,054,827.4537** |

Figure 8.36: A new version of the Internet Sales in USD measure

The preceding scenario shows how data modeling can simplify our DAX expressions. Just as a reminder, here is the same measure that we created in *Chapter 2, Data Analysis eXpressions and Data Modeling*, without creating the relationship between the **Exchange Rates** table and the **Internet Sales** table:

```
Internet Sales USD =
SUMX(
    NATURALINNERJOIN (
        SELECTCOLUMNS(
```