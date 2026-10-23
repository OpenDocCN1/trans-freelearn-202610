
11. Select the interface for the LAN side of the firewall (2, in my case) and specify the IP address you want to assign to that interface (172.25.100.1), as illustrated in the following screenshot:

```
WAN (wan)         -> em0        -> v4/DHCP4: 172.25.20.190/24
LAN (lan)         -> em1        -> v4: 192.168.1.1/24

0) Logout (SSH only)               9) pfTop
1) Assign interfaces             10) Filter Logs
2) Set interface(s) IP address    11) Restart webConfigurator
3) Reset webConfigurator password 12) PHP shell + pfSense tools
4) Reset to factory defaults      13) Update from console
5) Reboot system                  14) Enable Secure Shell (sshd)
6) Halt system                    15) Restore recent configuration
7) Ping host                     16) Restart PHP-FPM
8) Shell

Enter an option: 2

Available interfaces:
1 - WAN (em0 - dhcp, dhcp6)
2 - LAN (em1 - static)

Enter the number of the interface you wish to configure: 2

Enter the new LAN IPv4 address. Press <ENTER> for none:
> 172.25.100.1
```

![](img/c8ffa9959721097701584fcc95101362_196_0.png)

12. Specify a netmask (24).

13. Simply hit *Enter* to skip specifying a gateway and, finally, decide whether you want to enable a **Dynamic Host Configuration Protocol** (DHCP) server on the LAN interface (y/n).

14. The settings will be applied, so hit *Enter* once done to complete the process, as illustrated in the following screenshot:

![](img/c8ffa9959721097701584fcc95101362_197_0.png)

Figure 6.47 – Exercise 2: Configuring pfSense – IP subnet mask

This concludes the installation of the pfSense VM. Next, we will look at some basic configuration of the firewall.

# Configuring pfSense

As the preceding screenshot in the previous section shows, the WebConfiguration address for the newly created pfSense firewall is https://172.25.100.1. We will be using this Uniform Resource Locator (URL) to configure the firewall.

The first time we log in to the WebConfigurator portal (the default credentials are admin and pfsense), we are presented with some initial configuration steps. The login screen is shown in the following screenshot:

![](img/c8ffa9959721097701584fcc95101362_197_1.png)

Figure 6.48 – Exercise 2: Configuring pfSense – web portal login

Once we are logged in, the initial configuration process starts automatically. Follow the next instructions to get through these initial configuration steps:

- 1. On Step 1, simply click **Next**.
- 2. On Step 2, specify a name for the server and enter primary and secondary DNS servers to use. I specified 1.1.1.1 as the primary server and 9.9.9.9 as the secondary one. Leave the Allow DNS servers to be overridden by DHCP/PPP on WAN box checked, and click Next.
- 3. On Step 3, select the **Coordinated Universal Time (UTC)** time zone from the dropdown (in order to have the correct timestamp generated with events that we will be sending to Security Onion, we need to set the time zone for the pfSense firewall to UTC, a format that is universally accepted as default by many network and security devices and that allows for convenient synchronization of event log timestamps across multiple systems and devices).
- 4. On Step 4, define the **wide area network (WAN)** interface setup (DHCP, in my case) and click **Next**.
- 5. On Step 5, define the LAN Interface (already done earlier) and click **Next**.
- 6. On Step 6, change the default admin password to something more secure than pfSense, then click **Next**.
- 7. On Step 7, click **Reload**.

Our pfSense firewall VM is now built and has an initial configuration, ready to start handling traffic. Let's next configure event log forwarding so that we can include data from our firewall in Security Onion searches and dashboards.

# Configuring log forwarding to syslog (Security Onion)

In order to start forwarding pfSense firewall logs to our Security Onion syslog server, we need to first allow (expose) the service to be contacted by the pfSense firewall. In order to do this, follow these next steps:

- 1. SSH into the Security Onion VM (`ssh adm-pac@172.25.100.250`) and run the `sudo so-allow` command.
- 2. Choose selection s for **Syslog Device**—port 514/tcp/udp.
- 3. Specify the LAN IP address of the pfSense firewall as the allowed IP address (172.25.100.1).
- 4. Confirm the configuration by hitting Enter. The process is illustrated in the following screenshot:

![](img/c8ffa9959721097701584fcc95101362_199_0.png)

Figure 6.49 – Exercise 2: Security Onion – allowing syslog access

That takes care of allowing access to the syslog server on our Security Onion VM. Next, we need to configure pfSense to start forwarding events to the external syslog server. To accomplish this, follow these instructions:

- 1. Navigate to the pfSense WebConfigurator portal at https://172.25.100.1 and log in to the portal.
- 2. Head on over to Status/ System Logs/ Settings, as illustrated in the following screenshot:

![](img/c8ffa9959721097701584fcc95101362_200_0.png)

Figure 6.50 – Exercise 2: Configuring pfSense – System Logs settings

- 3. Scroll to the bottom of this configuration screen and enable Send log messages to remote syslog server.
- 4. Fill out the configuration options as shown in the following screenshot, to send everything to the Security Onion syslog server at 172.25.100.250:514:

![](img/c8ffa9959721097701584fcc95101362_201_0.png)

Figure 6.51 – Exercise 2: Configuring pfSense – setting up syslog forwarding

- 5. Click Save to complete the configuration process.

That's it: we now have a functioning pfSense virtual firewall VM in our lab setup and have configured event forwarding to the Security Onion appliance we built in Exercise 1 – Setting up and configuring Security Onion. We will see the functionality this brings to the table, starting in Chapter 9, Visualizing, Correlating, and Alerting.

In the next exercise, we will install and configure an industrial IDS and asset management solution—namely, Forescout's SilentDefense.

# Exercise 3 – Setting up, configuring, and using Forescout's eyeInsight (formerly known as SilentDefense)

The final exercise for this chapter is about building and configuring a Forescout SilentDefense OT/ICS IDS and asset management appliance.

# Deploying the SilentDefense sensor and Command Center VMs

The Forescout SilentDefense sensor and Command Center appliances come as downloadable Open Virtual Appliance (OVA) template files and can be downloaded from the Forescout customer portal once you have purchased the product. OVA is a file format that supports the exchange of VMs across various virtualization products and platforms.

For this exercise, I will be using the following two OVA files:

- `sd-411-cc-16g-vmware.ova`: This is the 16 GB random-access memory (RAM) version of the Command Center.
- `sd-411-sensor-8g-vmware.ova`: This is the 8 GB RAM version of the sensor.

We will be installing the VMs on a vCenter cluster for two reasons, the first being that a vCenter cluster is typically the environment you would use for these types of appliances; secondly, I want to change things up a bit so that we get a better coverage of the tools, techniques, and technology typically found in the virtualization environment of the OT network. If you want to follow along with the exercise, VMware offers an evaluation version of vCenter that allows you to run the vCenter appliance for 60 days. Head on over to https://my.vmware.com/en/web/vmware/evalcenter?p=vsphere-eval-7 to find out more. Also, Chapter 19, Lab Setup, has instructions on how to deploy vCenter.

So, with the two OVA files downloaded and ready, follow along with these instructions to get our SilentDefense VMs deployed:

- 1. Navigate to the vCenter appliance portal (https://10.11.11.50) and click on LAUNCH VSPHERE CLIENT (HTML5), as illustrated in the following screenshot:

Figure 6.52 – Exercise 3 – vSphere client login

- 2. Right-click on the LAB-Datacenter folder and select Deploy OVF Template, as illustrated in the following screenshot:

Figure 6.53 – Exercise 3 – SilentDefense – deploying the sensor VM Open Virtualization Format (OVF) template
