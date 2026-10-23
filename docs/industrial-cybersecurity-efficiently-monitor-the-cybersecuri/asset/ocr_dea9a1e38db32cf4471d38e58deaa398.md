Before we run into Company Z's production plant, Universal Serial Bus (USB) keys blazing, let's first see if we can get a foothold without breaking in, using the virtual realm of the internet. We will be assessing the cyber presence of Company Z by identifying the internet IP addresses they own (public IP address range or classless inter-domain routing (CIDR)). We can use that list to assess if there are any systems responding on them, maybe even misconfigured, to allow us in.

To find the public address range a company uses, we use a WHOIS lookup on the company website's IP address. To find the IP address of Company Z's website, we first perform a nslookup operation on companyZ.com, as follows:

```
nslookup companyZ.com
Server:        1.1.1.1
Address:       1.1.1.1#53

Non-authoritative answer:
Name:  companyz.com
Address: 10.0.0.120
```

Armed with the public IP address for their website, we do a WHOIS lookup to find out details about the company's public filing, as follows:

```
$ whois 10.0.0.120
#
# ARIN WHOIS data and services are subject to the Terms of Use
# available at: https://www.arin.net/resources/registry/whois/tou/
#
# If you see inaccuracies in the results, please report at
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
#
# Copyright 1997-2021, American Registry for Internet Numbers, Ltd.
#
NetRange:      10.0.0.0 - 10.0.0.255
CIDR:          10.0.0.0/24
NetName:       CompZ
NetHandle:     NET-10-0-0-0-1
```