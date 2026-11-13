# Stop master and slaves

Spark master and slaves can be stopped using the following scripts:

| $SPARK_HOME/sbin/stop-master.sh: This script is used to stop Spark Master nodes.

+   - $SPARK_HOME/sbin/stop-slave.sh: This script is used to stop individual slave nodes. This should be executed to the individual nodes
+   - $SPARK_HOME/sbin/stop-slaves.sh: This script is used to stop all slave nodes together. This should be executed on the Spark master node. All Hostnames of slave nodes should be added in the $SPARK_HOME/conf/slaves file.

Apart from the preceding scripts, the following scripts are available to start/stop Spark master and Spark slaves together:

sbin/start-all.sh- This script is used to start both Spark master and Spark slave nodes. This should be executed on Spark master node. All hostnames of slave nodes should be added in the $SPARK_HOME/conf/slaves file.

sbin/stop-all.sh- This script is used to stop both Spark master and Spark slave nodes. This should be executed on Spark master node. All hostnames of slave nodes should be added in the $SPARK_HOME/conf/slaves file.