
Once the servers were ready to go, the software developer would share their masterpiece, placing it on the provided infrastructure. As users accessed the application, all would be great... until the app was no longer accessible. Suddenly, the software developer's phone would ring in the middle of the night. System engineers would struggle to fix hardware failures, thus bringing down the application and facing many angry end users. This required trips to the data center—which itself needed disaster recovery plans in case of a fire, flood, or storm. And how about if the application had a spike of major success? If tons of users rushed to type the URL into their browsers and navigate to the site, resources on existing hardware could be overloaded. The system engineers would need to purchase and install additional servers, firewalls, and load balancers to provide more resources for the additional traffic. All of these potential hazards could be a headache for everyone involved—especially a software developer whose focus should stay on the application itself.

## The birth of enterprise virtualization

November 10, 2003: A group of IT executives in dresses, business suits, and blazers stood around a boardroom, watching a screen projecting a video clip of Terminator II: Judgement Day. "Are you ready?" said one of the men standing at a podium. On the bottom right of the screen, a small box of a few Microsoft Windows control panels displayed icons representing running servers.

These servers were virtual machines, and the video clip the room was watching was being played from a file server on one of these virtual machines. Suddenly, the host clicked on a few context menus on the virtual machine and a progress bar appeared. The video continued to play as the T-1000 Advanced Prototype morphed from liquid metal into human form, chasing John Connor in a burning police car. The room exploded with hoots, hollers, and cheers—and it wasn't because of the Terminator. This was a demo of a technology called VMotion, which would revolutionize the IT operations world for years to come.

Although virtualization had been around since the 1960s, VMware can certainly be credited for bringing it to the masses in their official release of VMware VirtualCenter featuring VMotion. VMotion allowed system engineers to convert their existing physical machines to hypervisors, enabling the running of virtual machines. It also kept those virtual machines up by live-migrating them to another hypervisor in the event of an underlying hardware problem. This was done by transferring the virtual machine's active memory and execution state over the network.

Software developers and system engineers alike were overjoyed with the technology, knowing they would now sleep quite well while their application hummed along regardless of hardware headaches. The software developers continued to develop their incredible applications, but their development methodology remained the same. They were still developing their application as if they were on traditional physical machines!

Gartner, the information technology research and advisor company, has labeled this type of environment Mode 1, otherwise known as enterprise virtualization. In fact, enterprise virtualization still exists today and is the most adopted type of the cloud by enterprises around the world. See Figure 1.1.

| Enterprise Virtualization (Mode 1) | Elastic Cloud (Mode 2) |
|---|---|
| GUI-emphasized | API-emphasized |
| Expensive Hardware | Standard Hardware |
| Vertical Scaling | Horizontal Scaling |
| Proprietary | Open Source |
| Traditional Software Development | Cloudy Development |

Figure 1.1: Enterprise Virtualization (Mode 1) versus Elastic Cloud (Mode 2)

Here are some common characteristics of the enterprise virtualization cloud:

- **GUI-emphasized**: When one creates a new virtual machine in this model, they typically do so via a Graphical User Interface (GUI). This may be something like vSphere Web Client or Hyper-V Manager. Command-line tools may exist but are less popular with a majority of its operators.
- **Expensive hardware**: Enterprises that buy into enterprise cloud also buy into expensive blade servers, SANs, network/fabric switches, component redundancy, and high-cost, boutique super microcomputers—specialized hardware with a high price tag.
- **Vertical scaling**: If your application begins getting more traffic, the ability to scale will be necessary. In the enterprise virtualization world, this means assigning more resources to virtual machines powering the application. When one scales up or vertically, they go about this by adding more CPU, RAM, or disk space to the individual virtual machines. This is the opposite philosophy to scaling horizontally.
- **Proprietary**: The infrastructure code in this model is closed source. This includes software such as *VMware VCenter* powered by Microsoft Windows Server. If a bug is discovered while working with the software, it must be filed with the vendor. Because one monolithic organization controls access to the code, bug fixes can take weeks or months to be patched.
- **Traditional software development**: In an Enterprise Virtualization cloud, the software developer continues to develop the applications in the same manner they developed applications on physical machines. This is sometimes referred to as the *monolithic* approach. Although the web server or database may reside on different virtual machines, the application's functions are combined into a single program from a single platform. This differs drastically from the cloudy development approach explained in the next section.

