![](bbox=[0.22, 0.11, 0.78, 0.28])

Figure 1.1: UF forwarding web server logs to indexer

Let us now look at SH, which is a critical user-facing processing component in a distributed deployment.

HF is a Splunk Enterprise instance and doesn't require separate binary for installation. It provides an extended feature set compared to a UF. It not only collects and forwards data, but also includes a Splunk user interface for configuration and management. To operate an HF, a forwarder license is required. Typically, an HF is configured in forwarding mode by disabling local data storage. Splunk Add-ons available on Splunkbase can be installed on an HF to facilitate data collection from various sources. This combination of features makes HFs a versatile choice for preprocessing and forwarding data while benefiting from a user-friendly interface.

# SH

The SH component is a Splunk Enterprise instance that is dedicated to search management and provides a number of interfaces for users to interact with. The popular interfaces it offers to users are web, CLI, and RESTful API.

Multiple SHs can be grouped together and form a cluster called a SH cluster (SHC). Members of an SHC share the same baseline configuration, and jobs are allocated to available members by the SH captain.

In a standalone deployment, a single Splunk Enterprise instance (that is, the same instance) works as both the SH and indexer. In a distributed deployment model, the SH or SHC can submit searches to multiple indexers and consolidate the results returned. The results are stored locally in a dispatch directory located in $SPLUNK_HOME/var/run/splunk/dispatch for later retrieval, and the results will be deleted after the job expires. $SPLUNK_HOME refers to the installation directory where the Splunk software is installed. For example, ad hoc search results (that is, the search job outcome) are retained for 10 minutes in the dispatch directory, which will be removed after the job expires by a process called the dispatch reaper, which runs every 30 seconds.