

## Server security

Your CRM server is likely to have a permanent link to the Internet to provide remote users with access to its data while away from their office. The data they will be accessing is perhaps the most vital and sensitive information your business owns. Bells should be going off in your head, warning you to make sure that the server is properly secured, as this combination is too tempting for someone with malicious intentions. Cyber attacks, or the attempted theft of your competitive information are two among the many very real threats for which you must plan.

For a start, security specialists will always tell you that nothing is completely secure. There are simply levels of security —each more cumbersome and expensive than the last— and you need to implement a level of security that is sensible and appropriate in your business context.

Some minimal security measures you should consider include the following:

- A UPS (Uninterruptible Power Supply) to save your server from crashing when there is a power outage. Windows and Linux both have utilities that can receive a message from the UPS notifying the server when the UPS has gone over to battery backup, so that the server can be shut down in a controlled manner if the power remains out for too long.
- A locked server room, so only authorized employees can access the server, reducing its chances of being stolen, being damaged, or having its data compromised.
- A surveillance system is an effective deterrent of theft or vandalism. You should consider installing one that has the ability to monitor your server room.
- A VPN (Virtual Private Network) to limit the server’s exposure to the Internet, while simultaneously allowing remote users uninterrupted access to the system.
- A firewall between the server and the Internet connection, with only limited and specific access to the web server being permitted from the Internet.

If you are installing the SugarCRM server at your own office, you will typically position the server behind your firewall to protect it from the outside world, but will allow external access by opening ports on the firewall to the web server on your SugarCRM server. From a security point of view, it would be even better to actually have separate web and application servers, with security rules between them — but this is not a necessary security measure for smaller firms.

If your SugarCRM server will be collocated, a similar networking configuration should be used, but you must make sure that you talk to your service provider about this matter.

If you are using an On-Demand service, none of this will be vital to you, except insofar as you may wish to ask your service provider about the security architecture currently in use and how it handles the web server and application server issues.

## Emergencies and natural disasters

Backups and security measures are intended to protect you from foreseeable mishaps. Unfortunately, there are other situations that may arise that are often overlooked by individuals maintaining their own servers.

Fire and floods are two additional dangers you must be prepared to handle. Should a fire damage your office, you will want to have proper fire fighting equipment handy that will protect your server.

You must also be prepared for water emergencies, such as a flood or a burst water line in your office. To reduce such potential risks, it would be wise to not locate your server in an area that is near water lines. In addition, your server should not be sitting on the floor of the selected area. It should be placed in a server rack where it cannot be stepped on or easily damaged by rising waters.

Lastly, the area where the server is maintained must be kept cool. Sometimes this requires the purchase of additional air conditioning equipment. A closed room with 3 to 4 servers will warm up rather quickly and heat is not well received by servers. In extreme situations, the heat in such environments can cause the server to reboot or even shut down and in turn cause unexpected downtime.

## Bandwidth capacity and reliability considerations

We have all had the experience of visiting a useful website, containing a lot of valuable information, and being frustrated by its low speed or responsiveness. You need to make sure that your CRM users never have such an experience.

## Bandwidth, or connection speed, is an important link in the chain of good performance for a web-based CRM system, such as SugarCRM.

![](img/7ccaf60360d7c8e809d336edbba2263d_92_0.png)

In the previous diagram, the **Outbound Traffic** leaves **Your Offices** for the **Internet** at a speed called the uplink, or upload speed. **Inbound Traffic** arrives at your building through the downlink, or download speed. Notice that for a **Remote User** connecting to the server at your office; it is important that the uplink connection be reliable and fast so as to deliver the data as quickly as possible.

Usually, your employees will complain if the office download speed is slow, as web browsing, or retrieving e-mail from an external e-mail service will be slow.

Employees will also complain (at least the ones who access your CRM system while away from the office) if the office’s Internet connection has a slow uplink connection.

So the conclusion we must draw is that if we intend to house our CRM server at the office, we need to make sure that the office has a fast download and upload connection.

Another potential bottleneck for a **Remote User** is the speed of the connection they are using while away from the office. For such users, the speed of inbound traffic will have a direct impact on their experience of using the CRM system. Data travelling from the office to a remote user’s computer will travel only as fast as the slowest link between the two. For example, if the server at the office is able to deliver data through its outbound traffic link at a rate of 10 mbps, but the inbound traffic link being used by the remote user is limited to one tenth of that speed, the data from the office to the remote user’s computer will be received at 1 mbps and not at 10 mbps.

The last scenario highlights some of the challenges in determining the appropriate speed for your office. In the last example, increasing the speed of the outbound traffic connection at the office would have yielded zero benefits as the bottleneck was at the receiving end.

## CRM Deployment Options

Another factor to consider is the day-to-day importance of Internet usage within your business. A software development company, for example, will most likely have higher bandwidth requirements than a T-shirt printing firm, or for that matter, RayDoc Carpets.

For an average white collar business (if there is such a thing), 2 mbps of download speed for each of the 25 employees/ users will normally suffice. With a CRM server at the office being accessed by remote employees during the day, or at night from home, you will most likely need something like 1 megabit of upload or uplink speed for each of the ten concurrent remote users. Your needs may dictate that you get a dedicated connection to your CRM server in order to eliminate saturation of your connection from other activities.

You should avoid uplink speeds lower than 1 megabit if you intend to access SugarCRM remotely. It will still work on slower connections of course, but you may find that the response time is less than desirable and it may frustrate you.

Many Internet connections tend to be faster in the download direction than the upload direction. These connections are referred to as Asymmetric. If your CRM server is located at your office, your business becomes a candidate to have an Internet connection that is closer to being Symmetric, or balanced, in its upload and download speeds.

When you use an On-Demand service for your CRM deployment, or have your server collocated, your server benefits from the fact that the Internet connection at the provider's location is much faster in both directions than it is at your office. It is also a more reliable connection. Typically, these providers will utilize multiple suppliers for their Internet connection pipes, so that if the connection provided by one supplier fails, those from another supplier will automatically carry on moving traffic.

This redundancy is usually difficult for small businesses to afford. If you intend to use an On-Premise CRM deployment, the lack of this redundancy may be something you will have to accept. Nevertheless, you should investigate pricing for redundant connections in your area, as well as keep yourself informed on the level of guaranteed availability offered by the Internet Service Providers in your area. It also does not hurt to solicit input about them from your business associates.

Lastly, if you choose the On-Demand or Collocated deployment option, be sure you know the amount of bandwidth you are allowed to consume (per user or for your whole server), or you might end up paying for bandwidth surcharges. Some providers do not have any restrictions, or the limits are not practical, but it is important to understand any restrictions in advance so as to avoid any additional charges.

