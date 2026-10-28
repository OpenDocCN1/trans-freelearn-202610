
Let's start with the simplest one — the One-time downtime. When adding a time period like that, we may enter a short description in the Note field, and we choose From-Till dates and times:

![](img/f5313a804532fabacc8d07cc8607eeba_564_0.png)

The note is not used for much, though — it is only displayed in the list of configured times, as shown in the preceding screenshot.

The Downtime option allows us to define times that will be excluded from the SLA calculation:

![](img/f5313a804532fabacc8d07cc8607eeba_564_1.png)

This is done on a weekly basis, where we may choose the weekday and time with minute precision. Unfortunately, here is the only place in Zabbix where a week sort of starts with Sunday. The biggest issue is that these periods cannot cross the week border, thus it is actually impossible to add SLA calculation downtime for the weekend in one go — we would have to add one entry for Saturday and one for Sunday.

But what about the Uptime option? That one works in the reverse way. If an uptime entry is added, SLA calculation only happens during that time period; all other time is considered to be "downtime".

Of course, when adding time periods here, one should obey the clauses from the actual agreement, not use this to hide problems from the SLA calculation, right?

## Summary

In this chapter, we departed a bit from the low-level monitoring of CPU, disks, and memory. We discussed a higher level of monitoring, one that looked at business services, called "IT services" in Zabbix. We were able to configure a service tree to represent real life dependencies and structure, link individual entries against triggers to propagate problem states to services, and configure SLA calculation for those services. We did not have a large IT system to test against, so we sent in fake data and observed the resulting reports, including a service availability report and yearly SLA graph.

We noted two important facts about IT service functionality in Zabbix:

- Triggers with severity of "Not classified" or "Information" are ignored when calculating the SLA
- SLA information cannot be calculated at a later time — the IT services must be configured in advance

For those cases when a service does not have full-time SLA coverage, we learned about a way to specify when the SLA calculation should take place based on weekly time periods — but we also noted that host and host group-level maintenance does not affect the SLA calculation and the uptime/downtime configuration has to be done for the IT services themselves.

In the next chapter, we'll go back to lower-level monitoring — even lower than before. We will cover monitoring hardware directly using the Intelligent Platform Management Interface (IPMI). Zabbix supports monitoring both "normal" or analog IPMI sensors and discrete IPMI sensors. There is even a special trigger function for discrete sensors. What is it? See the next chapter for details.

## Monitoring IPMI Devices

By now, we are familiar with monitoring using Zabbix agents, SNMP, and several other methods. While SNMP is very popular and available on the majority of network-attached devices, there's another protocol that is aimed at system management and monitoring: **Intelligent Platform Management Interface (IPMI)**. IPMI is usually implemented as a separate management and monitoring module independent of the host operating system that can also provide information when the machine is powered down. IPMI is becoming more and more popular, and Zabbix has direct IPMI support. IPMI is especially popular on so-called **lights-out** or **out-of-band** management cards, available for most server hardware today. As such, it might be desirable to monitor hardware status directly from these cards, as that does not depend on the operating system type or even whether it's running at all.

### Getting an IPMI device

For this section, you will need an IPMI-enabled device, usually a server with a remote management card. The examples here will use real hardware that could have vendor-specific quirks, but it should be possible to apply the general principles to any product from any vendor.

### Preparing for IPMI monitoring

To gather data using IPMI, Zabbix must be configured accordingly, and the device must accept connections from Zabbix. If you installed Zabbix from packages, IPMI support should be available. If you compiled Zabbix Server from source, OpenIPMI library support should be included as well. To be sure, check the startup messages in the server log file. Make sure the line about IPMI says YES:

```
IPMI monitoring:              YES
```

## Monitoring IPMI Devices

That is not enough yet—by default, Zabbix Server is configured to not start any IPMI pollers; thus, any added IPMI items won't work. To change this, open `zabbix_server.conf` and look for the following line:

```
# StartIPMIPollers=0
```

Uncomment it and set the poller count to 3, so that it reads as this:

```
StartIPMIPollers=3
```

Save the file and restart `zabbix_server`.

On the monitored device side, add a user that Zabbix would use. The IPMI standard specifies various privilege levels, and for monitoring, the user level might be the most appropriate. The configuration of IPMI users could be done using the vendor-supplied command line tools, web interface, or some other method. Consult the vendor-specific documentation for the details on this step.

## Setting up IPMI items

Before we can add IPMI items to Zabbix, we should test IPMI access. By default, IPMI uses UDP port 623, so make sure it is not blocked by a firewall. Check whether your Zabbix server has the `ipmitool` package installed—if not, install it, and then execute the following:

```
$ ipmitool -U zabbix -H <IP address of the IPMI host> -I lanplus -L user sdr
Password:
```

Provide the password that you have set in the IPMI configuration. We are using user-level access, as specified by the `-L user` flag, so that administrative privileges should not be required for the Zabbix IPMI user. The `-I lanplus` flag instructs `ipmitool` to use the IPMI v2.0 LAN interface, and the sensor command queries the host for the available sensors. If your device has IPMI running on a non-default port, you can specify the port with the `-p` flag.

> Zabbix does not use `ipmitool` to query IPMI devices; it uses the OpenIPMI library instead. This library historically has had a few bugs, and a working `ipmitool` instance does not guarantee that IPMI monitoring will work with Zabbix Server. When in doubt, test with the latest version of OpenIPMI.

The output will contain a bunch of sensors, possibly including some such as these:

```
BB +5.0V      | 4.97 Volts   | ok
Baseboard Temp | 23 degrees C | ok
System Fan 2    | 3267 RPM     | ok
Power Unit Stat | 0x00        | ok
```

That looks like useful data, so let's try to monitor fan RPM in Zabbix. In the frontend, navigate to Configuration | Hosts. To keep things organized, let's create a new host for our IPMI monitoring—click on Create host. Then, enter the following values:

- **Name:** IPMI host
- **Groups:** Click on Linux servers in the Other groups box, then click on the ◄ button, and make sure no other groups are in the In groups listbox
- **IPMI interfaces:** Click on the Add control and enter the IPMI address, and then click on Remove next to Agent interfaces

> Some IPMI solutions work on the primary network interface, intercepting IPMI requests. In such a case, simply set the same IP address to be used for IPMI.

Switch to the IPMI tab, and enter the following values:

- **IPMI username:** Enter the username used for IPMI access
- **IPMI password:** Enter the password you have set for IPMI access

> If you have set a long IPMI password and revisit the host editing screen, you'll see it being trimmed. This is normal, as the maximum password length for IPMI v2.0 is 20 characters.

If you have a different configuration for IPMI, such as a different privilege level, port, or other parameters, set them appropriately. When done, click on the Add button at the bottom.

> For this host, we reused the Linux servers group—feel free to add it in a separate group.
