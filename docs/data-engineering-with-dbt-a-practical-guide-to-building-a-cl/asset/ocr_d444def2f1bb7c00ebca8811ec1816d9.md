Let’s discuss it:

+   - By using * (the star symbol) in the SELECT command, we take all the fields from the source we are reading from, so we stay open for future changes
+   - We calculate UNREALIZED_PROFIT as the difference between the current value and the cost base of the position

Just a note on the file names: we named the CSV file ABC_BANK_POSITION and the dbt model POSITION_ABC_BANK. Are they good names? Do we risk confusion?

It is common, and logical, to think about the incoming data with the source first and then what the content is, so the name of our CSV reflects this “source first” thinking that is proper for the source and storage layers. When we move to the dbt models in the refined layers, where we convey information, we start thinking in terms of the content (or concept) first and maybe we need or want to recall the source if the data is not of general use or scope. The name of our model conveys that we talk about the POSITION concept and that the content is limited to the ABC Bank system.

So far so good, but if you run the query, it looks like the position for “TRADE DESC INC/THE” with the TTD symbol has a large absolute profit, and the others just a small one. Let’s add a UNREALIZED_PROFIT_PCT field to account for the size of the initial investment.

Let’s change the POSITION_ABC_BANK.sql file as follows:

```
SELECT
    *
    , POSITION_VALUE - COST_BASE as UNREALIZED_PROFIT
    , UNREALIZED_PROFIT / COST_BASE as UNREALIZED_PROFIT_PCT
FROM {{ source('abc_bank', 'ABC_BANK_POSITION') }}
```

Now we also have the percent of profit concerning the position cost base.

All looks good, but maybe we do not like the long string of decimals from the division.

Let’s fix it by using the standard ROUND function from Snowflake.

Change the line as follows:

```
, ROUND(UNREALIZED_PROFIT / COST_BASE, 5)
    as UNREALIZED_PROFIT_PCT
```

Now we have rounded the result to the fifth decimal digit.

You can find the final version of this source file in the Git repository in the 05_04_v1__POSITION_ABC_BANK.sql file inside the materials for Chapter 5 in the dbt folder.