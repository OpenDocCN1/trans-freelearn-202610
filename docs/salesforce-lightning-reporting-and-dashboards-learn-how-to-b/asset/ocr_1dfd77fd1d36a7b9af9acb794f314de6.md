# Advanced Report Configuration

The summary formula will be added next to amount subtotal for each month; from here, we can easily check the month by month percentage and see that it increases for all Open and Won Opportunities:

| CLOSE DATE | QUALIFICATION AMOUNT Sum | QUALIFICATION MONTH BY MONTH | ID. DECISION MAKERS AMOUNT Sum | ID. DECISION MAKERS MONTH BY MONTH | CLOSED WON AMOUNT Sum | CLOSED WON MONTH BY MONTH | Total AMOUNT Sum | Total MONTH BY MONTH |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| March 2016 | $0.00 |  | $15,000.00 |  | $22,000.00 |  | $37,000.00 | - |
| April 2016 | $27,000.00 |  | $0.00 |  | $40,000.00 |  | $67,000.00 | 81.08% |
| May 2016 | $55,000.00 |  | $50,000.00 |  | $0.00 |  | $105,000.00 | 56.72% |
| June 2016 | $80,000.00 |  | $10,000.00 |  | $0.00 |  | $90,000.00 | -14.29% |
| Total | $162,000.00 |  | $75,000.00 |  | $62,000.00 |  | $299,000.00 |  |

Note that there are no value shown in the formula for March 2016 because it is the first value. In **April 2016**, an increase of **81.08%** is compared to the previous group, which is **March 2016**. **May 2014** sees an increase of **56.72%** compared to **April 2016**, and the last group shows a **-14.29%** decrease compared to the group earlier. As mentioned earlier, the `PREVGROUPVAL()` function will not know the value of the previous group. In this sample, it just shows nicely because there is no skipped month in the report.

# Summary

In this chapter, we explored the bucket fields and **Custom Summary Formulas** in detail. With the bucket field, we can categorize data for report purpose, and nothing needs to be done on the field in the object. This will eliminate the admin right to create a custom field, and if that field only serves for one or two reports. However, bucket fields are not used to replace formula fields; you can't have a formula calculation in the bucket field; you can have it to group values into a bucket.

Bucket fields only work for certain field types, and different field types will have different ways configured for the bucket value; you can use the range for currency, number, and percentage but not for others.