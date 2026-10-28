# Summary

In this chapter, we departed a bit from the low-level monitoring of CPU, disks, and memory. We discussed a higher level of monitoring, one that looked at business services, called "IT services" in Zabbix. We were able to configure a service tree to represent real life dependencies and structure, link individual entries against triggers to propagate problem states to services, and configure SLA calculation for those services. We did not have a large IT system to test against, so we sent in fake data and observed the resulting reports, including a service availability report and yearly SLA graph.

We noted two important facts about IT service functionality in Zabbix:

+   - Triggers with severity of "Not classified" or "Information" are ignored when calculating the SLA
+   - SLA information cannot be calculated at a later time — the IT services must be configured in advance

For those cases when a service does not have full-time SLA coverage, we learned about a way to specify when the SLA calculation should take place based on weekly time periods — but we also noted that host and host group-level maintenance does not affect the SLA calculation and the uptime/downtime configuration has to be done for the IT services themselves.

In the next chapter, we'll go back to lower-level monitoring — even lower than before. We will cover monitoring hardware directly using the Intelligent Platform Management Interface (IPMI). Zabbix supports monitoring both "normal" or analog IPMI sensors and discrete IPMI sensors. There is even a special trigger function for discrete sensors. What is it? See the next chapter for details.