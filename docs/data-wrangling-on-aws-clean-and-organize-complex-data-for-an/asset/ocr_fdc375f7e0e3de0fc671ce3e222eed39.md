![](bbox=[0.13, 0.09, 0.85, 0.25])

Figure 9.7: Querying the S3 file using S3 Select in the AWS console

2. In the AWS console, verify the input and output settings are correct. It will be auto-populated and inferred correctly for most use cases, but you can verify it for our use case as well. We stored our input data in the GZIP compressed comma-separated values (CSV) file format.

![](bbox=[0.34, 0.37, 0.66, 0.63])

Figure 9.8: The AWS console S3 Select configurations

If you run a query with output format as CSV, you can see the header is populated correctly in the formatted view. However, there is an issue with the column and field alignment, and we will explore this further in the next steps. Look at the columns, starting with name:

| position | player_number | name | status | stat1 | stat1_val | stat2 | stat2_val | stat3 | stat3_val | stat4 | stat4_val | team |
| OT | 66 | Graf\ | Kevin | ACT | G | -- | GS | -- |  |  |  | IND |
| FS | 32 | Green\ | T.J. | ACT | TCKL | -- | SCK | -- | FF | -- | INT | -- | IND |
| FS | 27 | Guy\ | Winston | ACT | TCKL | 6 | SCK | 0 | FF | 0 | INT | -- | IND |
| OT | 73 | Haeg\ | Joe | ACT | G | -- | GS | -- |  |  |  | IND |

Figure 9.9: Exploring the CSV file data using S3 Select