# ## SilentDefense alerts

The final alert summary widget we will cover in this exercise is around showing SilentDefense alerts. We want to show interesting events that our OT-IDS alert is catching. To do this, follow this procedure:

+   1. Navigate to our **Breach Detection** dashboard (**Kibana | Dashboards | Breach Detection**) and click on **Edit** in the top left of the dashboard screen.
+   2. Click on the **Create New** button to add a new widget (visualization).
+   3. Select the **Data Table** visualization type and search for and add the `* :so-*` data source.
+   4. Enter an `event .dataset :ot-ids` search term to filter out SilentDefense logs only, then hit **Update**.
+   5. Add a **Split Rows** data bucket and set **Aggregation** to **Terms**, with `source .ip` as the **Field** selection. Set the **Size** option to 50.
+   6. Add a **Split Rows** data bucket and set **Aggregation** to **Terms**, with `rule . category .keyword` as the **Field** selection. Set the **Size** option to 50.
+   7. Add a **Split Rows** data bucket and set **Aggregation** to **Terms**, with `rule . name .keyword` as the **Field** selection. Set the **Size** option to 50.
+   8. Click **Update** to apply data buckets.
+   9. Filter out any noisy (uninteresting) alert categories such as `NameResolution` and alerts such as `FileRead`, to clean up the view.
+   10. Click on **Save**, saving the widget as `Breach Detection – SilentDefense Alerts Summary`.

We now have a widget that shows us interesting OT-IDS alerts.

There are many more widgets we could cover here, and additionally, there are many more views, columns, and filters we could apply to customize the overall **Breach Detection** dashboard. Over time, when you start using this information, you will adapt the views and data to fit your needs. For now, you should have a solid knowledge of Security Onion and Kibana’s capabilities to get you started. Let’s finish up the dashboard with some standard widgets that should be present on every dashboard.