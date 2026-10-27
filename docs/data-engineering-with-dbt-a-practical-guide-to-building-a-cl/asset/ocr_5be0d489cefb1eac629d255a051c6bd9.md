This will run and test all the models in the staging folder, including the old and new HIST tables:

![](bbox=[0.13, 0.13, 0.85, 0.31])

Figure 14.3: Results of the run with the new portfolio as input

Looking at the result of the run, you can see that two rows have been inserted into the normal HIST table. They are the row for the new position in 'SAFE', and for the changed position in 'STAR':

| Row | ACCOUNT_CODE | SECURITY_CODE | REPORT_DATE | QUANTITY | COST_BASE | POSITION_VALUE | LOAD_TS_UTC |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | ABC000123 | GCM | 2021-04-09 | 100 | 559.00000 | 678.00000 | 2023-03-08 14:12:32... |
| 2 | ABC000123 | SAFE | 2022-04-23 | 100 | 4015.00000 | 4389.50000 | 2023-03-08 14:14:29... |
| 3 | ABC000123 | STAR | 2021-04-09 | 150 | 2444.00000 | 2725.50000 | 2023-03-08 14:12:32... |
| 4 | ABC000123 | STAR | 2022-04-23 | 200 | 3125.00000 | 2950.50000 | 2023-03-08 14:14:29... |
| 5 | ABC000123 | TTD | 2021-04-09 | 10 | 5310.45000 | 6925.60000 | 2023-03-08 14:12:32... |

Figure 14.4: Selected contents of the normal HIST table

Note that the position in 'GCM' is still open, and only two rows have been updated by the run at 14:14 with the new contents of the portfolio.

On the other hand, the new HIST table with the closed position has three new entries. Two are the same as the old HIST table, while the third is the closed position in 'GCM':

| Row | ACCOUNT_CODE | SECURITY_CODE | REPORT_DATE | QUANTITY | COST_BASE | POSITION_VALUE | LOAD_TS_UTC | CLOSED |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | ABC000123 | GCM | 2021-04-09 | 100 | 559.00000 | 678.00000 | 2023-03-08 14:12:32... | FALSE |
| 2 | ABC000123 | GCM | 2022-04-23 | 0 | 0.00000 | 0.00000 | 2023-03-08 14:14:29... | TRUE |
| 3 | ABC000123 | SAFE | 2022-04-23 | 100 | 4015.00000 | 4389.50000 | 2023-03-08 14:14:29... | FALSE |
| 4 | ABC000123 | STAR | 2021-04-09 | 150 | 2444.00000 | 2725.50000 | 2023-03-08 14:12:32... | FALSE |
| 5 | ABC000123 | STAR | 2022-04-23 | 200 | 3125.00000 | 2950.50000 | 2023-03-08 14:14:29... | FALSE |
| 6 | ABC000123 | TTD | 2021-04-09 | 10 | 5310.45000 | 6925.60000 | 2023-03-08 14:12:32... | FALSE |

Figure 14.5: Selected contents of the HIST table with closed positions