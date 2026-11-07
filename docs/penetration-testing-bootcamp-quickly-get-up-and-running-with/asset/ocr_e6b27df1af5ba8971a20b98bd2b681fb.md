Your job as a penetration tester is to find weaknesses in the environment. So having these being clearly seen in as they pass over the wire is like forgetting to close your safe, where you have all of your most important and valuable items.

For my penetration-testing requirements, I will use a reverse SSH tunnel within a stunnel instance. I will show you the steps to set up this secure communication from the C&C server side to the on-premises box, which in my case is the Raspberry Pi. First, we will start inside in with the reverse SSH setup, followed by the stunnel setup. But when bringing up the connection, this happens in the opposite order, with the stunnel session getting set up first, followed by the reverse SSH session within that tunnel.

![](bbox=[0.09, 0.32, 0.18, 0.4])

Since this is an intermediate level book, I will not go into a complete step-by-step setup of some of these tools. I will assume that the base operating system is in place; in this case, it's Kali Linux on the laptop VM as well as the Raspberry Pi.

# Inside server SSH setup

Let's start by working on getting the Raspberry Pi, or whichever inside server you will use to set up the secure reverse SSH tunnel. The process itself is not really that difficult. It just involves setting up SSH since this host will be the one initiating connections back to the C&C server:

+   1. The first thing we do is back up old SSH keys and create new ones. On the Raspberry Pi, these keys are set up by default.
+   2. Typically, the Raspberry Pi Kali Linux image allows root login by default. I always verify to prevent unnecessary troubleshooting later.
+   3. Next, we need to allow SSH to start on reboot. This is a very important task. I have had situations where power was lost to one of the hosts, and SSH was not set to restart on boot. I had to manually console in and update. If the device is not local, it can be a hard lesson to learn.
+   4. Restart SSH, and you are ready to go. I always test it out as well just to make sure. So, try to SSH to the Raspberry Pi or whichever inside box you are using just to verify that everything is working.