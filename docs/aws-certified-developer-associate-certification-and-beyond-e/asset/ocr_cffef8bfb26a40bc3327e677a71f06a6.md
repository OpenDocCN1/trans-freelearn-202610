10. Under **Security groups**, select **NLB-Security-Group**, which you created earlier. Also, ensure you remove the default security group from the selection.

11. Under **Listeners and routing**, select **todo-list-alb-target-group**, which you created earlier, for the **Default action** forwarding rule, as per the following screenshot:

![](bbox=[0.13, 0.2, 0.86, 0.42])

Figure 8.19 – Configure listeners and routing for your network load balancer.

12. Click the **Create load balancer** button at the bottom of the page.

# Additional Steps – Important Note

Recall that when you created the security group for the load balancer in *Project Task 8*, you did not create an inbound rule. This is because it is impossible to identify the traffic source as the API Gateway for inbound traffic. However, we need to ensure that only the API Gateway can send traffic to the load balancer, so you do not want to open inbound rules on the security group. The security group is still needed because its security group ID will be referenced to ensure that traffic to the application load balancer is only allowed from the network load balancer.

To facilitate communication between the API Gateway and the network load balancer, you must create a VPC link (endpoint) for the REST APIs, which will send traffic to the load balancer over AWS PrivateLink. However, since we are not creating any inbound rules on the security group, a workaround is required to enable traffic from the VPC link to reach the network load balancer. In this additional step, you will configure this workaround.