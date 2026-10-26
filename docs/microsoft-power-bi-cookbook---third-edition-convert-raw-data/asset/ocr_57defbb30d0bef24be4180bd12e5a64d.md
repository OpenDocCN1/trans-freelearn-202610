+   - Avoid reusing the same synonym across multiple entities, as this can lead to incorrect query results.
+   - Ensure that the primary synonym for each entity of the model is unique.

The entire semantic layer definition that drives Q&A can be exported, modified, and imported via Power BI Desktop using the Linguistic schema option in the ribbon of the Home tab while in Model view. The exported file is a YAML ain’t Markup Language (YAML) file. YAML is a human-readable text file used for data serialization. The portion of the YAML file after adding the wholesaler net margin synonym looks like the following:

```
reseller_sale.reseller_net_sales:
  Definition:
    Binding: {Table: Reseller Sales, Measure: Reseller Net Sales}
  State: Generated
  Terms:
  - reseller net sales: {State: Generated}
  - wholesaler net sale: {LastModified: '2021-04-05T16:58:30.2996117Z'}
  - sales: {State: Generated, Weight: 0.97}
  - net sales: {State: Generated, Weight: 0.97}
  - sale: {State: Suggested, Weight: 0.97}
  - net sale: {State: Suggested, Weight: 0.97}
  - reseller net sale: {Type: Noun, State: Suggested, Weight: 0.739}
  - wholesaler net auction: {Type: Noun, State: Suggested, Weight: 0.492}
  - net auction: {Type: Noun, State: Suggested, Weight: 0.485}
  - wholesaler net transaction: {Type: Noun, State: Suggested, Weight: 0.482}
  - auction: {Type: Noun, State: Suggested, Weight: 0.476}
  - net transaction: {Type: Noun, State: Suggested, Weight: 0.475}
  - wholesaler net deal: {Type: Noun, State: Suggested, Weight: 0.468}
  - wholesaler net trade: {Type: Noun, State: Suggested, Weight: 0.468}
  - wholesaler net vending: {Type: Noun, State: Suggested, Weight: 0.468}
  - wholesaler net retailing: {Type: Noun, State: Suggested, Weight: 0.468}
```

# See also

+   - Use Power BI Q&A in a report to explore your data and create visuals: http://bit.ly/2QYa7JN
+   - Best practices to optimise Q&A in Power BI: http://bit.ly/3cSddYp
+   - Limitations of Power BI Q&A: http://bit.ly/2PtCrDL

# Adding Alerts and Subscriptions

Alerts can be configured on the tiles of dashboards to provide a notification that a specific condition has been met. Alert rules and options can be managed in the Power BI service, and notifications can be limited to the notification center in Power BI or shared via email. Data-driven alerts enhance the value of Power BI dashboards, as they immediately bring attention to significant events or outcomes as the semantic model supporting the dashboard tile is refreshed.