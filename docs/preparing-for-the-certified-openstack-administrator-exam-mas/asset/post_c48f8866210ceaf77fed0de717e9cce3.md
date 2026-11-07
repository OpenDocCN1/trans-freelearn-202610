
You can filter specific actions using grep. Here are some examples:

```
$ openstack help | grep domain
$ openstack help | grep project
$ openstack help | grep image
$ openstack help | grep server
$ openstack help | grep network
$ openstack help | grep volume
```

Step 2: After you find out the action you want to perform, you can provide --help to explore all available positional arguments and flags. Here are some examples:

```
$ openstack domain create --help
$ openstack project create --help
$ openstack image add project --help
$ openstack server create --help
$ openstack network list --help
$ openstack router create --help
```

As previously mentioned in Chapter 8, Swift Object-Storage Service, Swift requires you to use python-swiftclient.

Beware! Some previous versions of python-swiftclient display incorrect information about how to set ACLs on containers.

Refer to Chapter 8, Swift Object-Storage Service, on how to properly set ACLs on a container or refer to the official upstream Swift documentation available at https://docs.openstack.org/swift/.

## External monitors

If you are planning on taking the exam on a personal laptop, that's completely fine! Keep in mind that you are free to hook up one or many external monitors to the laptop while taking the exam. In some cases, using an external monitor will fit more details found on the Horizon dashboard and more output from the terminal. If you have an external monitor available (28" or larger), you may want to use it instead of relying on a small laptop screen.

![](img/da864e0670c65edf6f5ab861ef9906b3_322_0.png)

Figure 11.5: Using an external monitor may help you fit more dashboard and terminal details on one screen

## Issues during the exam

If you experience any network connectivity issues during the exam, remain calm and wait for the exam to reconnect. The proctor chat window should be available if you have any questions or if you experience any technical difficulties. If you have any concerns toward the end of the exam, ensure that you contact the proctor via the chat window before ending the exam.

## Diving deeper into OpenStack

The content in this book certainly prepares you to succeed on the COA exam, but it covers only the tip of the iceberg of OpenStack knowledge! There is still a whole world to uncover on OpenStack, including topics such as high availability, building cloud-aware apps on OpenStack, and OpenStack networking. I recommend that you check out these titles if you want to dive deeper: OpenStack Cloud Computing Cookbook - Third Edition by Kevin Jackson, Cody Bunch, and Egle Sigler, Troubleshooting OpenStack by Tony Campbell, and OpenStack Networking Essentials by James Denton.

## Summary

These tips and tricks will put you in a great position going into the exam. We reviewed the importance of project scope on the Horizon dashboard and CLI. Remember: if an objective can be completed on the Horizon dashboard, do it! It's typically faster than using the CLI unless you are a command-line pro! Time is extremely important and you only have about 4 to 5 minutes to complete each objective. With that said, don't waste your time with `http://docs.openstack.org` during the exam. Navigating the help in `python-openstackclient` is very useful in the event of forgetting some CLI arguments. Don't forget that older versions of `python-swiftclient` provide incorrect information on Swift ACLs, so use the syntax provided in this book or the `http://docs.openstack.org/swift`. And finally, be sure to check out other OpenStack books currently on the market! If this is your first OpenStack book, the cloud learning has just begun!

## 12 Practice Exam

Use this practice test to prepare for the Certified OpenStack Administrator exam.

### Before you begin...

Be sure to delete any previously running instances prior to starting. If you need help detecting running instances and deleting them, refer to the end of Chapter 5, Nova Compute Service.

### Instructions

Complete all objectives on the Horizon dashboard or CLI. Some questions may depend on resources created in other questions. Being able to complete all these objectives within one hour should put you in good standing for the exam. If a specific detail is not provided, assume the default value.

### Question 1

Within the **accounting** project, create a new tenant network called **tenant-network1**. It should have a subnet called **tenant-subnet1** with the following details:

- CIDR: 192.168.5.0/24
- DHCP: Enabled
- Allocation range: 192.168.5.50 to 192.168.5.60

### Question 2

From inside your terminal environment, build an openrc credential file for the admin user that will scope to the auto project.

### Question 3

Create a new keypair in the accounting project called keypair1.

### Question 4

Your infrastructure engineers just installed the Manilla service in your OpenStack environment. Create a new entry in the service catalog that advertises the following information:

- Service name: manilla
- Service description: OpenStack Shared File Systems
- Type: share
- Region: RegionOne
- Public endpoint: http://192.168.56.56:8786/v1/
- Internal endpoint: http://192.168.56.56:8786/v1/
- Admin endpoint: http://192.168.56.56:8786/v1/

### Question 5

Create a new security group in the trade project called database-sg. It should have one rule that allows incoming 3306 from the default security group.

### Question 6

Boot a new instance in the accounting project called instance1 with the m1.tiny flavor. It should use the Cirros image and should be attached to tenant-network1. Ensure that the keypair1 public key gets injected into the instance.

### Question 7

Create a new Nova flavor, accessible only by the **production** project. It should have the following properties:

- **Name**: nice-flavor
- **RAM**: 512 MB
- **VCPU**: 1
- **Disk**: 1 GB
- **Ephemeral**: 0
- **Swap**: 0

### Question 8

In the **accounting** project, create a new 1 GB empty volume called **volume1**. Attach the volume to **instance1**. After you have successfully attached the volume, use the novnc console to log in to the instance [username: cirros, password: cubswin:]. Format the attached volume with the ext3 filesystem and mount to the **/mnt** directory. Place a file on the mounted drive called `myfile.txt`. Unmount and detach the volume from **instance1**.

### Question 9

From the **auto** project, create an empty 1 GB volume called **volume1**. The volume should be from type: lvm.

### Question 10

Adjust the quotas in the **admin** project, allowing a total of 20 **instances** and 100 **volumes**.

### Question 11

Change the **sallyp** user email address to `sallyp@openstack.org`. Disable the account.

### Question 12

Create a Nova snapshot of instance1 in the accounting project. The snapshot should be called instance1-snapshot.

### Question 13

Create a router within the accounting project called router1. Add tenant-subnet1 to the router interface. Set the router gateway to provider-network-dashboard.

![](img/da864e0670c65edf6f5ab861ef9906b3_327_0.png)

If you don't see provider-network-dashboard, please go to Chapter 6, Neutron Networking Service and follow the exercises to create it.

### Question 14

In the accounting project, allocate the floating IP address 172.16.0.99. Assign this floating IP to instance1.

### Question 15

Attempt to get a reply by pinging instance1 via its floating IP address (172.16.0.99) set in the previous question. If you do not receive a reply, set the specific security group rule in the default security group to permit this.

![](img/da864e0670c65edf6f5ab861ef9906b3_327_1.png)

You must attempt to ping the instance from inside the virtual appliance, not your laptop or desktop system running VirtualBox.

### Question 16

Perform this specific task on the CLI only. Use wget to fetch a Cirros image from the official upstream website at https://download.cirros-cloud.net/0.3.5/cirros-0.3.5-x86_64-disk.img. Upload the qcow2 image to the trade project. The image should be named awesome-cirros. The image description should be "COA Exam Prep Awesome Image". The image should have a minimum RAM of 256 and minimum disk of 1 GB. It should be accessible only to the trade project.
