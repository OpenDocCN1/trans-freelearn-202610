
The preceding architecture depicts a situation where **Operational Technology (OT)** equipment and IT devices and systems formed independent, unrelated network segments, with a distinct physical separation between the two networks. This was mainly because the two areas of technology had very little overlapping technological areas.

Looking at this architecture from a cybersecurity perspective, it is very desirable. In order to remotely compromise and attack this setup, the maintenance laptop has to be infected with a **Remote Access Trojan (RAT)** or be in some other way remotely accessible while connected to the ICS equipment. Sounds like the ideal setup, right? Well, as with all good things, this too ended. The biggest drawback of the setup shown in *Figure 4.2* is the variety of technologies. With every vendor requiring their own type of communication media and I/O modules, the number and variety of spare parts needed to keep an enterprise's facility safely stocked grew exponentially with the amount of ICS equipment owned. Also, the technical knowledge a person needed to have to support all this grew, along with the variety of equipment vendors.

To compound things, not only did every vendor implement their own proprietary technology, but additionally, within each vendor's offering, things changed as well. The vendor could start offering improved, better, or updated technology; additional functionality; or whatever other story to make you buy more equipment. This need to somehow standardize the insane variety in proprietary communications options, in combination with the ever-increasing demand for communications bandwidth (as well as improving and standardizing ease of use and manageability) drove the demand for some common denominator in the industrial communications field.

As the various ICS equipment vendors were not likely to agree on a single command-and-communications protocol, the next-best thing was chosen: a common way to transmit their proprietary command-and-communications protocols over a shared media network. Probably gleaned from the IT side of the equation, Ethernet was chosen for the job. The expandability of the chosen networking standard allowed ICS equipment vendors to easily adapt their proprietary protocols to the Ethernet networking standard by simply encapsulating the protocol in an Ethernet frame. Additionally, the industrial protocol carrying Ethernet frames could now be routed over (public) networks by incorporating the IP as well as add the reliability of session management by using the TCP. With this change, things started to look up for the overwhelmed industrial support/controls engineer, who now only needed to know how to deal with common networking technologies such as Ethernet and IP.

Because of the ease of deployment, ease of use, and highly supportable change in technology, OT could now to some degree be supported by the large community of IT professionals that had dealt with Ethernet and the TCP/IP stack for years. Everything OT-related slowly started adapting the Ethernet and TCP/IP standards. And when I say everything, I mean everything! PLCs, HMIs, I/O modules, programming workstations, actuators, sensors, status display boards, valve blocks, signal lights… everything! Because of this aggressive move to Ethernet and the TCP/IP standard, around the mid-2000s from an ICS and business network perspective, a typical plant looked like this:

![](img/c8ffa9959721097701584fcc95101362_118_0.png)

Figure 4.3 – Typical IT/OT network architecture in the mid-to-late 2000s

What a wonderful time this was! Every industrial system, device, and component was interconnected while still disconnected from the dangers of the office/business network. Because of this, anything on the ICS network could freely communicate with everything else on the same Ethernet network, and all the equipment was accessible from the maintenance office or wherever you could hop on the ICS network.

I must admit, the controls engineer still sees this period in industrial network evolution as the golden age of ICS networking. Life was great and the risk was relatively low. I remember commissioning a new production line from a rooftop in Los Angeles while connected to the all-industrial wireless network we had just installed: what a freedom; what a convenience! But then came the downfall: we had pushed things too far, without really realizing the can of worms we had opened for ourselves. Engineers wanted to have access to business email on their engineering workstations as well as access to ICS vendor websites to pull down updates or read technical documentation; the plant manager demanded up-to-date and on-the-fly production statistics and performance numbers on their tablet while sitting on a beach in Tahiti; and plant controllers, quality folks, and Six Sigma black-belt employees wanted to get uptime numbers, production counts, details, and downtime causes, among other **Overall Equipment Effectiveness (OEE)** data from the plant's production systems. The automation and controls vendors had added all this data-generating functionality to their IP-enabled gear and it was ready for the taking, over the same wires and technology the takers of all that data were using.

So, what was the most natural thing to do? Well, just tie the two separate networks together, right? Why not—they both use Ethernet networking standards and can communicate via the TCP/IP protocols with one another, so what could be more convenient? So, the following happened:

![](img/c8ffa9959721097701584fcc95101362_120_0.png)

Figure 4.4 – The "dual-homed network" business-to-industrial network cross-connection solution

Certain systems were dedicated to dual-homed computers, connected to both the business network and the industrial network to bridge that gap. On one hand, they could communicate with industrial equipment to gather data, be used to program or troubleshoot automation and controls devices, and could function as gateways to access shared files and data with industrial-only devices.

On the other side, these dual-homed systems were able to provide business functions such as email and internet access to employees on the production floor, or could allow business users to generate reports, view statistics, or in some other way pull production data from the production floor.

The dual-homed scenario still maintained some of the separation between business and industrial networks, but often the tie between the two was implemented in a less restrictive way, as is shown in the following screenshot:

![](img/c8ffa9959721097701584fcc95101362_121_0.png)

Figure 4.5 – The "flat network" business-to-industrial network cross-connection solution

This basically created a completely flat network, not separating business network traffic from industrial traffic in any way.

Most of us controls engineers have made the mistake of implementing one of the two aforementioned “hacks” to address the business need to pull data from the production network and supply business functions to the folks on the production floor. I know I did. But after coming to our senses (or suffering through a cyberattack/compromise), we started communicating with our IT department and came up with the following solution architecture to get that cross-connectivity between business and industrial networks:

![](img/c8ffa9959721097701584fcc95101362_122_0.png)

This is a pretty solid solution and seems to cover all the functionality and security requirements we are after, or at least on paper it looks like this. The reality is that over time, the configuration of that firewall in the middle there will have as many holes poked in it as a piece of Swiss cheese, meaning that over time, more and more protocols and features will be allowed through the firewall. So, before long, that firewall configuration will look like this:

![](img/c8ffa9959721097701584fcc95101362_123_0.png)

Figure 4.7 – Swiss-cheese firewall configuration

This begs the question, with all these firewall exceptions in place: is it still a firewall, or did we just build a glorified router? The slew of firewall exceptions allows unrestricted access from the enterprise network to the industrial network and vice versa, thus we have industrial protocols (which inherently have no regard for security) leaking from the ICS network to the business network.

At this point, an attacker does not have to be on a production facility’s premises to attack the ICS environment. With an unrestricted and uncontrolled connection between the business network and the industrial network, all an attacker would need is a foothold into the business network to start attacking the industrial network. The firewalled business-to-industrial network cross-connectivity architecture also paves the path for pivot attacks on the ICS environment that have made the headlines lately. With a pivot attack, an attacker compromises the enterprise side of an industrial organization by, for example, sending an individual in that company a phishing email with malware or some sort of exploit attached to the body of the email.
