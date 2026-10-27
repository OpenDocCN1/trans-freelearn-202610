Unless you've been playing with cables and switches for the last several years, this may be a new concept. So, before we talk much more about the service mesh, I want to get a baseline of networking laid out. Don't worry, we aren't prepping for a Cisco Certified Internet Expert exam.

# Understanding the network connection

When our applications communicate with each other, the messages that are transmitted need to travel through a variety of mediums. When we open up our favorite video conferencing software, the application connects with a server on the other side. I think we all can understand that. But what facilitates that communication? We can say it just goes over Wi-Fi, but what does that really mean? Well, the application software and the Wi-Fi are just layers within the communication path. Thankfully, we aren't dealing with the number of layers of an onion, or an ogre. And we're even going to simplify the number of layers for our discussion as well.

![](bbox=[0.1, 0.43, 0.85, 0.65])

Figure 6.1 – Vidona connection over Swampnet

Take a look at Figure 6.1. Due to a pandemic in the swamp, Shrek and Donkey haven't been able to see each other for quite some time. However, they've discovered a new application, Vidona, that allows them to see each other virtually. When Donkey calls Shrek, the message originates in Vidona, the application in use. That message flows through a channel and is wrapped up in an envelope with Shrek's address, and Donkey's return address. The message then flows down to the physical layer. Although we can't feel Wi-Fi signals (wouldn't that be wild if we could?), the Wi-Fi radio exists physically. In order to send the message on the Wi-Fi radio, it is packaged in another envelope, with a different address representing Donkey's physical tablet.