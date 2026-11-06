# Technical requirements

This chapter will mainly focus on Linux and high-level operating system concepts. For details on specific operating systems and MongoDB versions, see the documentation operations checklist at https://www.mongodb.com/docs/manual/administration/production-checklist-operations/ and the production notes at https://www.mongodb.com/docs/manual/administration/production-notes/ for self-managed deployments.

To monitor utilization of all kinds of system resources, you can use operating system commands, MongoDB Atlas monitoring, and other GUI tools that are designed specifically for monitoring multiple servers. More details will be provided in *Chapter 14, Monitoring and Observability*.

For system resources, Linux commands such as top, vmstat, iostat, and netstat can be used to monitor CPU, memory, storage, and network, respectively, at a point in time on a single server. Tools such as dstat and sar can be used to combine data from simpler tools and view it over a period of time.

MongoDB Atlas monitoring provides historical and real-time system resource data, which is integrated with database-specific metrics.

For self-managed deployments, you can use graphical tools such as Netdata, Prometheus + Grafana, Zabbix, Munin, or Observium. Finally, you can use commercial **application performance monitoring (APM)** tools such as AppDynamics, Datadog, Dynatrace, LogicMonitor, New Relic, SolarWinds, and Splunk Observability.

To try the configuration changes or commands mentioned in this chapter, you will need to use a Linux server and install MongoDB locally. You can download the MongoDB Community Edition from mongodb.com. You will also need to download the official drivers for the language of your choice (Ruby or Python): https://www.mongodb.com/docs/drivers/.

# Managing resources for optimal performance

So far, throughout this book, we’ve explored situations where performance is limited by a resource being used to its full capacity. For example, these resources can include CPU cycles, memory, storage, or network bandwidth. To improve performance, we either need to increase the availability of the limiting resource or reduce its usage. This section provides an overview of each type of resource and describes how it can impact MongoDB performance.