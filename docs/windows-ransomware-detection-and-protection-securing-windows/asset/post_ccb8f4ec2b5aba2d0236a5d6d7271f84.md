
Killnet's attack patterns were split between Layer 4 and Layer 7 depending on where the target website was. If the website was hosted in the public cloud, then Killnet would use Layer-7-based attacks, but for targets that were not in the public cloud, they would use Layer-4-based attacks.

One of the reasons why they avoided using Layer-4-based attacks on a target that used a public cloud such as AWS is because AWS has DDoS protection for Layer-4 attacks enabled by default. Large providers such as AWS have built-in DDoS protection mechanisms that protect incoming traffic at the edge so that the traffic never reaches the actual customer service.

These attacks were often launched via a large fleet of bots, also known as zombies or agents, which are often compromised computers or devices that are controlled remotely by an attacker. The bots are controlled by a master server, which is a centralized server that coordinates the actions of all the bots in the botnet. The master server sends commands to the bots, instructing them to initiate an attack on a specific target. This allows the attacker to launch a large-scale DDoS attack with minimal resources, as the attack is distributed across multiple bots. The use of bots and a master server can make it difficult to detect and mitigate a DDoS attack, as the source of the attack is distributed across multiple compromised devices and controlled centrally.

Azure and Google Cloud have similar mechanisms and features but they are not enabled by default. In addition, we have options such as Cloudflare and Akamai, which also use DDoS protection mechanisms.

For smaller companies, I recommend looking at Cloudflare since they have a cheaper SKU and it is a simple implementation for protecting against Layer-4-based DDoS attacks. Regardless of whether you choose Cloudflare or another provider, in most cases, you do not need to have the actual website hosted there. With Cloudflare, you just set up a new public address and point the DNS record to the new Cloudflare IP, and Cloudflare will act as a reverse proxy and connect to the backend, which is your web server, either directly or using a feature called Argo Smart Routing.

When it comes to Layer-7 attacks, as mentioned earlier, they are more difficult to distinguish from regular traffic because each bot in a botnet makes seemingly legitimate network requests suggesting that the traffic is not spoofed and may appear normal in origin.

Sometimes, these attacks go directly to the main URL of the site using HTTP GETs, and sometimes they use HTTP POST since that requires web servers to handle the process and consumes more of their resources.

Now, there are some ways to block these types of attacks, which I'll mention next, but they require that you have some form of Layer-7 firewall in front of your services, such as Microsoft Azure Application Gateway with Web Application Firewall (WAF), CloudFront from AWS, or Cloudflare:

- **User agent blocking**: An automated attack, in most cases, will be triggered using a script on the botnet and therefore will not have a regular HTTP user agent, as seen in the following screenshot. Therefore, having a WAF that can block attacks based on unknown user agents can be a good start for blocking incoming attacks or web scraping. We can see this from traffic logs against my website where a Python-based script user agent is scraping my website for information:

  | Field | Value |
  | --- | --- |
  | HTTP Version | HTTP/1.1 |
  | Host | msandbu.org |
  | Path | /azure-vm-guest-agent/ |
  | Query string | Empty query string |
  | User agent | python-requests/2.27.1 |

  Figure 6.10 – User agent activity log

- **Geo-blocking**: Most WAF providers also can block IP ranges based on the country of origin, which should be done either way if your business only operates within a specific country. You should review your firewall log and block countries when there is no logical reason why they should visit your site. However, when you are dealing with a DDoS attack, in most cases, the botnet spans many different countries, making it difficult to maintain and not lock out countries where your legitimate users are.

- **WAF signatures**: Most WAF vendors block based on built-in signatures or custom rules. The built-in signatures are there to block against known attack patterns such as OWASP TOP 10. You can, for instance, read about the signature files that Microsoft have here for their WAF feature: https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/application-gateway-crs-rulegroups-rules?tabs=owasp32. Again, the problem is that this feature does not stop regular DDoS attacks on Layer 7 since the attack pattern can be a simple web request and the attacks seem like regular user traffic. Therefore, it would be difficult to block the traffic from the attack without impacting actual end users.

- **Rate limiting**: The most common approach to reduce the impact of HTTP DDoS attacks is to add rate limiting. This allows us to define a threshold on our website. Let us say that someone sends over 100 requests per second – we can impose a QoS check on their traffic or block it entirely. Many vendors have also implemented a **JavaScript (JS)** challenge, which means that if they reach a threshold, the service will send them a JS challenge (just a small JS script) that they need to run on the endpoint making the request. As mentioned, DDoS attacks are often triggered by scripts that are not able to parse the JS challenges and therefore fail the challenge and get blocked. Legitimate web users with a browser can run and process the JavaScript challenge and therefore will not be blocked.

When using Microsoft Azure to host your web services, the WAF feature within Application Gateway does not include rate-limiting capabilities. To implement rate limiting in Azure, you can use the Azure Front Door service, as demonstrated in the following figure. Azure Front Door allows you to create custom rules, including rate-limiting rules, which will be applied to all users accessing your website. It's important to note that the rule should be configured to match the hostname of your website:

![](img/7b49a27a3c2adc52d097f2e77f3bf394_201_0.png)

Figure 6.11 – Azure Front Door and rate limiting

We can see that DDoS-based ransomware attacks have become more common. Fortunately, DDoS attacks have had a limited impact on customers that use the public cloud or other public DDoS protection mechanisms such as Cloudflare or Akamai because of their large infrastructure and ways of processing traffic.

While DDoS attacks may simply be annoying, they can also have a large impact on specific industries and infrastructures. Therefore, it is important to understand what options you have to minimize the risk of a DDoS attack impacting your business.

## Summary

In this chapter, we learned about ZTNA technologies and some of the vendors in the ecosystem and how we can use those services to securely publish our internal services to end users to reduce the risk of ransomware. Then, we went into some of the ways that we can securely access management solutions to manage our Windows-based infrastructure using both built-in tools and third-party options.

Then, we went into DDoS trends and how DDoS attacks can affect businesses, and how we can protect ourselves against DDoS attacks at different levels.

Lastly, we went through network segmentation and some common best practices on how we should segment our network to reduce the possibility for attackers to do a lateral movement.

While we have focused on securely publishing services in this chapter, attacks can still happen. So, therefore, it is also important to be able to protect data so that attackers are not able to directly see the data if they manage to exfiltrate it. In the next chapter, we will focus on what kinds of services and methods we can use to encrypt and protect our data.

## Protecting Information Using Azure Information Protection and Data Protection

In recent years, most ransomware attacks have changed from just encrypting data to encrypting and exfiltrating data. The reason is that most companies realized that they just needed to restore their encrypted files from a backup, which meant that the ransomware group did not get any payment. Therefore, to increase the probability of getting paid, ransomware groups have started exfiltrating any data they can find.

In this chapter, we will look at how to use services to encrypt data to reduce the risk of data getting in the wrong hands. We will take a closer look at some of the built-in services in Windows and Azure Information Protection (AIP) and lastly look at some common best practices related to data protection and backup.

In this chapter, we are going to cover the following topics:

- Why should you classify and encrypt your data
- Overview of AIP
- DLP features and the future of AIP
- Encryption of SQL-based workloads
- Best practices for data protection and backups

### Technical requirements

In this chapter, we will cover some functionalities that are part of Microsoft E5 using features of AIP. If you signed up for a trial license in the previous chapters, you will already have access to the features that we will walk through in this chapter.
