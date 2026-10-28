The output will contain a bunch of sensors, possibly including some such as these:

```
BB +5.0V      | 4.97 Volts   | ok
Baseboard Temp | 23 degrees C | ok
System Fan 2    | 3267 RPM     | ok
Power Unit Stat | 0x00        | ok
```

That looks like useful data, so let's try to monitor fan RPM in Zabbix. In the frontend, navigate to Configuration | Hosts. To keep things organized, let's create a new host for our IPMI monitoring—click on Create host. Then, enter the following values:

+   - **Name:** IPMI host
+   - **Groups:** Click on Linux servers in the Other groups box, then click on the ◄ button, and make sure no other groups are in the In groups listbox
+   - **IPMI interfaces:** Click on the Add control and enter the IPMI address, and then click on Remove next to Agent interfaces

> Some IPMI solutions work on the primary network interface, intercepting IPMI requests. In such a case, simply set the same IP address to be used for IPMI.

Switch to the IPMI tab, and enter the following values:

+   - **IPMI username:** Enter the username used for IPMI access
+   - **IPMI password:** Enter the password you have set for IPMI access

> If you have set a long IPMI password and revisit the host editing screen, you'll see it being trimmed. This is normal, as the maximum password length for IPMI v2.0 is 20 characters.

If you have a different configuration for IPMI, such as a different privilege level, port, or other parameters, set them appropriately. When done, click on the Add button at the bottom.

> For this host, we reused the Linux servers group—feel free to add it in a separate group.