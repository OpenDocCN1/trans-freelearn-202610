Sometimes you may need to create multiple VPCs, such as for different workloads or different Regions. However, you may also need to have network connectivity between the VPCs you create. You will explore VPC interconnectivity next.

# Configuring VPC interconnectivity

Often a single VPC will not be enough, and you will find yourself creating separate VPCs for different workloads and applications as well as for different branch offices and locations. Sometimes you may need to share resources with other VPCs. In *Chapter 1, Introduction to AWS Accounts and Global Infrastructure*, you learned about the concept of hosting an AWS account to manage all shared infrastructure resources. VPCs that belong to other departments or branches of the company may wish to access resources in such shared VPCs. One way to grant access is to establish network connectivity between VPCs.

AWS offers several different options to connect your VPCs to each other, which will be explored next.

# VPC Peering

A VPC peering connection is a networking connection between two VPCs where traffic between them takes place over the AWS backbone network using private IP address space. EC2 instances in one VPC can then communicate with resources in a peered VPC. Peering connections can be configured between VPCs in your AWS accounts or even with VPCs that belong to other AWS accounts. Peering connections can also be established between VPCs within the same Region or even cross-Region.

A typical example would be where you have VPCs in different Regions that belong to branch offices of your company, and you need to establish a peering connection between them to share access to centralized resources.

Connectivity between VPCs never traverses the public internet, and this improves your security posture further by avoiding DDoS attacks and common exploits originating from the internet.

Setting up a peering connection requires sending a request from one VPC (the requester) to the other VPC (the accepter). Once the request is accepted, the peering connection is established. Note that you will still need to configure necessary routing rules in your route tables for the newly established network connections. The owner of each VPC in the VPC peering connection must create the necessary routes in the relevant route table to allow traffic to flow between the VPCs.