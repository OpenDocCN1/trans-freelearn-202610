# Organization information

Seed Organization names for asset discovery ①

msandbu
mariussandbu

Example: Microsoft | One per line.

# Seeds

Domains (2)

Seed Domains for asset discovery ①

msandbu.org
msandbu.net

Example: office.com | One per line.

Domains to exclude from asset discovery ①



Example: office.com | One per line.

IP Blocks (2)

Seed IP Blocks for asset discovery ①

81.166.4.0/16
81.167.4.0/16

Example: 20.64.0.0/10 | One per line.

IP Blocks to exclude from asset discovery ①



Example: 20.64.0.0/10 | One per line.

![](bbox=[0.48, 0.1, 0.85, 0.4])

Figure 9.6 – Defining custom assets for discovery

Once you have added the information, click Start Search.

We also have the option to exclude assets, which might be useful if we only use a certain part of an IP block or if we share a service with others, which might clutter the search results. Secondly, this service is billed per asset per day, so we should try and scope the search parameters as accurately as possible.

Once you have entered the search parameters, click Next and then click Confirm. Once we are done with the wizard, the service will start crawling the assets that were defined. It can take some time before the results are available, which can range from 24-48 hours.

Once the search data is available, it will be automatically populated into the main dashboard, and you have different dashboards on the left that can be used to view the attack surface and security posture.

If you click on Attack Surface, it will list whether any vulnerabilities have been found, along with other low-security observations such as deprecated technologies or SSL expiration. For instance, if we have medium-severity vulnerabilities, such as the ones seen in the following screenshot, the service is able to map known vulnerabilities against an external service, which, in this case, was a Citrix NetScaler appliance: