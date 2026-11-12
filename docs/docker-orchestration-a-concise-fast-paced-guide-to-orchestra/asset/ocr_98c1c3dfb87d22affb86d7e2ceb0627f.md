# Monitoring Your Cluster

Setting up orchestration for a Docker cluster is only half the battle. One also needs visibility into the cluster. Why is data retrieval slow? Is it disk or network? What is being logged by the web server for an application? How much memory is being used by an application? At some point, these or similar questions will come up and it will be up to you, as the administrator, to have the answers for these.

This chapter will look at different tools that can be deployed to assist in monitoring and troubleshooting. Some, such as Kibana, can provide a summary of the cluster health while others, such as Sysdig, can provide deep dive information on applications or individual containers.

This chapter will cover the following topics:

+   - Docker logging drivers
+   - Logging for containers that do not send logs to standard out or standard error
+   - Logging and viewing logs with the ELK stack
+   - Using remote logging with Kubernetes, Mesosphere, and Docker Datacenter
+   - Collecting performance metrics
+   - Graphing metrics with Kibana and Grafana
+   - Considerations for logging and troubleshooting
+   - Using Sysdig to view container metrics in real time

# Logging with containers

Nearly every application generates logs of some sort. They might track usage such as web server access logs or they may only show start up or error messages. In every case, logs have their use and not being able to see them can be a serious problem.