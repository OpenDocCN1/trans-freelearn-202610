So, 60 items per minute would result in 1 NVPS. By the way, one item per minute would be 1/60 or 0.01557. To get the total NVPS in the projected environment, we would simply multiply it all by the amount of hosts:

```
<average item count per host> / <average item interval> * <total host count>
```

Plug in various values and see how the expected NVPS changes as one of these values is changed. The more hosts you have, the more impact the average interval and average item count per host will have.

The value that the frontend gives us is a nice way to determine the expected NVPS right now, but it is not that easy to see how it has changed over time and how configuration changes have impacted it. We can add an internal item that will store this value so that we can see long-term changes and graph them. Navigate to Configuration | Hosts, click on Items for A test host, and then click on the Create item button. In this form, start by clicking on Select next to the Key field, and change the Type dropdown to Zabbix internal in the item helper. This presents us with a nice list of the available internal items. We will set up a few of these, but won’t discuss every single item in there. If you are curious about some we are done with this topic, consult the Zabbix manual for detailed information on each internal item. Remember how we created an item to monitor the time when the proxy last contacted the server? That also was an internal item.

In this list, click on zabbix[requiredperformance]. Fill in the following:

+   - **Name:** Expected NVPS
+   - **Type:** Zabbix internal
+   - **Type of information:** Numeric (float)
+   - **Units:** NVPS
+   - **New application:** Zabbix performance

When done, click on the Add button at the bottom. Check this item in the Latest data page. After a short while, it should have the value, somewhat similar to what we saw in the Zabbix status report:

| Zabbix performance (1 Item) |
| Expected NVPS | 2015-12-09 11:36:46  8.13 NVPS |

This value is likely to be different than the one we saw in the report. We just added an item to monitor the expected NVPS, which provides values of its own, so this action has affected the NVPS already.