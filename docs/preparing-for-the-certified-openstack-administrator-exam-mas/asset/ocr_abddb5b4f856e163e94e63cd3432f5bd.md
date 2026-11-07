# Exam objective - managing flavors

In OpenStack, you always need to specify the desired RAM, local disk drive size, and virtual CPUs for your virtual machine instance. To control usage, OpenStack requires users to choose from a collection of previously defined hardware configurations called flavors. See Figure 5.6.

![](bbox=[0.05, 0.25, 0.96, 0.44])

Figure 5.6: A list of Nova flavors in a typical OpenStack environment

+   - **ID:** UUID of the flavor.
+   - **Name:** Name of the flavor.
+   - **RAM:** Amount of RAM to use in megabytes.
+   - **Disk:** Amount of disk space to use for the root partition in gigabytes.
+   - **VCPU:** Number of virtual CPUs. By default, Nova uses a 16:1 CPU allocation ratio. This allows you to boot sixteen 1VCPU instances per physical core.
+   - **Ephemeral:** Amount of disk space to use for the secondary partition.
+   - **Swap:** Amount of disk space to use for swap in gigabytes.
+   - **RXTX Factor:** This is only used by Xen or NSX-based systems and allows throttling of the inbound/outbound traffic from the network attached to the instance.
+   - **Properties:** Any metadata set on the flavor.

By default, creating a new flavor is only granted to a user with the admin role. As an admin, it's easy to create and customize new flavors from the Horizon dashboard or `python-openstackclient`.