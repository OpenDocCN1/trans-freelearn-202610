# High-Level Business Service Monitoring

If the entries are collapsed for you, expand them all and observe the final tree:

| SERVICE | ACTION | STATUS CALCULATION | TRIGGER |
|---|---|---|---|
| root | Add child |  |  |
| Customer services | Add child | Problem, if at least one child has a problem |  |
| Banana | Add child | Problem, if at least one child has a problem |  |
| Code repository | Add child Delete | Problem, if at least one child has a problem | Code repository down |
| Ticketing | Add child Delete | Problem, if at least one child has a problem | Ticketing down |
| Pineapple | Add child | Problem, if at least one child has a problem |  |
| Ticketing | Add child Delete | Problem, if at least one child has a problem | Ticketing down |
| Warehouse analytics | Add child Delete | Problem, if at least one child has a problem | Warehouse analytics down |

Note that we enabled SLA calculation starting from the company level. Computing total SLA across all customers is probably not a common need, although it could be done. In the **STATUS CALCULATION** column, all of our services have **Problem, if at least one child has a problem**. In the **SERVICE** properties, we could also choose **Problem, if all children have problems**. At this time, those are the only options for problem state propagation; setting the percentage or amount of child services is not possible (it could be useful for a cluster solution, for example).

# Sending in the data

Now is the time to send in our data, which will be a bit fake. As mentioned, IT services/SLA functionality is more interesting when we have data for a longer period of time, and we could try to send in data for a year. Of course, we won't create it manually—we will generate it. Create a script like this on the Zabbix server:

```
```bash
#!/bin/bash
hostname="IT services"
time_period=$[3600*24*365] # 365 days
interval=3600 # one hour
probability=100
current_time=$(date "+%s")
for item_key in code_repo warehouse_analytics ticketing; do
    [[ -f $item_key.txt ]] && {
        echo "file $item_key.txt already exists"
        exit
    }
    for ((value_timestamp=$current_time-$time_period;
          value_timestamp<$current_time;
          value_timestamp=$value_timestamp+$interval)); do
```
```