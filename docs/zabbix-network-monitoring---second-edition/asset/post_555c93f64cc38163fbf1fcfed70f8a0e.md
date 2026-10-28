
## Preface

Imagine you're celebrating the start of the weekend with Friday-night drinks with a few friends. And then suddenly, your phone rings—one of the servers you administer has gone down, and it needs to be back up before tomorrow morning. So you drag yourself back to the office, only to discover that some logfiles have been growing more than usual over the past few weeks and have filled up the hard drive. While this scenario is very simplistic, something similar has happened to most IT workers at one or another point in their careers. To avoid such situations, this book will teach you to monitor your network's hardware, servers, and web performance using Zabbix, an open source system monitoring and reporting solution.

This book will guide you through setting up almost every feature in Zabbix. With step-by-step instructions, anybody should be able to configure monitoring. You could even say that this is an idiot's guide to Zabbix—but only because we all have had moments of not understanding something. In order to not leave you confused, throughout this book, I will try to hold your hand, push you forward, and explain everything in so much detail that you won't get frustrated at all.

## What this book covers

*Chapter 1, Getting Started with Zabbix*, covers Zabbix installation from scratch, including the initial database, server and agent daemons, and web frontend, all running on the same machine. We will also configure the Zabbix web frontend, using PHP to access the database.

*Chapter 2, Getting Your First Notification*, covers configuring Zabbix using the frontend to set up data gathering, triggering upon specified conditions, and informing us by sending an e-mail for a single data source.

*Chapter 3, Monitoring with Zabbix Agents and Basic Protocols*, takes you through setting up the most widely used and basic data-gathering methods—Zabbix agents and simple checks, such as ICMP ping and direct TCP service checking.

Chapter 4, Monitoring SNMP Devices, teaches you how to set up the industry-standard monitoring protocol, SNMP. We will explore both polling by Zabbix and receiving SNMP traps, which will allow us to monitor a large variety of devices, including printers, switches, UPSes, and routers.

Chapter 5, Managing Hosts, Users, and Permissions, covers hosts, users, and permissions, including host and user group functionality and their impact on permissions. We will also explore related topics, including host and host group maintenance.

Chapter 6, Detecting Problems with Triggers, looks at ways to define which conditions are noteworthy by crafting expressions to detect values outside of the normal range, and other cases.

Chapter 7, Acting upon Monitored Conditions, helps you figure out how to react to triggers firing by sending an e-mail, launching an external script, opening a report in a separate bug tracker, or even restarting a faulty service. We will also learn to configure escalations in Zabbix and figure out how hysteresis works.

Chapter 8, Simplifying Complex Configuration with Templates, tells us how we did it all wrong before and teaches us to improve our configuration using templates that allow us to apply uniform configuration to a bunch of hosts while customizing thresholds per host with user macros. We'll also explore template nesting, which allows creating very flexible configuration in a large and mixed environment.

Chapter 9, Visualizing the Data with Graphs and Maps, explains how to create visual elements to display gathered data, including several types of graphs and interactive network maps.

Chapter 10, Visualizing the Data with Screens and Slideshows, helps you configure screens that collect various types of elements to display and slideshows that allow cycling through several screens in an automated fashion.

Chapter 11, Advanced Item Monitoring, explores more advanced ways to gather information using external, calculated, aggregate, and custom item types to retrieve any information.

Chapter 12, Automating Configuration, teaches us how to discover filesystems, network interfaces, and other entities on hosts with low-level discovery. On a higher level, we will explore the ability to automatically manage hosts via network discovery and active agent autoregistration.

Chapter 13, Monitoring Web Pages, takes us through monitoring the accessibility, performance, and availability of web pages using the built-in web scenario feature of Zabbix.

*Chapter 14, Monitoring Windows*, covers Windows monitoring by installing Zabbix agents, using performance counters, and retrieving data over WMI.

*Chapter 15, High Level Business Service Monitoring*, demonstrates the "IT services" feature, which allows aligning monitored services in a tree and calculating SLA conformity. We will fake some data to see how this feature actually looks.

*Chapter 16, Monitoring IPMI Devices*, teaches us how to set up yet another industry-standard protocol, IPMI. Monitoring both analog and discrete IPMI sensors will be covered.

*Chapter 17, Monitoring Java Applications*, a separate daemon called Zabbix Java gateway is set up and used to retrieve basic information over JMX.

*Chapter 18, Monitoring VMware*, covers the built-in VMware monitoring support. We will explore the ability to automatically discover and monitor hypervisors and virtual machines.

*Chapter 19, Using Proxies to Monitor Remote Locations*, explores the usage of passive and active Zabbix proxies that collect the data on behalf of the Zabbix server and then transmit it back to the server, which helps with remote locations that can't be accessed directly because of firewall concerns and also reduces the load on the Zabbix server.

*Chapter 20, Encrypting Zabbix Traffic*, looks into one of the major new features in Zabbix 3.0: the ability to encrypt traffic between the Zabbix server, proxies, agents, and the get and sender utilities.

*Chapter 21, Working Closely with Data*, helps us figure out some details about how data is stored in the Zabbix database and how we can interact with it directly as well as using Zabbix's native XML import and export functionality to more easily create large amounts of configuration. We will also get started with the Zabbix API for more advanced automation needs.

*Chapter 22, Zabbix Maintenance*, teaches us about the Zabbix upgrade procedure, how different components of various versions can interact, and what database patching between versions involves. We will also explore the internal monitoring options available, discuss backup strategies, and talk in detail about all the configuration parameters Zabbix daemons have.

*Appendix A, Troubleshooting*, covers common pitfalls with installation, connectivity, configuration, and other areas.

*Appendix B, Being Part of the Community*, tells us how we are not alone and there's a community around the Zabbix monitoring solution, which we can reach via IRC, forums, and the community wiki.

## What you need for this book

You will need at least one Linux system, which could be a virtual machine as well. Depending on specific features discussed, you might also benefit from the following:

- Access to an SMTP (e-mail) server
- More Linux systems
- A device with SNMP support
- A Windows system
- A device with IPMI support
- A Java virtual machine
- A VMware instance

Some of these can be replicated on the same Linux box — for example, running snmpd or a Java VM will allow you to try out all the monitoring solutions without a separate system.

## Who this book is for

This book is perfect for system admins who are new to Zabbix and are interested in creating simple monitoring reports. This book assumes knowledge of basic system administration tasks. Zabbix has added quite a lot of new features over the years, and many features new to 3.0 will be covered, so even experienced users of previous versions should be able to find new information.

## Conventions

In this book, you will find a number of text styles that distinguish between different kinds of information. Here are some examples of these styles and an explanation of their meaning.

Code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, and user input are shown as follows: "One of the first monitoring solutions most administrators implement is a simple shell script invoked from a crontab that checks some basic parameters such as disk usage or some service state, such as an Apache server."
