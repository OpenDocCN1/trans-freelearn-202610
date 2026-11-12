The forwarders are DNS servers connected to the internet, usually in a perimeter network segment or an **external service provider (ESP)** DNS server. These DNS servers can resolve the internet names for these requests outside the local network.

**Conditional forwarding** is another option and is intended to only forward any request lookups for a *single specific DNS domain* instead of forwarding all requests that cannot be resolved locally.

# Security

DNS is secured by validating all responses from a DNS server through the extensions suite of **Domain Name System Security Extensions (DNSSEC)**. This aims to make the DNS protocol less prone to attacks such as *DNS spoofing* by providing authenticated **denial of service (DoS)**, data integrity, and origin authority.

The DNS zone is secured using a **zone signing** process when supported by an authoritative DNS server; this does not change the DNS query and response mechanism but adds a validation step. Digital certificates are used for validation in the DNS responses. The process generates new DNSSEC resource records containing digital certificates during zone signing.

When a query for a *DNSSEC-signed zone* is received by a DNSSEC-aware forwarding or recursive DNS server, it will request that the authoritative DNS server also responds with the DNSSEC records. It will then use the digital certificate contained in the DNSSEC records to validate the response. The DNS server recognizes a zone that supports DNSSEC if the zone has a **DNSKEY** record (trust anchor).

# Troubleshooting DNS

This section will look at some commands that can troubleshoot DNS.

*An adage says that 90% of the time, it's DNS, and so is the other 10%.*

Here are some examples of troubleshooting commands:

+   - NSLOOKUP: Queries for DNS information and looks up resource records and the DNS server status
+   - DNSCMD: Manages the DNS server role
+   - DNSlint: Diagnoses common DNS issues
+   - IPConfig: Lists the IP configuration details used by a DNS client

This section concludes the topic of name resolution using the DNS network service. We looked at DNS, how it works, and how to implement, manage, and secure it. In the next section, we will look at DHCP and its use to manage leasing IP addresses to clients.