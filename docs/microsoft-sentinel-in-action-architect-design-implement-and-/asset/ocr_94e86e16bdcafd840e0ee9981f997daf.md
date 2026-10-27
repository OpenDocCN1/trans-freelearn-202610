# Query scheduling

Run query every *

Lookup data from the last *

# Alert threshold

Generate alert when number of query results *

# Event grouping

Configure how rule query results are grouped into alerts

Group all events into a single alert

Trigger an alert for each event (preview)

# Suppression

Stop running query after alert is generated

Figure 7.17 – Create new rule, Set rule logic page continued

The following table provides further details for each entry:

| Field Name | Description |
|---|---|
| Run query every | Select the numeric value and the time span (minutes, hours, or days) to determine how often the query runs. While there are no additional charges for running a query often, consider how often the suspicious activity would occur and how quickly you need to know about it. It is tempting to run every query as quickly as possible, but that can lead to alert overload. Do you need to be notified as quickly as possible of the low-severity event of someone entering the wrong password 5 times in 10 minutes?

This is the rule frequency when viewing the rule on the Overview page. |