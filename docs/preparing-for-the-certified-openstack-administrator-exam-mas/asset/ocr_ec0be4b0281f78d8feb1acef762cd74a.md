# About Cinder

When Amazon Web Services EC2 launched in 2006, it looked a lot different than we know it today. One of the primary differences was how AWS handled virtual machine storage. At that time, AWS only offered **ephemeral** storage for virtual machine instances. Consider a user booting an EC2 instance with an instance type (or flavor) that offered a 20 GB primary disk and a 50 GB secondary disk. Any data written to those local disks (that is, `/dev/xvda` or `/dev/xvdb` inside the operating system) would reside on the compute node hosting that instance. See Figure 7.1.

![](bbox=[0.27, 0.27, 0.76, 0.68])

Figure 7.1: Amazon EC2 virtual machine ephemeral storage

It wasn't long before many users encountered problems:

+   - Data copied to the disks could not be easily transferred to other virtual machines.
+   - Stopping or terminating the instance meant complete data loss (once started again, the virtual machine would boot another compute node with newly provisioned disks).
+   - If the instance failed, your data was not accessible and could not be easily recovered.