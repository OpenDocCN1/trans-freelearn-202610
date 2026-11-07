
## Summary

The challenge presented to me at the start of this chapter was to be able to create a secure tunnel to transfer our evidence offsite, while making sure we could get around any potential security devices. I also needed a way to get back into the environment from my C&C server without having the company create associated firewall access control rules and NAT rules.

Utilizing stunnel and reverse ssh, I was able to accomplish this. stunnel allowed me to have an SSL tunnel from the internal server to my external C&C server. The SSL tunnel is key since most firewalls will allow outbound SSL and most IPS setups will not decrypt the payload, and therefore it will go through without being inspected. The reverse ssh tunnel allowed the C&C server to piggyback off the already established ssh connection that originated from the inside server. Automating this allowed the entire setup to come back in the event of a reboot of the Raspberry Pi.

Using the script that I created, I was able to transfer files into the correct directory as well as log each transfer to a log file for auditing purposes. To make sure I made the best use of this tunnel, I talked about how to compress and split the files, as well as how to put them all back together on the other end.

In the next chapter I will jump right into the penetration test by performing vulnerability scans against some of our targets and then try to exploit these vulnerabilities with tools such as Metasploit. I will also jump into the user side of exploitation by looking into BeEF for browser-based exploitation and SET.

## 4 Vulnerability Scanning and Metasploit

How about we start the fun stuff now! Having completed the setup of our tools and systems, and completed the necessary information gathering, it is now time to start acting upon the data we have collected. Our focus in this chapter will be on scanning the environment for vulnerabilities and then attempting to exploit these vulnerabilities with tools such as Metasploit. We will also explore the user side by utilizing tools called **Browser Exploitation Framework (BeEF)** and **Social Engineering Toolkit (SET)**.

We will cover the following topics in this chapter:

- Vulnerability scanning tools
- Scanning techniques
- OpenVAS
- Getting started with OpenVAS
- Performing scans against the environment
- Getting started with Metasploit
- Exploiting our targets with Metasploit
- Using BeEF for browser-based exploitation
- Using SET for client-side attacks

## Vulnerability scanning tools

There are many types of vulnerability scanning tools out there. Some are commercial products, so you need to pay for them. These products typically have refined GUIs and are easy to use with great reporting functionality. These include scanners such as Rapid7, Qualys, and Tenable. There are others out there that are free. These include products such as OpenVAS, Retina CS, and Nexpose. These free tools are often not as easy to use and don't always have as many features as the paid versions. However, in my experience, OpenVAS is a great free tool. Nessus was one of the most popular free vulnerability scanners until they closed-sourced it, and since then requires an annual subscription.

Vulnerability scanners are designed to scan through an environment and look for known vulnerabilities within the network parameters. Depending on the product, there could be 10,000+ known vulnerabilities within vulnerability scanner product that can be used against targets. These types of vulnerabilities include network, web application, database, host-based, browser-based, or just a simple port scanner. A lot of companies have adopted the use of vulnerability scanners internally over the last couple years due to the increased pressure to maintain a secure environment. With the disastrous effects a breach can have on a company, security is at the forefront of everyone's minds; and if not, they will be shortly.

Vendors are even realizing the importance of vulnerability scanners in organizations and are starting to build integration into their products for vulnerability scanners. One of the ones that I am most excited about is an integration that Cisco has with their **Identity Services Engine (ISE)** and third-party vulnerability scanners. With ISE, you can use the vulnerability score a machine gets as a deciding factor as to whether that system can be placed in the correct VLAN on the network, or whether it needs to be pushed to a quarantine VLAN. I think this integration will only grow as more and more companies adopt vulnerability scanning as a normal business security function. Compliance plays a huge role in driving vulnerability scanning as well since standards such as PCI-DSS require scans to be done regularly.

The goal for every vulnerability is to find them before the bad guys do. They are designed not to take any systems down, but just report them (unlike what we will do later with Metasploit). They often take a long time to run just due to the sheer number of tests they perform, so plan appropriately. They should also be scheduled on a recurring basis. Typically, that is quarterly; however, if you have a high-change environment, you may want to perform them more frequently, or maybe just after bigger changes.

![](img/095df2cd1930baecbcecaa51ee3417aa_82_0.png)

You don't want a change to open up a hole in your environment.

## Scanning techniques

There are two major scanning techniques that are performed by organizations. These are internal and external vulnerability tests. You may think there is not really a lot of difference in them, but that is often a misconception. External scans look through the eyes of an outsider trying to come into your environment. So, they will interact with all the systems that are utilized by any public-facing services. This will include the public website and public-facing services as well. They are designed to test against whatever these external users have access to. So, the smaller your ACL is on your external firewall for inbound traffic, the smaller your attack surface will be. Always lock down your firewall to include only the necessary services and ports. External scans are typically tested via an approved scanning vendor, or ASV.

Internal scans, on the other hand, are designed to look for vulnerabilities within the internal systems. Typically, in this sort of test, the vulnerability scanner has full access to the systems. The reason is that these tests are looking for any issues within the system, not just those which are reachable. Internal scans are typically done by the in-house security staff most of the time, unless the organization does not have this type of personnel. If this is the case, that is where we can come in. Some organizations have internal boxes that constantly run scans and generate reports for management as well. This tends to be the case since internal systems change more often than public-facing services.

## OpenVAS

With the background information on vulnerability scanning complete, let's jump into the heart of vulnerability scanning and start checking some hosts. Like I mentioned earlier, there is a plethora of vulnerability scanning products out there. However, for my lab, I will be utilizing OpenVAS, which is a free, open source tool.

![](img/095df2cd1930baecbcecaa51ee3417aa_83_0.png)

Even though we are using OpenVAS in this lab, don't think this is a required scanner. If you have a subscription to some of the paid scanners, or if you prefer the use of another tool, feel free to use that. The syntax, execution, and process of the scans may be different; however, the overall results should be the same.

OpenVAS was a fork of the Nessus product, so it has a great history. OpenVAS is an extremely powerful scanner, but can be difficult to get the hang of. There is a defined structure to OpenVAS, which is labeled in this figure:

![](img/095df2cd1930baecbcecaa51ee3417aa_84_0.png)

Network vulnerability tests, or NVTs, are the defined tests that are brought into the scanner to be used in scans that will ultimately lead to results that are stored. OpenVAS has a great feature in which these NVTs are added and updated via a feed service. This makes sure that you always have the latest and greatest in terms of feeds. As of mid 2016, there were 47,000+ NVTs within OpenVAS. These plugins really make OpenVAS a very powerful and flexible tool. If there is a new vulnerability found, someone will make this plugin, and you can then load it into your system to scan for it on your network. This shows the openness and powerfulness of OpenVAS.

## Getting started with OpenVAS

When installing OpenVAS, you will notice there are a lot of moving parts to it. Because of this, one can initially become overwhelmed when starting with OpenVAS. However, once you play around with it and understand the flow, it's quite a powerful product.

After installing OpenVAS, you need to run the setup script to help finish the various tasks that need to be completed. This is done via the handy `openvas-setup` CLI command. This will ensure that not only is your OpenVAS setup all ready to go, but also ensures it's up to date with the latest rules:
