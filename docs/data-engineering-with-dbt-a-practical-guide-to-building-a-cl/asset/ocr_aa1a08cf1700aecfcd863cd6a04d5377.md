Note that now, three rows have been updated by the run at **14:14** with the new contents of the portfolio. The third row that was inserted is the one with `quantity = 0` to close the position in the 'GCM' stock.

# Picking the right loading pattern to load history

Now that you have two HIST tables with different data for the same input, you might wonder which one is correct.

Both tables are correct, as they both correctly record what they set out to record.

The question you should have is not about correctness, but about how to choose the right pattern. It is us, the analytics engineers, that must choose the right loading pattern based on the data to be loaded and how the data is exported from the source system.

In general, you can load with the basic history loading pattern all the dimensions, whose entries we must keep in use even after they are deleted, and the facts that are not deleted, but eventually disabled or deactivated. We can also use this pattern when we have Change Tracking or Change Data Capture enabled to get the data from the source, along with the column telling us when the entries of the table are deleted.

In all cases where we have facts where rows are deleted and we do not get the information of the deletion as part of the source data, we must use the load pattern which recognizes and stores the deletions, even if it is not as performant as the other.

# Getting the closed position in the flow

Now that we have created the new HIST table that detects the closed positions, let’s get this better data into use in place of the old HIST table.

Open the `REF_POSITION_ABC_BANK` model and replace the reference to the old HIST table in the `current_from_history` macro:

```
history_rel = ref('HIST_ABC_BANK_POSITION'),
```

Apply the following reference to the new HIST table:

```
history_rel = ref('HIST_ABC_BANK_POSITION_WITH_CLOSING'),
```

Now, if you try to run the REF model, you will receive an error. This is because we have a division by zero in correspondence with the closed position as the cost basis is now zero.

To remove the error and output a value of zero when we have a cost basis value of zero, we must replace the normal division with the `SF DIVO` function:

```
, ROUND(DIVO(UNREALIZED_PROFIT, COST_BASE), 5)*100
  as UNREALIZED_PROFIT_PCT
```