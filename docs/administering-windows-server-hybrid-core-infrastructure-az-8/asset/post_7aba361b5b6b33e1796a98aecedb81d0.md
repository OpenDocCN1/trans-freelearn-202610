
You can read more on reverse lookup at the following URL:

https://en.wikipedia.org/wiki/Reverse_DNS_lookup

## Implementing DNS

The DNS service is installed as a role on Windows Server using Server Manager, Windows Admin Center, or PowerShell with the following command:

```
Install-WindowsFeature -Name DNS -IncludeManagementTools
```

The installation via Windows Server Server Manager is represented in the following screenshot:

![](img/cf35f46a8dd5f11e58349c1d81611b75_165_0.png)

Figure 5.5 – Adding a DNS role

Once the DNS server role is installed, it can be managed from Windows Admin Center or the DNS Manager console. The DNS Manager console is represented in the following screenshot:

![](img/cf35f46a8dd5f11e58349c1d81611b75_165_1.png)

Figure 5.6 – DNS Manager console

The DNS role is commonly installed on AD DS domain controllers and, instead of zone files, can use AD-integrated DNS to store the DNS information in the AD database. This means the DNS information is made secure and highly available as it is replicated by native AD replication and availability mechanisms.

The following sections look at the topics of zones, records, forwarders, and security.

## Zone

When a zone file is created, it must be set as a primary or secondary zone. The primary zone is used to create, edit, and delete records. The primary zone records can be stored in a zone file or configured to be AD-integrated, which means the AD DS database is used to store the zone information. The secondary zone is a read-only copy of the primary zone and is always stored in a zone file. PowerShell can also be used to manage the DNS zones.

Zone records can be transferred between primary and secondary zones with a zone transfer. The secondary zone servers requesting a zone transfer can be controlled by setting the following options:

- To any server
- Only to servers listed on the Name Servers tab
- Only to the following servers

These zone transfer options are represented in the following screenshot:

![](img/cf35f46a8dd5f11e58349c1d81611b75_166_0.png)

Figure 5.7 – DNS zone transfer options

Incremental zone transfers happen after the initial secondary zone synchronization from the primary zone is complete.

## Records

Records can be manually managed using the DNS Manager console, Windows Admin Center, or PowerShell cmdlets.

The zone files are represented in the following screenshot:

![](img/cf35f46a8dd5f11e58349c1d81611b75_167_0.png)

Figure 5.8 – DNS zone files

All records have a **time-to-live (TTL)** value configured. This defines how long the information in the record is valid and how long it is allowed to be cached by clients and servers. If a TTL is set for 36000 secs (1 hour), then the response to the DNS query can only be cached for that period.

During that time, the client will only use the information in the cache and not make a new request. When the TTL expires, the client will make a new request to the DNS server and may receive any updates made to the record since it was requested and cached. The cache should always be cleared for troubleshooting.

Host and PTR records can also be created dynamically, making managing DNS easier. The current IP address for a device is automatically registered after its IP is changed when dynamic DNS is enabled.

## Forwarders

DNS servers can be configured with *forwarders*, typically for internet name resolution.

Where the DNS server is requested to resolve a name that is not authoritative for that zone—that is, it does not hold a primary or secondary zone for that domain—it must ask another DNS server for this information. The default is that *root hints* will be configured on each server to find the authoritative servers on the internet; however, that is only possible if the DNS server has access to the internet and the record requested is published on the internet.

Due to network segmentation and security, the DNS servers will not always be directly connected to the internet. They will have to proxy the request through forwarders if the records are not stored locally in a cache.

The forwarders are DNS servers connected to the internet, usually in a perimeter network segment or an **external service provider (ESP)** DNS server. These DNS servers can resolve the internet names for these requests outside the local network.

**Conditional forwarding** is another option and is intended to only forward any request lookups for a *single specific DNS domain* instead of forwarding all requests that cannot be resolved locally.

## Security

DNS is secured by validating all responses from a DNS server through the extensions suite of **Domain Name System Security Extensions (DNSSEC)**. This aims to make the DNS protocol less prone to attacks such as *DNS spoofing* by providing authenticated **denial of service (DoS)**, data integrity, and origin authority.

The DNS zone is secured using a **zone signing** process when supported by an authoritative DNS server; this does not change the DNS query and response mechanism but adds a validation step. Digital certificates are used for validation in the DNS responses. The process generates new DNSSEC resource records containing digital certificates during zone signing.

When a query for a *DNSSEC-signed zone* is received by a DNSSEC-aware forwarding or recursive DNS server, it will request that the authoritative DNS server also responds with the DNSSEC records. It will then use the digital certificate contained in the DNSSEC records to validate the response. The DNS server recognizes a zone that supports DNSSEC if the zone has a **DNSKEY** record (trust anchor).

## Troubleshooting DNS

This section will look at some commands that can troubleshoot DNS.

*An adage says that 90% of the time, it's DNS, and so is the other 10%.*

Here are some examples of troubleshooting commands:

- NSLOOKUP: Queries for DNS information and looks up resource records and the DNS server status
- DNSCMD: Manages the DNS server role
- DNSlint: Diagnoses common DNS issues
- IPConfig: Lists the IP configuration details used by a DNS client

This section concludes the topic of name resolution using the DNS network service. We looked at DNS, how it works, and how to implement, manage, and secure it. In the next section, we will look at DHCP and its use to manage leasing IP addresses to clients.

## Implementing and managing DHCP

This section will look at managing on-premises IP addresses using the DHCP network service.

The first skills area we will look at in this section is what is DHCP?

### What is DHCP?

DHCP can be considered a network service component of a DNS-DHCP-IPAM (DDI) solution; we also cover DNS and IPAM in this chapter.

Concisely, DHCP is a network management protocol used to simplify IP address management through auto-assigning IP addresses and network information.

DHCP is an extended version of Bootstrap Protocol (BOOTP). The difference between the two is that BOOTP provides static configuration of IP addresses, and DHCP provides automatic configuration of IP addresses.

Without DHCP, there would be a need for manual network IP address configuration on the network interface for the required network settings and manual assignment of IP addresses; this is highly inefficient and error-prone.

With regards to DHCP version 6, this provides IPAM in IPv6 networks. Stateful and stateless configurations are supported.

The DHCP server assigns the IPv6 addresses and network settings in stateful configurations. In stateless configurations, the router assigns the IP addresses, and the DHCP server assigns only the network settings.

### How does DHCP work?

DHCP works on the traditional client/server relationship model. Windows computers run the DHCP client service, and when set to Obtain an IP address automatically, they will communicate with a DHCP server to receive their network information. This setting is found in the Internet Protocol Version 4 (TCP/IPv4) Properties dialog, as shown in the following screenshot:
