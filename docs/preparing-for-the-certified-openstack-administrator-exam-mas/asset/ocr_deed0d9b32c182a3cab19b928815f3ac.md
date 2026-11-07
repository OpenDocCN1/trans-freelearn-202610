# Exam objective - managing floating IPs

In the previous objective, we set our router gateway to the provider network. This provided all the instances attached to networks connected to the router with access to the internet. But what if we wanted to access an application or resource on one of these instances from outside our *OpenStack environment*? Because the IP address assigned to our instances is internal and not publicly routable, Neutron allows users to associate floating IPs with their instances. Floating IPs are similar to the concept of elastic IPs in the Amazon Web Services world. Using them is a two-step process:

+   1. **Allocate** a floating IP to your project.
+   2. **Assign** the floating IP to an instance.

If you no longer need your floating IP, you can easily unassign and release it from the project, sending it back into the original floating IP address pool. Remember: the floating IP address pool was defined in the provider network subnet. In the real world, this could be an external IP address range provided by our internet service provider.

# Horizon dashboard

Let's associate a floating IP address with our **instance1-dashboard** instance in the **logistics** project. We will then attempt to ping the instance from our virtual appliance via its floating IP. Because we have the admin role, we will see how to assign a specific IP address via the admin-system panel group. Let's begin!

+   1. See Figure 6.57. From the OpenStack overview page, select the **Admin-System** panel group.
+   2. Select the **Floating IPs** panel.