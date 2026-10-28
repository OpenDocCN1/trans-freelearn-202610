> The default acceptable SLA level when adding a new service entry is 99.05, and we will leave it at this level for all services. When editing an existing service entry, the default is 99.9 instead. At the time of writing, it is not yet known when this might be fixed.

Click on **Add child** next to **Customer services** again. Enter "Pineapple" in the **Name** field, enable the **Calculate SLA** checkbox, then click on **Add**. Notice how the **Customer services** entry can be expanded now. Expand it and observe the result, which should be like this:

![](bbox=[0.385, 0.285, 0.615, 0.425])

The customers are in place; let's add their services now. Click on **Add child** next to **Banana**. Enter "Code repository" in the **Name** field and enable the **Calculate SLA** checkbox. This will be our "leaf" or lower-level service, and we will now link it to a trigger. The trigger state will affect the SLA state for this service and for all upper-level services with SLA calculation enabled. Click on **Select** next to the **Trigger** field, then click on **Code repository down** in the **NAME** column. The final configuration for this service should look like this:

![](bbox=[0.105, 0.6, 0.895, 0.865])