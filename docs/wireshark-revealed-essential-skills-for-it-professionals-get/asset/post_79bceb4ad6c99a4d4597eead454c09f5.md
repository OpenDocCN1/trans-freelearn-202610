
to request for its details; this is basically used to find nearby APs.

- **The probe response frame**: This frame is sent in response to the request that AP/STS might have received from another device in the network.
- **The reassociation (request/response) frame**: This frame is sent to the new AP when an STA's association with the current AP gets dropped. In response, the AP acknowledges the acceptance/rejection for the reassociation request.

Monitoring the time gap between each beacon frame sent from the hosts can be useful when dealing with high latencies. Due to these beacon packets broadcasted from the AP, the devices know that they are available to connect to.

- **Control**: This is to ensure that the delivery of the packets between the hosts manages the level of congestion in your channel and uses packets such as clear-to-send and request-to-send. In short, we can say that these frames are used for maintenance tasks. These control packets ensure the integrity of the packets that are transmitted. Likewise, the management frame several kinds control frame has just three kinds:
  - **Request-to-send (RTS)**: This frame is sent by the STA to request for gaining the control of the medium for a particular duration.
  - **Clear-to-send (CTS)**: This frame is sent by the AP from where it received the RTS to specify when the medium will be allocated to the STA for transmission. This frame is often used for protection from older stations that want to gain access to the medium again.
  - **Acknowledgement (ACK)**: This frame is sent by the receiving STA to tell the sending station that the data packet was received successfully. If the sending station does not receive this packet, then after a definite period of time, the sending station will resend the data packet to the same recipient to ensure the delivery of the packet.
- **Data**: These frames contain the data that is actually sent between the hosts. These are the only frames that get transmitted between the wireless and the wired domain.

The 802.11 packets are similar to the wired network packets that we saw; the terminologies do differ a little bit, but the basic concept is identical. Let's take a look at a beacon frame. Refer to the following screenshot for that:

![](img/da40f92928403866599198344271d63e_1240_0.png)

Let's take a look at the fields present in the frame in detail:

This is the first section in the frame header that lists out quite a good amount of info in it.

- **Frame Control**
- `Protocol Version`: This represents a 2-bit value that is used to verify the version of the protocol in use; the current version is 0 at the time of writing.
- `Type`: This identifies the type of the frame; in our case, we are dealing with a management frame (beacon).
- `Subtype`: This represents the subtype of the header; for us, it is a beacon frame for which we are seeing a numerical code 8.
- `DS Status`: This represents whether a data frame is heading to a **distribution system (DS)** or working in which mode. If the bit is set to 1, then this must be a data frame; if this is set to 0, then this frame is probably a management/control frame.
- `More Fragments`: If this bit is set to 1, this means that the frame has been distributed into couple of parts and is being sent one by one.
- `Retry`: This bit is set to 1 when there is a requirement upon retransmission of the frame.
- `PWR Management`: If this is set to 1, it represents the current power management state of the STA whether it is `active: 0` or in the `power-save: 1` mode.
- `More Data`: This bit is set to 1 if the AP is trying to tell the STA in the power-save mode that it has more frames to send. In case of control frames, this will always be 0.
- `Order`: If this bit is set to 1, this means that the frame is forcefully lined up and would be sent in a sequence. Usually, this bit is not set because it might cost transmission performance.
- `Duration ID`: This denotes the time the sender might require for frame exchange; this is usually seen in an **request-to-send (RTS)** frame, which requests to occupy the medium for a certain amount of time.
- `Address 1/2/3`: This is the physical address of the communicating device (receiver, transmitter, and destination address).
- `Sequence Control`: This is composed of two subfields: a 12-bit sequence number and a fragment number of 4 bit. A sequence number field is used to identify the sequence of the frames that arrive and for their proper reassembly (this ranges between 0-4,095). The fragment number field is used to denote the number of fragments for each frame (this ranges between 0-15).
- `Address 4`: This represents the sender's physical address and would only be present in a wireless distribution mode.
- `Data/Payload`: This field is not part of the header, but at the end, it will be appended when data is being sent across. The size of this field can be up to 2,324 bytes.
- `FCS`: The frame check sequence field is used to perform a data integrity test; you must have heard about the **cyclic redundancy check (CRC)**, which helps in calculating a value related to the data we received. If the FCS value is identical to the one we calculated, then the packet is received without errors.

## RTS/CTS

These are one of those essential components of WLAN data transfers that avoid collisions from happening and ensure the integrity of the data that is sent. The following illustration determines the four-step process that takes place to follow a 100% fail-proof delivery:

![](img/da40f92928403866599198344271d63e_1242_0.png)

First, the AP sends a request to the STA to gain medium access; once the STA approves the AP's request, the AP starts sending data. As soon as the data transfer is completed, the STA sends an ACK packet to acknowledge error-free delivery. If the ACK is not sent, then then the AP will start retransmission after some time.

## Usual and unusual WEP – open/shared key communication

Here, we will discuss two types of **Wired Equivalent Privacy (WEP)** authentication procedures: open and shared keys. As a matter of fact, discussing WEP is really unnecessary, but we should be aware of how it works because you never know when you might be asked to troubleshoot an old router whose firmware is still not upgraded and just supports WEP as an authentication mechanism.

WEP-open is way better than WEP-shared because even when the password that you provide turns out to be wrong, you will get connected to the network; here, it reduces the chance of getting the router brute forced. If you are using WEP-shared communication, then an experienced hacker won't take more than 2 minutes to crack your strongest key, and because of the small pool of keys that WEP supports, your password won't last long.

So, to begin with, we need the infrastructure to capture packets that are required for WEP-open. A key point to note here is that the infrastructure I am using consists of three different machines: the access point on the 192.168.1.1 IP, the station on the 192.168.1.105 IP, and Kali Linux running Wireshark on the 192.168.1.104 IP. Refer to the following illustration to understand this:

![](img/da40f92928403866599198344271d63e_1243_0.png)

1. First, let's activate the monitor mode over my interface:

![](img/da40f92928403866599198344271d63e_1244_0.png)

In the bottom-right corner of the preceding screenshot, you can see the message that the monitor mode is enabled over the `mon0` interface. This is the same interface that we will use to capture `802.11` packets from our AP and STA.

2. Next, to confirm the channel over which my channel is working, I used the `airodump-ng mon0` command.

![](img/da40f92928403866599198344271d63e_1244_1.png)

3. Now, once we have figured out that the channel is 6, we can go ahead and make our interface listen specifically to this channel, thus avoiding any noise from other channels. To do so, I used the `iwconfig mon0 channel 6` command.

```
root@kali:~# iwconfig mon0 channel 6
```

Figure 1: Configuring mon0 interface to channel 6
