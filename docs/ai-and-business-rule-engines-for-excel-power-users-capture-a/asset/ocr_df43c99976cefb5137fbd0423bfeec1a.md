4. Figure 11.15 shows a sample of this data:

|  | Order Number | Country | Customer Age | Country-Region Code |
|---|---|---|---|---|
| 0 | 9 | South Africa | 64 | 402 |
| 1 | 14 | United States | 68 | 103 |
| 2 | 84 | United States | 70 | 118 |
| 3 | 16 | United States | 80 | 108 |
| 4 | 21 | United States | 77 | 101 |
| ... | ... | ... | ... | ... |
| 94 | 93 | UK | 56 | 198 |
| 95 | 94 | Sri Lanka | 77 | 260 |
| 96 | 97 | Sri Lanka | 73 | 253 |
| 97 | 98 | New Zealand | 58 | 230 |
| 98 | 99 | UK | 69 | 190 |

|  | Qty Silk Tray | Qty Lumpy Bar | Qty Peanut | Qty Main Product |
|---|---|---|---|---|
| 0 | NaN | NaN | 55.0 | 244 Peanut Candy |
| 1 | NaN | NaN | 97.0 | 268 Peanut Candy |
| 2 | NaN | NaN | 97.0 | 451 Peanut Candy |
| 3 | NaN | NaN | 98.0 | 500 Peanut Candy |
| 4 | NaN | NaN | 98.0 | 389 Peanut Candy |
| ... | ... | ... | ... | ... |
| 94 | 71.0 | NaN | NaN | 142 Silk Tray |
| 95 | 97.0 | NaN | NaN | 292 Silk Tray |
| 96 | 92.0 | NaN | NaN | 370 Silk Tray |
| 97 | 73.0 | NaN | NaN | 220 Silk Tray |
| 98 | 87.0 | NaN | NaN | 522 Silk Tray |

[99 rows x 9 columns]

Figure 11.15 – The sample data loaded in Python

5. In the next cell, features and labels identify the columns (features) we use to make our prediction, and the output (labels) we expect in each case. This helps train our prediction model.

To keep our example simple, we are only training on two features. In reality, you could use 10s or even 100s of features for training, and experiment to find which give the clearest predictions:

```
features = sales_data[['Customer Age' ,'Country-Region Code']]
features.columns=["Customer_Age","Country_Code"]
label = sales_data["Main Product"]
```

6. Our notebook is missing a common step—splitting our data into 80% of data for training and keeping back 20% of lines to test how effective our training was. We’ll do that inspection visually in a later step since our sample data step is so small (100 lines). Either way, we can experiment with different algorithms and see which works the best.

7. The next cell sets the algorithm we want to use as GaussianNB in this step:

```
model = PMMLPipeline([("classifier", GaussianNB(),)])
```