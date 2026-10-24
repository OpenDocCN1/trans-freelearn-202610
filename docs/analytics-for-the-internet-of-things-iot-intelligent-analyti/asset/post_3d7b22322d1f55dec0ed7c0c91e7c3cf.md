
There are two types of device inside a 6LoWPAN network, **hosts** and **routers**. Routers can forward datagrams to other devices. Hosts are simply endpoints. They can operate in sleepy states, periodically waking up and checking in with their parent device (router) for new data.

![](img/9f2a55db1236f995dcd9392acbc82910_51_0.png)

Example of a 6LoWPAN network

## ZigBee

ZigBee is a little different animal than what we have talked about so far. It covers multiple layers of the protocol stack. It is the network protocol, the transport layer, and an application framework that is the underpinnings of a customer developed application layer.

![](img/9f2a55db1236f995dcd9392acbc82910_52_0.png)

It has security builtin as a requirement and works using small, low power digital radios. It is a self-organizing wireless mesh-style network. The name comes from the waggle dance honey bees often do on returning to the hive.

The range is short, up to 100 meters line of sight, but it can travel long distances by passing data through a mesh network of other ZigBee devices.

The ZigBee alliance is a consortium of companies that maintain and publish Zigbee standards. ZigBee was first released as a standard in 2003. It is intended to be simpler and cheaper than other wireless protocols. Development code can be as little as 2% of equivalent code on Bluetooth.

Data rates are low and devices tend to be simple. ZigBee standards require at least a two-year battery life for a device to qualify. The data transmission is periodic and intermittent. There are different categories of ZigBee device. A fully functioning device can act as a network coordinator and operates in both star and mesh topologies. A reduced functionality device can only communicate with a network coordinator and works in a star topology.

These devices form a **Personal Area Network (PAN)**. Network interaction times can be very fast. A device can join a network in 30 ms and change from sleeping to active in 15 ms.

There are three types of ZigBee devices in a network:

- A **coordinator device** that initiates network formation. There is one and only of these for each network. It may also act as a router once the network is formed. It serves as the trust center and is the repository for security keys.
- A **router device** is optional. It can join with a coordinator or another router device and acts as a multi-hop intermediary in routing of messages.
- An **end device** has just enough functionality to communicate with its coordinator. It is optional and cannot participate in the routing of messages.

![](img/9f2a55db1236f995dcd9392acbc82910_53_0.png)

The ZigBee networking routing protocol is reactive. It establishes a route path on demand instead of proactively such as IP. The network protocol used is **Ad hoc On-Demand Distant Vector (AODV)** which finds a destination by broadcasting to all its neighbors, which then send a requests to all their neighbors, until the destination is found.

The network is silent between connections. Networks are self-healing. ZigBee networks are secured by 128 bit AES encryption. There is often a gateway device that is part of the ZigBee network and also linked to the internet. It translates data into the appropriate format for messaging over the internet. It can also convert ZigBee network routing protocol into IP routing protocol. The gateway is commonly an unconstrained device that does not need to worry much about power.

### Advantages of ZigBee

There are several advantages to ZigBee networks:

- **Reliable and robust**: Networks are self-forming and self-healing.
- **Low power requirements**: Devices can operate for years without needing a new battery. Solar power could last even longer.
- **Global**: The wireless protocol it is built on is available globally.
- **Security**: Encryption is part of the standard instead of an optional add on.

### Disadvantages of ZigBee

There are also some disadvantages:

- **Not free**: It costs $3,500 USD at the time of writing to license the standard
- **Low data rates**: It is not designed to support higher data rates
- **Star network is limited**: The coordinator device supports up to 65,000 devices

### Common use cases

ZigBee is commonly used in the following applications:

- Home automation
- Utilities monitoring and control
- Smart lighting
- Building automation
- Security systems
- Medical data collection

## NFC

**Near Field Communication (NFC)** allows two devices to communicate when brought within very short distances of each other - about 4 cm. It is a set of communication protocols that offer low speeds and simple setup. There is a variety of NFC protocol stacks depending on the type and use case.

![](img/9f2a55db1236f995dcd9392acbc82910_55_0.png)

NFC logo

NFC uses electromagnetic induction between two loop style antennas when devices exchange information. Data rates are around 100-400 kbit/s. It operates over radio frequency band 13.56 MHz, which is unlicensed and globally available.

There is usually a full NFC device, such as a smartphone, and an NFC tag, such as a chip reader. All full NFC devices can operate in three modes:

- **NFC peer-to-peer**: Devices exchange information on the fly.
- **NFC card emulation**: Smartphones can act like smart cards in this mode, allowing mobile payments or ticketing.
- **NFC reader/writer**: Full NFC devices can read information stored in inexpensive NFC tags, such as the type used on posters or pamphlets. You have probably used this at a conference to scan your attendee badge.

NFC tags are usually read-only, but it is possible for them to be writeable. There is an initiator device and a target device in NFC communications. The initiator device creates an RF field that can power a target device. This means NFC tags do not need batteries in what is called passive mode. Unpowered NFC tag devices are very inexpensive, just a few pennies.

There is also an active mode where each device can communicate with the other by alternatively generating its own field to read data, then turning it off to receive data. This mode typically requires both the devices to have a power supply.

For data records that eventually end up in an analytics dataset, the full NFC device would act as a gateway, translate the message, and then communicate over a different network protocol stack. In the case of a smartphone, it would take the NFC tag data and create its own data message to send over 4G/LTE for a destination on the internet.

### Common use cases

NFC is commonly used in the following applications:

- Smartphone payments:
  - Apple pay
  - **Host Card Emulation (HCE)** - Android supported
- Smart posters
- Identification cards
- Event logging
- Triggering a programmable action

## Sigfox

Sigfox is a company based in France that has developed a cellular style, ultra low power, long-range networking technology. It is intended for small messages that are sent infrequently. Data rates are very low at 100 bits per second (bps). The signals used can travel up to 25 miles, and a base station can handle up to a million nodes. Similar to cellular, it is not intended to be for private networks, as it requires coverage in the area to use it.

![](img/9f2a55db1236f995dcd9392acbc82910_56_0.png)

Sigfox devices can be very low cost due to low complexity and power requirements. Power requirements could be as low as one thousandth of equivalent cellular communications. It has been used in inexpensive, but crucial, things such as smoke detectors and a fitness tracking collar for dogs (seriously).

The current coverage includes the entirety of France and Spain, several other pockets in Europe, and some metropolitan areas in North America.

## Connectivity protocols (when power is not a problem)

These protocols are more about speed and high bit rates. Power constraints are less of a concern. They usually require more complexity in the supported IoT devices.

## Wi-Fi

Wi-Fi is any wireless network that follows IEEE 802.11 specifications. It is ubiquitous and global. Provided you have not been hiding in a cave for the last decade, you know it well. Many IoT devices use it to connect to the internet, so it is useful to review how it works.

![](img/9f2a55db1236f995dcd9392acbc82910_57_0.png)

To connect to a **Wi-Fi Local Area Network (WLAN)**, a device needs a network interface controller. This may be a separate controller card or simply integrated as part of the chipset inside the device. Communication uses Ethernet-style data packets over pre-identified radio communication bands. Packet delivery is not guaranteed and uses best effort delivery mechanism. Network protocol layers higher up in the stack, such as TCP or UDP, can offer guarantees of data delivery on top of the Wi-Fi standards.