## Amazon - not just a place for books

In March of 2006, Michael Arrington—founder of Silicon Valley news blog *TechCrunch*—had an exciting announcement:

> *Amazon Web Services is launching a new web service tonight called S3 – which stands for “Simple Storage Service”. It is a storage service backend for developers that offers “a highly scalable, reliable, and low-latency data storage infrastructure at very low costs... This is game changing.”*

Arrington was right. At this time, Amazon Web Services had yet to release the catalog of services we all know today. **Simple Storage Service (S3)** provided users with the ability to create an account, enter their credit card number, and have access to upload files to AWS's hosted infrastructure within seconds. One benefit stood out above all: unlimited storage space.

At the time, hosting companies offered **virtual private servers (VPS)**, allowing one to rent a server and use it for backup or file storage. The problems with these solutions was the space limitation on a particular server, not to mention the responsibility of the customer to maintain the health and security of the operating system. While a VPS may have charged users with monthly or yearly flat rates, S3 billed the user for what they used, much like a household electricity bill. The software developer now had the power to avoid the purchase of additional physical servers and storage arrays.

In 2006, there was one surprise left from AWS. On August 25, 2006, Jeff Barr, Chief Evangelist at AWS, announced the launch of **Elastic Compute Cloud (EC2)**:

> > "Amazon EC2 gives you access to a virtual computing environment. Your applications run on a "virtual CPU", the equivalent of a 1.7 GHz Xeon processor, 1.75 GB of RAM, 160 GB of local disk and 250 Mb/second of network bandwidth. You pay just 10 cents per clock hour (billed to your Amazon Web Services account), and you can get as many virtual CPUs as you need."

This post was the shot heard 'round the IT operations world. Not only could the software developer have access to unlimited storage space with S3, but create as many virtual machines as they wanted. A college student in their dorm now had access to the data centers, bandwidth, and computing power of large-scale enterprises. It was the competitive advantage they needed. This truly provided power to developers. They would quickly scale their application during times of success (with a few clicks of a button) and quickly handle failure elegantly if anything went wrong.

Gartner labeled this type of cloud Mode 2, otherwise known as **elastic cloud**. See Figure 1.1.

Here are some common characteristics of the elastic cloud:

- **API emphasized:** Both AWS S3 and EC2 offered **Simple Object Access Protocol (SOAP)** and **Representational State Transfer (REST)** APIs at the time of launch. The power of the API is the ability for a developer to easily incorporate these web services into their web application software logic. Visualize a website allowing users to upload and share photos. The developers creating the web application could make API calls to S3 whenever a user uploads or downloads a photo, thus ensuring all the users' uploaded photos were stored in AWS's S3 infrastructure. Although the SOAP API was deprecated by AWS in December 2015, the much more elegant REST API allows browsers to easily communicate with GET, PUT, POST, HEAD, and DELETE methods over the protocol that powers the web: HTTP.
- **Standard hardware:** Werner Vogels, Vice President and CTO of Amazon, once said *everything fails all the time*. The inevitability of hardware failures means that redundancy and high availability are going to be a fundamental part of the application running on top of the underlying hardware. If hardware fails, it shouldn't be a problem because the developer has ensured that their application follows the cloudy development style (see final bullet point).
- **Horizontal scaling:** When one needs more computing power, they rarely bother with resizing existing instances. They instead choose the unlimited nature of elastic cloud by creating more virtual machines and do work in parallel. This is also known as scaling horizontally.
