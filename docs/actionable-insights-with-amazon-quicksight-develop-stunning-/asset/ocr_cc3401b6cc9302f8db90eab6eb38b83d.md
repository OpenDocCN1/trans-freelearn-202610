# ## 3. This should trigger our Lambda function, which in turn will call the API and trigger a refresh. To verify, log in to QuickSight and select **Datasets**. Select the sample dataset and then edit to access the preview mode:

![](bbox=[0.168, 0.174, 0.839, 0.496])

| Name    | Age    |
|---------|--------|
| String  | Integer |
| George  | 54     |
| Anna    | 56     |

Figure 8.8 – Previewing the imported data

Note the new values in our dataset. QuickSight completed the ingestion, and now the new values are in SPICE, shortly after we updated the file. A key benefit of our event-driven architecture is that we don’t need to wait for a scheduled refresh – instead, data is refreshed as soon as we have new values.