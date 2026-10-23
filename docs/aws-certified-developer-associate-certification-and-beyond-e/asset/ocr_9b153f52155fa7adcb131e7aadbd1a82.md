Finally, let us briefly discuss the Classic Load Balancer.

# Classic Load Balancer (CLB)

The Classic Load Balancer is a previous-generation load balancer and one that AWS does not recommend for any modern production-level environments of applications. The CLB, as depicted in the following screenshot, can support both layer 7 and layer 4 traffic but lacks many of the advanced features and robustness of the ALB and the NLB.

Classic Load Balancer - previous generation

# Classic Load Balancer Info

![](bbox=[0.15, 0.325, 0.86, 0.485])

Figure 7.5 – Setting up a CLB

CLBs support SSL termination, as depicted in the diagram in Figure 7.5, the offloading of encryption tasks to the load balancer, and the management of SSL certificates. There is also support for both IPv4 and IPv6 traffic.

In this section, we examined AWS Elastic Load Balancers in detail and learned how you can build your application architecture to offer high availability by handling and distributing traffic to resources hosted in your AWS account. In the next section, you will learn more about scalability, a vital ingredient in enabling organizations to design elastic solutions, ensuring consistency, reliability, accessibility, and cost control. Auto Scaling for EC2 instances is discussed in the next section.