# Linking templates to hosts

Now we'd like to link this template to our very first host, "A test host". First, let's compare item lists between the freshly created template and that host. Open Configuration | Hosts in one browser window or tab and Configuration | Templates in another. In the first, choose Linux servers in the Group dropdown, then click on Items next to A test host. In the other one, select Custom templates in the Group dropdown, then click on Items next to C_Template_Linux. Place the windows next to each other and compare the listings:

| NAME | TRIGGERS | KEY | NAME | TRIGGERS | KEY |
|---|---|---|---|---|---|
| CPU load | Triggers 1 | system.cpu.load | CPU load |  | system.cpu.load |
| Full OS name |  | system.uname | Experimental SNMP trap |  | netSnmpExperimental2 |
| ICMP ping performance |  | icmppingsec | ICMP ping performance |  | icmppingsec |
| Incoming traffic on interface lo |  | net.if.in[lo] | Incoming traffic on interface enp0s8 |  | net.if.in[enp0s8] |
| Incoming traffic on interface enp0s8 |  | net.if.in[enp0s8] | Incoming traffic on interface lo |  | net.if.in[lo] |
| SMTP server status | Triggers 1 | net.tcp.service[smtp] | Local time | Triggers 1 | system.localtime |
| SNMP trap fallback | Triggers 1 | snmptrap.fallback | SMTP server status | Triggers 1 | net.tcp.service[smtp] |
| SNMP trap tests |  | snmptrap[test] | SNMP trap fallback |  | snmptrap.fallback |
| SSH server status | Triggers 1 | net.tcp.service[ssh] | snmptraps | Triggers 1 | snmptraps2 |
| Web server status | Triggers 1 | net.tcp.service[http,,80] | SNMP trap tests |  | snmptrap[test] |
| Zabbix agent version |  | agent.version | SSH server status |  | net.tcp.service[ssh] |
|  |  |  | Testfile exists | Triggers 1 | vfs.file.exists[/tmp/testfile] |
|  |  |  | Web server status | Triggers 1 | net.tcp.service[http,,80] |
|  |  |  | Zabbix agent version |  | agent.version |

We can see here that the template has three more items than the host. Looking at the lists, we can see that the items available on the template but not the host, are both SNMP related items that we added later — Experimental SNMP trap and snmptraps, the time item Local time, and also the check for a file, Testfile exists. If the template has four items the host is missing, but in total it only has three items more, that means the host should have one item that the template doesn’t — that’s right, the Full OS name exists for the host but is missing in the template. Keeping that in mind, and return to Configuration | Hosts.