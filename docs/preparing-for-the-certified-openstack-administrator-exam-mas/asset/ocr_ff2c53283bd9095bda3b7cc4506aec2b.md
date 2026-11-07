# Question 2

From inside your terminal environment, build an openrc credential file for the admin user that will scope to the auto project.

# Question 3

Create a new keypair in the accounting project called keypair1.

# Question 4

Your infrastructure engineers just installed the Manilla service in your OpenStack environment. Create a new entry in the service catalog that advertises the following information:

+   - Service name: manilla
+   - Service description: OpenStack Shared File Systems
+   - Type: share
+   - Region: RegionOne
+   - Public endpoint: http://192.168.56.56:8786/v1/
+   - Internal endpoint: http://192.168.56.56:8786/v1/
+   - Admin endpoint: http://192.168.56.56:8786/v1/

# Question 5

Create a new security group in the trade project called database-sg. It should have one rule that allows incoming 3306 from the default security group.

# Question 6

Boot a new instance in the accounting project called instance1 with the m1.tiny flavor. It should use the Cirros image and should be attached to tenant-network1. Ensure that the keypair1 public key gets injected into the instance.