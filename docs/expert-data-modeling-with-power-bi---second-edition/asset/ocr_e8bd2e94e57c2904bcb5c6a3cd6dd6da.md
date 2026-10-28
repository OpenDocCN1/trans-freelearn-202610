The last requirement, which is the most confusing one yet, is that the columns contributing to the join must have the same lineage if there is a physical relationship between the two tables. If there is no physical relationship between the tables, the columns participating in the join must not have lineage to any physical columns within the data model. Therefore, we need to break the lineage of the join columns. Otherwise, the `NATURALINNERJOIN()` function does not work. We need to use an expression rather than an actual column name to break the lineage. Therefore, we add 0 days to `'Internet Sales'[OrderDate]` and multiply `'Exchange Rates'[CurrencyKey]` by 1 to break the lineage of those two columns.

Finally, we have the desired result set, so we can multiply `[SalesAmount]` by `[AverageRate]` to get the sales amount in USD.

The following figure shows the **Internet Sales** (without considering the exchange rates) and **Internet Sales USD** measures side by side:

| Product Category | Internet Sales | Internet Sales USD |
|---|---|---|
| Accessories | 700,759.96 | $640,920.11 |
| All-Purpose Bike Stand | 39,591.00 | $35,628.69 |
| Bike Wash - Dissolver | 7,218.60 | $6,352.51 |
| Fender Set - Mountain | 46,619.58 | $41,974.10 |
| Hitch Rack - 4-Bike | 39,360.00 | $35,934.55 |
| HL Mountain Tire | 48,860.00 | $42,839.80 |
| HL Road Tire | 27,970.80 | $24,531.24 |
| Hydration Pack - 70 oz. | 40,307.67 | $35,307.84 |
| LL Mountain Tire | 21,541.38 | $19,092.44 |
| LL Road Tire | 22,435.56 | $21,492.01 |
| ML Mountain Tire | 34,818.39 | $31,580.66 |
| ML Road Tire | 23,140.74 | $20,554.35 |
| Mountain Bottle Cage | 20,229.75 | $18,814.51 |
| Mountain Tire Tube | 15,444.05 | $13,759.74 |
| Patch Kit/8 Patches | 7,307.39 | $6,595.31 |
| Road Bottle Cage | 15,390.88 | $13,953.77 |
| Total | 29,358,677.22 | $26,054,827.45 |