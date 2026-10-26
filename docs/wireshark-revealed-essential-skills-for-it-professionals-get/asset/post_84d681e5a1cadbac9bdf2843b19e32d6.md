
## tcpdump

**tcpdump** is free Unix-based software that runs under the Unix/Linux command line.
Some of the useful resources for it are:

- The tcpdump website: [http://www.tcpdump.org/](http://www.tcpdump.org/)
- The Windows version of tcpdump (Windump): [http://www.wincap.org/windump/default.htm](http://www.wincap.org/windump/default.htm)
- A friendly tutorial: [http://danielmiessler.com/study/tcpdump/](http://danielmiessler.com/study/tcpdump/)
- The official man page: [http://www.tcpdump.org/tcpdump_man.html](http://www.tcpdump.org/tcpdump_man.html)

Wireshark can open tcpdump files, so when you capture packets with tcpdump, you can later open it with Wireshark or any other graphical tool.

## SNMP tools

The first sets of tools that I usually use to solve a problem are SNMP tools. There are tools with strong mapping capabilities, there are some with good statistical capabilities, and there are some with good logging and events capabilities.

First, in order to just monitor SNMP counters, you can use simple free MIB browsers and graphical tools such as:

| Vendor | Software name | Where to download | Notes | License |
|---|---|---|---|---|
| Manage engine | MibBrowser | http://www.manageengine.com/products/mibbrowser-free-tool/ | Very friendly with minimal configuration. | Free |
| Open source | MRTG | http://oss.oetiker.ch/mrtg/ | Requires time and knowledge to install and configure. Good for long-term statistics. Commonly used by ISPs as a console for their customers. | Free with up to 10 sensors (*1); Commercial from 11 sensors |
| SolarWinds | Network device monitor | http://www.solarwinds.com/products/freetools/network-device-monitor/ | Solarwinds is one of the leaders in network management tools, and along with the commercial stuff, you can find many free tools. | Free |
| SolarWinds Engineering toolset | Engineer's Toolset | http://www.solarwinds.com/engineers-toolset.aspx | Various tools for network monitoring, discovery, SNMP, configuration, basic scanners and more. | Free with limited capabilities; Commercial with full capabilities |

## SNMP platforms

SNMP platforms are pieces of software that provide a central console that shows a map of the network, collects information and presents statistical reports, and collects SNMP events and presents them by severity and other parameters.

Some of the common tools in this category are:

| Vendor | Software name | Where to download | Notes | License |
|---|---|---|---|---|
| Castlerock Computing | SNMPc | [http://www.castlerock.com/](http://www.castlerock.com/) | This is one of the friendliest SNMP tools that I have worked with for more than a decade. The SNMP management platform is very easy to use and is great for network debugging. | Commercial |
| SolarWinds | Assorted | [http://www.solarwinds.com/network-management-software.aspx](http://www.solarwinds.com/network-management-software.aspx) | SolarWinds has various tools that provide monitoring, mapping, configuration management and other network management capabilities. These are some of the best options available but are expensive. | Commercial |
| Manageengine | Assorted | http://www.manageengine.com/network-performance-management.html | Various tools that provide monitoring, mapping, configuration management and other network management capabilities. One of the best but expensive. | Commercial |
| HP | IMC, NNM, and so on | http://h17007.www1.hp.com/us/en/networking/solutions/network-management/index.aspx#.UkqgGT8YhyI | This is a great platform. HP made it much friendlier than previous Network Node Manager (NNM) software. It is definitely worth checking out. | Commercial |
| OpenNMS | OpenNMS | http://www.opennms.org/ | It is open source but requires know-how of how to configure it. | Free |
| Nagios | Nagios | http://www.nagios.org/ | It is open source but requires a knowledge to configure it. | Free |

There are many others tools, such as:

- The open source Cacti ([http://www.cacti.net/](http://www.cacti.net/))
- Zabbix ([http://www.zabbix.com/](http://www.zabbix.com/))
- MRTG ([http://oss.oetiker.ch/mrtg/](http://oss.oetiker.ch/mrtg/))
- Some others (some under the GNU public license).

There are the "heavyweight" suites, such as:

- The HP OpenView suite of management applications ([http://www8.hp.com/us/en/software-solutions/software.html?compURI=1174702#tab=TAB1](http://www8.hp.com/us/en/software-solutions/software.html?compURI=1174702#tab=TAB1))
- CA Unicenter ([http://www.ca.com/us/network-performance-management.aspx](http://www.ca.com/us/network-performance-management.aspx))

There are also other medium-sized platforms, various tools from Plixer ([http://www.plixer.com/](http://www.plixer.com/)), and many others.

For network monitoring and troubleshooting you will need the very basic tools, while as a platform you will need a more sophisticated one. You can find a nice comparison of management platform on [http://en.wikipedia.org/wiki/Comparison_of_network_monitoring_systems](http://en.wikipedia.org/wiki/Comparison_of_network_monitoring_systems).

## The NetFlow, JFlow, and SFlow analyzers

NetFlow from **Cisco** ([www.cisco.com/go/netflow](http://www.cisco.com/go/netflow)) and JFlow from **Juniper** ([http://www.juniper.net/techpubs/software/erx/junose82/swconfig-ip-services/html/ip-jflow-stats-config2.html](http://www.juniper.net/techpubs/software/erx/junose82/swconfig-ip-services/html/ip-jflow-stats-config2.html)) provide a method for collecting TCP/IP traffic flow statistics on your routing devices.

SFlow ([http://en.wikipedia.org/wiki/Sflow](http://en.wikipedia.org/wiki/Sflow) and [http://www.sflow.org/index.php](http://www.sflow.org/index.php)) is an industry standard technology for monitoring high-speed switched networks.

The differences between them are:

- **NetFlow** applies to Cisco routers and L3 switches. In layer-3 switches make sure that they support NetFlow (depends on software version and hardware). In some cases, you will need additional software/hardware for this. It was standardized by RFC3954 ([http://www.ietf.org/rfc/rfc3954.txt](http://www.ietf.org/rfc/rfc3954.txt)).
- **JFlow** applies to Juniper routers and L3 switches.
- **SFlow** is a standard for monitoring LAN switches and was standardized by RFC3176 ([http://tools.ietf.org/html/rfc3176](http://tools.ietf.org/html/rfc3176)).
- **PFIX** (RFCs 5101 and 5102) is a standard developed from **NetFlow v9**, and standardized by the **IETF**.

All Flow/IPFIX technologies are based on the communications device that collects the flow data from the interfaces and sends them to the management station. They require a simple configuration on the router or switch and software to collect the data and present it.

This software can be used for monitoring which users are causing a load on the network (displayed according to IP addresses or DNS names), on which applications (HTTP, SMTP, and so on, displayed according to their port numbers), web pages (displayed according to their IP addresses, translated to DNS names), and other such criteria. While Wireshark is usually used for this purpose in short-term monitoring (the **Conversations** feature), these tools can be used for long-term monitoring as well.

Some common software options include:

- [http://www.plixer.com/Scrutinizer-Netflow-Sflow/scrutinizer-flow-analyzer.html](http://www.plixer.com/Scrutinizer-Netflow-Sflow/scrutinizer-flow-analyzer.html) from **Plixer**
- [http://www.sevone.com/technologies/NetFlow-analysis](http://www.sevone.com/technologies/NetFlow-analysis) from **SevOne**

There are freeware tools, and there are commercial tools with free limited capabilities versions (usually limited by the number of interfaces they can monitor); in commercial SNMP platforms, you usually have a free license for two to five interfaces.
