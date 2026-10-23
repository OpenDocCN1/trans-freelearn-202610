
```
[adm~pac0IND-SecurityOnionv2_zeek-otx]$ cat /nsm/zeek/spool/zeekas/intel.log | grep w0x.host
{"ts":"2020-12-27T18:06:45.697087Z","id":"C7Nrwkyu9EwFnz","indicator":"w0x.host","seen.when":172.25.100.220,"id.orig_h":"172.25.100.220","id.resp_p":53,"seen.indicator_type":"Intel::DOMAIN","seen.where":"DNS::IN_REQUEST","seen.node":"zeek","matched":["Intel::DOMAIN"],"sources":["AlienVault OTXv2 - Luhansk Ukraine Gov. Phishing Campaign ID: 5fb83d70906bd27194456779 Author: AlienVault"]}
{"ts":"2020-12-27T18:06:46.239010Z","uid":"C7NqV5JnYJNdg5oEEj","id.orig_h":"172.25.100.220","id.orig_p":1082,"id.resp_h":"172.25.100.100","id.resp_p":53,"seen.indicator":"w0x.host","seen.indicator_type":"Intel::DOMAIN","seen.where":"DNS::IN_REQUEST","seen.node":"zeek","matched":["Intel::DOMAIN"],"sources":["AlienVault OTXv2 - Luhansk Ukraine Gov. Phishing Campaign ID: 5fb83d70906bd27194456779 Author: AlienVault"]}
```

If you look over the output, you can see several pieces of information. First, there is the indicator that triggered the event, `seen.indicator: w0x.host`, as well as the type of indicator, `DNS::IN_REQUEST`. The log also shows the originator of the incident, `id.orig_h: 172.25.100.220`. Additionally, the log entry shows details about the indicator: `sources: AlienVault OTXv2 - Luhansk Ukraine Gov. Phishing Campaign ID: 5fb83d70906bd27194456779 Author: AlienVault`.

Optionally, you can see the intel logs via the Kibana web app. Open the Kibana dashboard by aiming your browser at `https://172.25.100.250`. Log in to the Security Onion portal and click on the **Kibana** tool icon on the left of the screen. From the Kibana home dashboard, navigate to **Home | Network | Intel**. This will display the intel dashboard and should show you the intel logs we just triggered:

![](img/c8ffa9959721097701584fcc95101362_331_0.png)

Figure 8.6 – Exercise 1 – Security Onion intel dashboard

## 9
Visualizing,
Correlating, and
Alerting

In this final chapter of the second part of the book, *Cybersecurity Monitoring*, we are going to combine all our activities from the previous chapters to create a holistic cybersecurity monitoring view.

We start off with a discussion around what a holistic view of cybersecurity monitoring entails. Then, we will add the finishing touches to our cybersecurity data collection toolset by incorporating custom visualization, correlation, and alerting capabilities. Visualization, correlation, and alerting is what makes the security monitoring solution we implement more usable and allows us to customize the solution to our particular needs and environment, enabling us to make sure no stone is left unturned.

Finally, we will finish the chapter—and Section 2, *Cybersecurity Monitoring*—with the creation of an interactive dashboard that adds visualization, correlation, and alerting capabilities to our toolbox. The dashboard is built around the immensely popular **ELK** (short for **Elasticsearch, Kibana, Logstash**) stack technology and will allow a single-page overview to everything cybersecurity-related in our environment, with the intention of creating a combined, one-stop overview display to discover malicious activity easily and conveniently.

- **SilentDefense System Logging Protocol (syslog) configuration:**
  - https://github.com/SackOfHacks/Industrial-Cybersecurity-2nd-Edition/blob/main/security-onion-logstash/syslog
- **Oinkcode:** https://snort.org/

## Holistic cybersecurity monitoring

**Holistic** is defined by the *Cambridge English Dictionary* as "*dealing with or treating the whole of something or someone and not just a part*". In the light of cybersecurity monitoring, this means we should leave no stone unturned, no log unchecked, and no system unmonitored. In this chapter, we will see that by combining a variety of security monitoring tools, each geared toward monitoring a certain aspect of the environment, we create a holistic view of the current state of the cybersecurity posture.

