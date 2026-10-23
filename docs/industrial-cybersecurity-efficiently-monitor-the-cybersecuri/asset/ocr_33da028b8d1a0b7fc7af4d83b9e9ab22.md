7. To trim down some of the noisy alerts, click on the minus (-) symbol (filter out) next to a rule.name when you hover over an alert you want to suppress. You can come back later (Edit visualization) to add any annoying noisy alerts.

8. Additionally, we can filter on event severity. To do this, click on the Add Filter button on the top of the editor screen and set Field to event.severity. Now, set Operator to is between and enter the values 3 and 8 for the bottom and top values, as illustrated in the following screenshot:

![](bbox=[0.24, 0.26, 0.73, 0.57])

Figure 9.49 – Filtering on severity

Click on Save to apply the filter.

9. With that, we just created a data table that shows us a summary of the NIDS alerts seen within the timeframe we specify (7 days). Click on Save, specify the name for this visualization as Breach Detection - NIDS Alerts Summary, and click on Save and return. This saves the visualization and adds it to our new dashboard.

10. We can resize and move the visualization to our liking while we are in Edit Mode for the dashboard.

11. Save the dashboard with the Save button on the top left of the editor screen, and name our new creation Breach Detection Portal.

We just created our initial breach detection dashboard, which for the moment only shows the NIDS alerts, but we will be adding some more data here soon, starting with Zeek notices next.