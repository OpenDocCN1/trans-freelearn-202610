There are more event IDs that could indicate a failed login attempt, so I will leave it as a homework task for you to research additional event IDs that are related to failed logins and add them here (think domain login versus local login).

+   8. Click on Save, saving the widget as `Breach Detection - Failed Login Attempts over time.`

We now have a very convenient view of failed login attempts over time. Spikes in the graph indicate activity, and with us pulling logs from several machines, the spike can indicate either of the two attack scenarios we discussed at the beginning of this section.

Next, we will look at user creation and change logs.

# New user creation and changes to user accounts

We will now visualize alerts around new user creation and changes to user accounts. User creation or changes to a user account are great indicators of foul play in an environment, and alerts around these actions should be kept a close eye on. Follow along with these instructions to get a data table widget created that shows new or changed user account alerts:

+   1. Navigate to our **Breach Detection** dashboard (**Kibana | Dashboards | Breach Detection**) and click on **Edit** in the top left of the dashboard screen.

+   2. Click on the **Create New** button to add a new widget (visualization).

+   3. Select the `Data Table` visualization type and search for and add the `Security Onion - Alerts` data source.

+   4. Add a `Split Rows` data bucket and set **Aggregation** to **Terms**, with `rule.name.keyword` as the **Field** selection. Set the **Size** option to 50.

+   5. Add a `Split Rows` data bucket and set **Aggregation** to **Terms**, with `winlog.event_data.subjectUserName.keyword` as the **Field** selection. Set the **Size** option to 50.

+   6. Add a `Split Rows` data bucket and set **Aggregation** to **Terms**, with `winlog.event_data.targetUserName.keyword` as the **Field** selection. Set the **Size** option to 50.