Typically, implementing a holistic cybersecurity monitoring program involves applying a combination of several solutions or appliances, distributed over the network or environment they should monitor. For the purpose of this book, we will use a single solution that encompasses an open source variety of each category of these security monitoring applications—namely, Security Onion. We will be expanding Security Onion's functionality to cover a variety of security monitoring aspects and extend its monitoring reach into the following areas of concern:

- We want to be able to monitor network traffic-related data and artifacts such as connection data, protocol usage, **Domain Name System (DNS)** queries, traffic analysis, and so on.
- We want to be able to monitor network traffic to look for known **indicators of compromise (IOCs)** or malicious behavior such as protocol misuse or attack patterns, allowing us to uncover intrusions.
- We want to be able to monitor the health and security state of our endpoints, including configuration changes, performance statistics, detection of malicious code or activities, and so on.

By combining these distinct disciplines of security monitoring, we effectively create holistic security monitoring coverage for our entire environment.

The next sections will go into detail on these main security monitoring disciplines.

## Network traffic monitoring

Network traffic monitoring allows us to index (meta)data about connections and connection attempts across a network. A network connection is the sharing of data between two endpoints on a computer network. The types of details we are interested in recording for a connection include the following:

- Endpoints involved in the connection
- **Internet Protocol (IP)** addresses and port numbers used in the connection
- Network protocol used for the connection (**Transmission Control Protocol (TCP)**, **User Datagram Protocol (UDP)**, and so on)
- The amount of data transferred
- The types of data transferred
- Anomalies within the connection
- And so on...

Even though network (meta)data doesn't allow us to directly discover malicious activities, having these types of details about connections that take place over our network allows us to pinpoint specifics about a suspicious connection or find out other systems involved with a malicious interaction we discovered by some other means (IDS, firewall, and so on).

### Zeek

Discussed in detail in *Chapter 6, Passive Security Monitoring*, Zeek is the de facto network monitoring tool that allows us to capture all the requirements stated in the preceding section, and more. Zeek keeps tabs on specifics about network connections, as well as the data and protocols transmitted within those connections. Besides supporting the major network layer protocols (TCP, UDP, **Internet Control Message Protocol (ICMP)**, and so on), Zeek also supports many of the application layer protocols, some of which include the following:

- DNS
- **Dynamic Host Configuration Protocol (DHCP)**
- **File Transfer Protocol (FTP)**
- **Remote Procedure Call (RPC)**
- **HyperText Transfer Protocol (HTTP)**
- **Internet Relay Chat (IRC)**
- Kerberos
- **Network Time Protocol (NTP)**
- **Remote Desktop Protocol (RDP)**
- **Session Initiation Protocol (SIP)**
- **Secure Shell (SSH)**
- Syslog

Here are some common industrial network protocols Zeek supports:

- Modbus
- **Distributed Network Protocol 3 (DNP3)**

Zeek can also extract (carve out) files discovered within network connections, giving us the capability to perform forensic checks on those files to determine if they are malicious or harmful. Furthermore, Zeek will look for anomalies within protocol use, perform diagnostics on connections, and observe and record other specifics about network connections.

We installed Zeek in *Chapter 6, Passive Security Monitoring*, as part of the Security Onion **virtual machine (VM)**, and will see the tool used extensively in *Section 3* of this book: *Threat Hunting*.

## Network intrusion monitoring

Network intrusion monitoring allows us to find malicious content or activities traversing a network. This detection can be based on known artifacts (signature-based detection) or on anomaly detection (where an anomaly is different from an established normal state—a baseline). Network intrusion monitoring is typically done at the edge of a network as part of the functionality of a firewall (**deep packet inspection (DPI)** capabilities) or is done on an internal network, with the use of a **network-based IDS (NIDS)** or a **network-based intrusion-prevention system (NIPS)**.

## NIDS on the edge

As we discussed in *Chapter 6, Passive Security Monitoring*, DPI allows a security solution or appliance implementing the technology to look deeper into the packet, to inspect the upper network layers. By being able to inspect these higher layers of the **Open Systems Interconnection (OSI)** model, we gain the ability to check protocol abuse and exfiltration of data out of—or transfer of malicious content into—the networks we are monitoring.

We will be applying an edge NIDS in *Exercise 3 – Adding a Snort IDS to pfSense* in this chapter.
