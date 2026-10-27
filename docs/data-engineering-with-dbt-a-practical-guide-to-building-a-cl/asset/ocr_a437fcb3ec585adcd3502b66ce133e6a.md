The QUANTITY_PURCHASED metric is the difference between the current quantity and the previous one unless the previous was `null` as we are opening the position. In such cases, the variation is the quantity itself.

This value should be correct, or at least correctly represent all the transactions that happened between the two different positions that we have observed.

The TX_TYPE metric provides an easy-to-understand semantic regarding the direction of the transaction.

The *ESTIMATED_TX_PROCEEDS* metric, as the name implies, is an estimation of the monetary flow associated with the transaction (negative for buy, positive for sell). As explained previously, the cost of buying should be accurate, while the proceeds for the sale are only an estimation (based on the last change of the position before selling).

We will close this CTE with the metrics that describe the position after the transaction:

```
```sql
    , QUANTITY             as FINAL_QUANTITY
    , COST_BASE            as FINAL_COST_BASE
    , POSITION_VALUE       as FINAL_POSITION_VALUE
FROM position_with_lead as p
)
```
```

Finally, we will close the model by returning the columns from the previous CTE with the data ordered by the transaction key:

```
```sql
SELECT * FROM tx_calculation
ORDER BY ACCOUNT_CODE, SECURITY_CODE, REPORT_DATE
```
```

If we would like to use this new fact in our data mart, along with the security info dimension, we might add a CTE to join it with the dimension to remove orphans, but we prefer not to remove the missing key and use the self-completing dimension pattern. In this case, completing the dimension for the position fact will also complete the transactions as the list of securities involved is the same.

# Publishing dependable datasets

The role of the delivery layer is to make our data, built in the REF layer, accessible to our users, who will use them in a variety of ways.

One thing that we can be sure about is that the more often we change the layout of our data in the delivery layer, the less happy our customers will be as they must react to all our changes.

Unexpected changes are another good way to anger your customers.

What should we learn from this?