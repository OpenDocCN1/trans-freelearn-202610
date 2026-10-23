
In addition to CloudWatch, AWS offers other monitoring services tailored to specific needs. These include Amazon GuardDuty for threat detection, AWS X-Ray for distributed tracing of applications, and AWS Trusted Advisor for analyzing AWS resources and providing recommendations on cost optimization, performance, and security. By leveraging these AWS monitoring services, users can maintain visibility of their cloud environment, optimize performance, troubleshoot issues, and enhance security. There are two types of status checks:

- **System Status Checks:** The continuous monitoring of the AWS infrastructure supporting an EC2 instance is facilitated by the EC2 instance status check. This check helps identify any failures that could arise from various factors, such as disruptions in network connectivity, power outages, or issues with the host machine’s hardware or software. Upon detecting a failure, AWS initiates measures to rectify the problem. Concurrently, users can act by stopping and restarting the affected instance, effectively transferring it to a different host machine.
- **Instance Status Checks:** The purpose of monitoring is to oversee the software and network configurations of an EC2 instance. Various factors can trigger errors, such as improper network or boot settings, insufficient memory, or corrupted file systems. In case of any issues, users must address them, potentially needing to terminate the problematic instance and create a new one in severe cases. Maintaining adequate backups of your data is of utmost importance to avoid data loss.

In this section, you examined additional features of the Amazon EC2 service. In the next section, you will learn about the various options to access your EC2 instances.

## Accessing Your EC2 Instances

Access to your EC2 instances from the internet requires ensuring adequate levels of security and enforcing policies or rules to limit the type of traffic allowed into your instances. You can remotely connect to your instances over the internet using Secure Shell (SSH) for Linux-based EC2 instances or Remote Desktop Protocol (RDP) for Windows-based EC2 instances.

How you connect to your EC2 instance also depends on its placement within your VPC. For example, instances placed within public subnets that have direct access to the internet can be easily accessed as long as security groups allow inbound connections to relevant ports and your VPC has an internet gateway. Furthermore, your Network Access Control Lists (NACLs) must allow both inbound and outbound connections to the subnet that hosts your EC2 instance.

The following diagram depicts the architecture when using a bastion host:

![](img/34ff9b5b5a734dae3450ab701e223c73_185_0.png)

Figure 5.3 – Using a bastion host to connect to EC2 instances in the private subnet

In Figure 5.3, the administrator will first establish a remote connection to the bastion host and then, from that host, proceed to establish a new remote connection to the application server in the private subnet. Here are some key points to note.

The security group tied to the bastion host must allow inbound connections from the internet or a specified IP address/range on the public internet. You can limit the connection to specific ports, such as port 22 for SSH or port 3389 for RDP.

A second security group is tied to the application server for remote administration traffic, allowing inbound connections from only the bastion host.

With the security groups and NACLs correctly configured, the administrator can access the EC2 instance in the private subnet.

Using bastion hosts to connect to your other EC2 instances is one option for remotely administering your servers in the cloud. However, there are some risk factors associated with this approach. Most notable is that the bastion host is exposed on the public internet by being placed in the public subnet. While security group rules will prevent access except for traffic on specified ports, these instances get bombarded with rogue internet requests – packets that are dropped thanks to the security group restrictions but nonetheless result in processing by the instance. It is easy to flood bastion hosts with such rogue requests. You must ensure that you architect additional configurations to reduce the risks of anyone accessing your environment by brute force.

As an alternative, AWS offers another solution to administer your instances remotely and securely without opening ports on the firewall. The solution is offered by a service called Systems Manager Session Manager. Let’s look at this service next.

## Accessing Your EC2 Instances with AWS Systems Manager Session Manager

AWS Systems Manager offers many tools and utilities to manage your resources, such as EC2 instances on AWS. Features include patch management, inventory management, running commands across a fleet of instances without physically connecting to them, and remote administration of your EC2 instances with Session Manager.

Session Manager enables you to connect to EC2 instances in the cloud remotely and on-premises virtual and physical servers. With Session Manager, you do not need to open ports or manage SSH keys. Furthermore, you create policies and comply with your company’s processes and procedures, ensuring only authorized nodes are accessed while maintaining necessary audit logs and node access details.

To configure your EC2 instances such that you can manage them with Session Manager, you will need to ensure the following pre-requisites are in place:

- **Operating System (OS) support**: Session Manager supports all versions of Linux and Windows Server 2012 and later. In addition, Session Manager also supports macOS. To use Session Manager, you must ensure that your EC2 instance is running a supported OS.
- **SSM Agent**: Session Manager requires you to install SSM Agent on the EC2 instance or the on-premises virtual server before it can be enrolled as a managed node. Most of the latest AMIs offered by AWS have the agent pre-installed. Where installation is required, aim to ensure you install the latest version possible.
- **SSM-user**: Session Manager uses a unique identity on your managed nodes that has administrative privileges. Note that if you need to manage Microsoft Windows Server instances configured as domain controllers, you must do so manually. Create this user and assign it with domain administrator permissions.
- **IAM role**: To access your EC2 instances with Session Manager, you must ensure that an IAM role is assigned to the instance that allows Session Manager to communicate and manage the instance. The role must have the `AmazonSSMManagedInstanceCore` policy attached to it.

With the prerequisites, you can now access and remotely administer your EC2 instances without needing a bastion host, as illustrated in the following diagram.

![](img/34ff9b5b5a734dae3450ab701e223c73_187_0.png)

Figure 5.4 – Remote administration of EC2 instances using AWS Session Manager

In Figure 5.4, you can see how the administrator can remotely access the EC2 instance using the AWS Systems Manager Session Manager service, all without the need to open ports and use bastion hosts.

Here are some key points to note:

- You will need to configure Session Manager to manage your nodes; this involves installing SSM Agent on the EC2 instance.
- You must have an IAM role that enables Session Manager to manage the node.
- You can access the EC2 instance and even have remote desktop client access to Microsoft Windows desktop environments.
- For instances in the private subnet of your VPC, you must ensure that you configure a VPC endpoint or deploy a NAT gateway with necessary route table configurations to provide connectivity between instances in the VPC and AWS Systems Manager. If you opt to allow outbound internet access via a NAT gateway, you must also ensure that you allow HTTPS (port 443) outbound to `ssm.region.amazonaws.com`, `ssmmessage.region.amazonaws.com`, and `ec2messages.region.amazonaws.com`.
