
![](img/095df2cd1930baecbcecaa51ee3417aa_29_0.png)

6. Once completed, the Kali VM will now boot. Log in with the credentials we used with the Raspberry Pi image:

```
Username:root
Password:toor
```

7. You need to then change the default password that was set to make sure no one can get into it. Open up a terminal within the Kali Linux VM and use the following command:

```
passwd <INSERTPASSWORDHERE>
```

8. Make sure the software is up to date, like you did for the Raspberry Pi. To accomplish this, you can use the following commands:

```
apt-get update
apt-get upgrade
apt-get dist-upgrade
```

9. Once this is complete, the laptop VM is ready to go.

## Summary

Now that we have reached the end of this chapter, we should have everything that we need for the penetration test. Having had the scoping meeting with all the stakeholders, we were able to get answers to all the questions that we required. This included engagement questions and scoping criteria. A deliverable from the stakeholders after the scoping meeting should have included any documentation that you would find beneficial for the penetration test. We typically require a network diagram, organization chart, and any important data flow diagrams.

Once we completed the planning portion, we moved onto the preparation phase. In this case, the preparation phase involved setting up Kali Linux on both the Raspberry Pi as well as setting it up as a VM on the laptop. We went through the steps of installing and updating the software on each platform, as well as some basic administrative tasks.

In `Chapter 2`, *Information Gathering*, we will start diving into the information gathering task. In this task, we will look at the various types of information to investigate, and where to look for additional information. We can't always rely on the stakeholder documentation being correct, and there may even be undocumented systems that even they don't know about. It is our job to find those systems. We will use various tools to get this information in order to help us understand the security and systems in place so that we know where to start the penetration tests later.

## 2

## Information Gathering

With all the information that was received from the scoping meeting, it is now time to not only validate that information, but also learn as much as you can from your own information gathering research. The goal is to gain as much knowledge as possible about the network and systems before starting to scan for vulnerabilities and then exploiting them.

In this chapter, you will learn to use various tools to start and map out the network and systems and then enumerate your findings. The more information you can get from this phase, the easier it will be to find vulnerabilities and exploits. This step can save you a lot of time later in the lab. For example, if you learn that a web server is a Microsoft Windows 2012 server, you can utilize this information for a better understanding of how to approach the exploitation phase. Without this information, you may try a bunch of exploits against this server but they will not work because they are not meant for a Windows 2012 server.

With all that being said, there is always the temptation to speed through this phase after you find some systems you would like to probe deeper into. You must, at all costs, resist this action. You must spend a good portion of your allocated penetration-testing timeframe during this phase. The actual time spent here will depend on the overall engagement time. I cannot emphasize enough; this is probably the most important phase in the overall penetration test. The more the time you spend understanding the environment and targets, the less the time you will waste in other phases of the penetration test.

In this chapter, we will discuss the following topics:

- Various tools in the toolbox
- Whois, dnsmap, and DNSRecon
- Nmap
- P0f
- Firewall Dotdotpwn

## Understanding the current environment

Before jumping into the various tools to map out and probe the network, security, and systems in place, it is a good practice to review the current documentation that you may receive as a part of the stakeholder meeting. Along with that, you will want to interview various teams within the organization to get some background information on what the topology is like, prior to you having to discover it yourself. It definitely makes the job of the penetration tester easier if you have some sort of layout already defined, as opposed to spending days probing and mapping to just get to that point.

Here is a list of teams I tend to talk with. The teams you choose will ultimately depend on the organization and what is in scope for the penetration test:

- **Network Team**: Interview the network team to get a better understanding of the current network topology, network devices currently in play, network vendors that are used, and any other information that will help you understand the network.
- **Systems Administrators**: Talking with the systems administrators will help you get an understanding of the various operating systems that are in place as well as server roles, applications, and data flow throughout the applications. This information is priceless when it comes to looking at the application layer and vulnerabilities that may be in play.
- **Security Team**: This one is near and dear to my heart. Understanding the current security devices that are utilized in the network is key to really understanding the topology. Firewalls and devices may be hiding portions of the network, so to know where and why they exist will help you lay out the network and any potential targets. Also, understanding the company's security policies such as patch management policies, password policies, and endpoint policies, will definitely help you identity targets for future tools.

![](img/095df2cd1930baecbcecaa51ee3417aa_32_0.png)

The preceding list only represents a small list of teams to talk to prior to jumping into probing and mapping tools. The more people you can talk to, and the more information you can obtain, the greater the likelihood of success within the penetration test. One of the main reasons is strictly due to the time you will save by really delving into the details without wasting too much time on just mapping everything out.

## Where to look for information – checking out the toolbox!

Now that we have discussed why the information gathering phase is so important, let's jump into some of the tools to gain various types of information. The first thing you may notice very quickly is that there are a lot of tools and methods to gain increased intelligence from the environment. The second thing is that there is a lot of overlap of these tools and their respective function and outputs. This is a good thing, as with experience, every penetration tester will come to learn the tools they like to use the most, the ones they find the most beneficial, and the ones they want to avoid.

The following tools are the ones that I have used in my lab environment and in the real world to discover more information about what exactly is on the network I am running the penetration test on.

## Search engines as an information source

Search engines provide us with the means to answer any questions that we may have. Tools such as Google, Yahoo! or Bing have given us the ability to find the answer to any question, anytime we want. But these tools are very powerful as well for giving us information about the company or the environment that we plan to run the penetration test against. With various searches, you can look at various images of the environments that can help you lay out any physical security tests. You can also find any Google dorks who may have exposed some sensitive information about the environment. Google dorking is very popular in the information gathering phase.

Similar to Google, Shodan has emerged as the go-to search engine for any Internet connected devices. This search engine has great possibilities for exposing and divulging all sorts of information about an environment. Browsing the site can give you information on webcams that are connected to the Internet, search engine results of known vulnerable devices, C2C servers, botnets...the sky's the limit. Save yourself some time and stress, and utilize these tools to help understand the environment. You may quickly find out that the environment already has some large security issues before you even start to scan it.
