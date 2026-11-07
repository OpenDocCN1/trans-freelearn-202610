# Question 7

Create a new Nova flavor, accessible only by the **production** project. It should have the following properties:

+   - **Name**: nice-flavor
+   - **RAM**: 512 MB
+   - **VCPU**: 1
+   - **Disk**: 1 GB
+   - **Ephemeral**: 0
+   - **Swap**: 0

# Question 8

In the **accounting** project, create a new 1 GB empty volume called **volume1**. Attach the volume to **instance1**. After you have successfully attached the volume, use the novnc console to log in to the instance [username: cirros, password: cubswin:]. Format the attached volume with the ext3 filesystem and mount to the **/mnt** directory. Place a file on the mounted drive called `myfile.txt`. Unmount and detach the volume from **instance1**.

# Question 9

From the **auto** project, create an empty 1 GB volume called **volume1**. The volume should be from type: lvm.

# Question 10

Adjust the quotas in the **admin** project, allowing a total of 20 **instances** and 100 **volumes**.

# Question 11

Change the **sallyp** user email address to `sallyp@openstack.org`. Disable the account.