| | Count | rule_name | event_module |
|---|---|---|---|
| | 11 | ET P2P BitTorrent DHT ping request | suricata |
| | 7 | ET INFO InetSim Response from External Source Possible SinkHole | suricata |
| | 3 | ET P2P BTWebClient UA uTorrent in use | suricata |
| | 3 | Windows Application error event | windows_eventlog |
| | 2 | ET INFO Observed DNS Query to .cloud TLD | suricata |
| | 1 | ET CINS Active Threat Intelligence Poor Reputation IP group 39 | suricata |
| | 1 | ET DNS Query for to TLD | suricata |
| | 1 | Ssymon - Suspicious Process - svchost.Exe | sysmon |
| | 1 | Three failed attempts to run sudo | ossec |

Figure 12.7 - Security Onion - Alerts summary – excluding low severity

We can see several interesting alerts. There is evidence that someone is connecting to peer-to-peer file sharing services (**ET P2P BitTorrent DHT ping request**); more specifically, **uTorrent** seems to be used (**ET P2P BTWebClient UA uTorrent in use**). Additionally, we see alerts for DNS queries to suspicious **Top-Level Domain** (TLD) URLs, and poor reputation IP address detection (**ET CINS Active Threat Intelligence Poor Reputation IP group 39**).

Let’s take a closer look at the uTorrent alert. uTorrent is a well-known BitTorrent application and by itself is not malicious (unless you manage to have a trojaned version). However, this type of traffic typically should not be seen on an industrial network. Click on the **ET P2P BTWebClient UA uTorrent in use** rule name and select **Drilldown**: 

![](bbox=[0.23, 0.52, 0.77, 0.92])