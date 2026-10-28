
## Automating Configuration

For now, let's leave the conditions empty and switch to the **Operations** tab. Adding a new operation and expanding the **Operation type** dropdown will reveal all the available operations. We will discuss them in more detail a bit later, but for now, let's choose **Add to host groups**. In the input field, start typing `linux`, and choose **Linux servers** from the dropdown. Then, click on the small Add control in the **Operation details** block. Be very careful here, as it is easy to lose some configuration. When done, click on the **Add** button at the bottom:

![](img/f5313a804532fabacc8d07cc8607eeba_498_0.png)

After a couple of minutes, go to **Configuration | Hosts** to observe the results. If discovering our test systems, we should see one new host added.

> Even though we did not tell the action to add the host itself, it still happened. If the operation implies that there's a host—for example, adding it to a host group or linking to a template—the host will be automatically added.

Why only one host? The other host already existed as per **Monitoring | Discovery** earlier. For this host, you will see either its hostname or the IP address used as the hostname in Zabbix. If the Zabbix server was able to perform a reverse lookup on the IP address, the result will be used as the hostname. If not, the IP address will be used as the hostname.

> If multiple addresses reverse-resolved to the same name, others would be added as name_2 and so on.

Click on **New host** in the NAME column. In the **Groups** section, this host is in the **Linux servers** group, as expected. But it is also in some other group, **Discovered hosts**. Where did that come from?

![](img/f5313a804532fabacc8d07cc8607eeba_498_1.png)

## Automating Configuration

- **Enable host:** A host will be enabled. If there is no such host, one will be added first.
- **Disable host:** A host will be disabled. This could be used as a safer alternative to removing hosts, or we could disable a host first and remove it later. If there is no such host, one will be added first.

When linking to a template, the host still needs all the proper interfaces as required by the items in that template. During discovery, only successful discovery checks result in the adding of interfaces of a corresponding type. For example, if we only found SNMP on a host, only an SNMP interface would be added. If both SNMP and Zabbix agent discovery checks succeeded on a host, both interfaces would be added. If some checks succeed later, additional interfaces are created.

### Uniqueness criteria

But what about multi-homed hosts that have multiple interfaces exposed to Zabbix network discovery? Let's return to **Configuration | Discovery** and click on **A test discovery**. Look at the **Device uniqueness criteria** option—the only setting there is **IP address**. In the **Checks** block, click on **New** and choose **Zabbix agent** in the **Check type** dropdown. In the **Key** field, enter `system.uname`, and then click on **Add** in the **Checks** block. Notice how the **Device uniqueness criteria** got a new option—**Zabbix agent "system.uname"**:

![](img/f5313a804532fabacc8d07cc8607eeba_500_0.png)

By default, with the uniqueness criteria set to **IP address**, Zabbix will create a new host for each discovered IP address. If there's a system with multiple addresses, a new host will be created for each address. If the uniqueness criteria is set to a Zabbix agent item, it will look at all the IP addresses it has seen before and the values it got back for that item key. If the new value matches some previous value, it will add a new interface to the existing host instead of creating a new host. It works the same way with SNMP—adding an SNMP check will add another uniqueness criteria option, and Zabbix will compare values received for that specific OID. It is common to discover SNMP devices by the SNMPv2-MIB::sysDescr.0 OID.

> Both a Zabbix agent and SNMP must be preconfigured to accept connections from the Zabbix server.

Now that we have discussed network discovery, I'll give you one short suggestion about it—don't use it. Well, maybe not that harsh, but do not cling to it too much. There are use cases for network discovery, but quite often, there's a decent list of devices that should be monitored coming either from a **configuration management database (CMDB)** or some other source. In that case, it is better to integrate and automatically update your Zabbix configuration based on that authoritative source. If your answer to "What's your most definitive list of hosts in your environment?" is "Zabbix", then network discovery is for you.

### Active agent autoregistration

We just explored network discovery—it scanned a network range. Zabbix also supports a feature that goes the other way around, where Zabbix agents can chime in and Zabbix server can automatically start monitoring them. This is called active agent autoregistration.

Whenever a Zabbix agent connects to the Zabbix server, the server compares the incoming agent hostname with the existing hosts. If a host with the same name exists, it proceeds with the normal active item monitoring sequence. This includes both enabled and disabled hosts. If the host does not exist, the autoregistration sequence kicks in, that is, an event is generated.

The fact that an event is generated every time an unknown agent connects to the Zabbix server is important. If you do not use active items or autoregistration, switch off active checks on the agent side. Otherwise, each such check results in a network connection, log entry on the agent and server side, and an event in the Zabbix database. There have been cases where that increases the database size and results in significantly reduced performance. In some instances, there are millions of such completely useless autoregistration events, up to 90% of the total event count. It is suggested to check the server log for entries such as this:

```
cannot send list of active checks to [127.0.0.1]: host [Register me]
not found
```

If found, they should all be solved. The first pair of square brackets tells us where the connection came from, and second, what host the agent claimed to be.

Similar to trigger and network-discovery events, we may react to that event with an action. Let's configure an autoregistration action now — head to **Configuration | Actions** and switch to **Auto registration** in the **Event source** dropdown. Then, click on **Create action**. Enter `Testing registration` in the **Name** field, and then switch to the **Operations** tab. Click on **New** in the **Action operations** block. The **Operation type** dropdown reveals a subset of operations that are available for network discovery. Most notably, we cannot remove hosts, remove hosts from host groups, and unlink hosts from templates. The operations are functionally the same as for network discovery, so we won't look into them much — just choose **Add host** this time, and click on the small **Add** control in the **Operation details** block. Then, click on the **Add** button at the bottom. With the action in place, probably the easiest way to test this is to fake a new agent. Edit the agent daemon configuration file on A test host and change the **Hostname** parameter to `Register me`. Then, restart the agent daemon. Go to **Configuration | Hosts** — there's a new host again. If you check the host properties, it is included in the **Discovered hosts** group — the same group is used here as in the network discovery. Let's change the **Hostname** parameter back to the previous value in the agent daemon configuration file and restart the agent.

We haven't looked at the conditions for autoregistration yet — let's return to **Configuration | Actions**, click on **Testing registration**, and switch to the **Conditions** tab. The dropdown next to the New condition section reveals the available conditions:

![](img/f5313a804532fabacc8d07cc8607eeba_502_0.png)

As we can see, the list of available conditions is much shorter here. We can filter by hostname — for example, if all our **Linux hosts** have 'linux' in the name, we could detect them that way. We can also filter by proxy if we use Zabbix proxies for the autoregistration. There's also an entry called **Host metadata** — what's that?
