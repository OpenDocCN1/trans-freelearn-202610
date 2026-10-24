You can connect to data from Microsoft tools such as Power BI Desktop and Excel. You can also use connectors to link custom applications and some browser-based tools.

![](bbox=[0.18, 0.16, 0.81, 0.35])

* Power BI Embedded is not yet supported in preview as of Microsoft December 2016. Image source: Microsoft Azure

# HDInsight

Azure HDInsight uses the Hadoop components from the Hortonworks Data Platform (HDP) distribution. It deploys managed clusters in the cloud with a focus on high reliability and availability. Microsoft Active Directory is used for security and governance.

HDInsight includes the implementations of Hadoop ecosystem tools, such as Apache Spark, HBase, Kafka, Storm, Pig, Hive, Interactive Hive, Sqoop, Oozie, and Ambari. It also integrates with Business Intelligence (BI) tools, such as Power BI, Excel, SQL Server Analysis Services, and SQL Server Reporting Services.

Default storage for the HDFS used by HDInsight clusters can be either an Azure storage account or a Azure Data Lake store.

# The R server option

In 2015, Microsoft bought a company called revolution analytics, which maintained a managed version of the open source R distribution. Since then, Microsoft has been integrating R into several of its software products and Azure has been no exception.

HDInsight includes an option to integrate an R Server into your HDInsight cluster when the cluster is created. This allows R scripts to use Hadoop to run distributed computations. Microsoft includes a big data analytics R package called ScaleR.