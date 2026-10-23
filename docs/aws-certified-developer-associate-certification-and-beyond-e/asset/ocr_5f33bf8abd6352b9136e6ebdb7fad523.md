# Network mapping

The load balancer routes traffic to targets in the selected subnets, and in accordance with your IP address settings.

VPC Info
Select the virtual private cloud (VPC) for your targets or you can create a new VPC. The selected VPC can't be changed after the load balancer is created. To confirm the VPC for your targets, view your target groups.

todolist-vpc
vpc-081922287f0f81390
IPv4 VPC CIDR: 10.0.0.0/16

Mappings Info
Select at least two Availability Zones and one subnet per zone. The load balancer routes traffic to targets in these Availability Zones only. Availability Zones that are not supported by the load balancer or the VPC are not available for selection.

us-east-1a (use1-az1)
Subnet
subnet-0da90144bce993455 todolist-app-subnet01
IPv4 address
Assigned from CIDR 10.0.10.0/24

us-east-1b (use1-az2)
Subnet
subnet-0872444131d8229d1 todolist-app-subnet02
IPv4 address
Assigned from CIDR 10.0.11.0/24

Figure 7.30 – Selecting the appropriate subnets for your load balancer

+   10. Next, under Security groups, ensure that you select the security group you configured for your load balancer, for example, ALB-Security-Group, which was created in Project Task 4.6 in Chapter 4, Building Private Networks in the Cloud with Amazon VPC.

+   11. Under Listeners and routing, configure a listener for the HTTP protocol on port 80 to point to the target group you created in Step 4, by selecting it from the drop-down list.

+   12. Leave all other sections at their default values and click the Create load balancer button at the bottom of the page.

+   13. You should receive a message stating that the load balancer has been successfully created. You can then click on the View load balancer button. The load balancer will take a couple of minutes to be provisioned. Once it has been provisioned, you can then test to see whether the EC2 instances are in a healthy state.