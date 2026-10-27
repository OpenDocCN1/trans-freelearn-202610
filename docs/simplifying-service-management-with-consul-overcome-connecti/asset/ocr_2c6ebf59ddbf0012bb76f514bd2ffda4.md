# Create a Consul cluster

Provision a Consul service based cluster instance.

# Cluster ID

what-a-cluster

# HashiCorp Virtual Network

Your HashiCorp Virtual Network will be utilized by your cluster. This will associate the cloud provider and region from the HashiCorp Virtual Network for your cluster's deployment.

veganet aws Oregon (us-west-2)

# Consul tier

# Development

For non-production deployment for testing and development purposes to connect up to 50 service instances. Development tier clusters have 1 Consul server.

Extra Small
$0.027/hr
Size
2 vCPU / 1 GiB RAM
Service instances
1-50

Ready to deploy production workloads? Add a payment method to enable Standard Tier Consul clusters
For production deployments to connect up to 10,000 service instances. Standard tier clusters have 3 Consul servers replicated across availability zones within a region for high availability.
Add a credit card

# Network accessibility

Allowing public connections is easier for testing because it does not need more configuration to connect to and communicate with your cluster, but is less secure.

Allow public connections from outside your selected network
Not recommended for production servers

# Consul version

Please specify which Consul version, if available, you would like to install on your cluster.

# Version selection

v1.9.8 (Recommended)

Create cluster
Cancel

Figure 8.4 – Configure a Consul cluster