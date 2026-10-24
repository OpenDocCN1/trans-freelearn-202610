# Creating a VPC for IoT Analytics

Now, we will get into the fun part by creating a fully-fledged, highly available cloud environment. You will have a secure functioning data center at your fingertips in about 20 minutes. We will leverage CloudFormation and a quick start template to create a VPC that has some key elements:

+   - **Public subnets**: Resources in this subnet can be discoverable over the public internet if configured with a public IP, an attached internet gateway, and appropriate route tables. You would only want to put things such as web servers or NAT instances here.
+   - **Private subnets**: Resources in these subnets are hidden from public view. Anything outside your VPC cannot find what you put here even if it has a public IP address. For security reasons; put things such as Hadoop clusters, databases, and EC2 instances created for analytics processing purposes here.
+   - **NAT gateways**: **Network Address Translation (NAT)** gateways serve as an intermediary between resources in private subnets and the public internet.
+   - **Bastion hosts**: These are necessary to allow you to connect to your EC2 instances and Hadoop clusters in the private subnets.

# What is a NAT gateway?

NAT gateways hide information about your resources while still allowing communication with the outside internet. They work by translating a private IP address into a different public IP address and remembering the translation and connection state when your internal resources request outside communication. Anything in the public internet is blocked from doing the reverse and never knows the real IP address of the resource in the private subnet.