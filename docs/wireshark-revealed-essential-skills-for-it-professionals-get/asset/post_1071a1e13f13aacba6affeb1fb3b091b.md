
## Chapter 12. SIP, Multimedia, and IP Telephony

In this chapter, we will learn how to use Wireshark in order to resolve and troubleshoot IP telephony, voice and video calls, video streams, and other types of multimedia sessions. In this chapter we have the following recipes:

- Using Wireshark's features for telephony and multimedia analysis
- Analyzing SIP connectivity
- Analyzing RTP/RTCP connectivity
- Troubleshooting scenarios for video and surveillance applications
- Troubleshooting scenarios for IPTV applications
- Troubleshooting scenarios for video-conferencing applications
- Troubleshooting RTSP

### Introduction

Various types of multimedia applications take up a significant part of modern communication networks. Among these applications, we have telephony, video conferencing, surveillance systems, distance-learning systems, and many more.

In multimedia applications, the requirements from the network are different from the requirements in other enterprise applications. While applications such as HTTP, e-mail, and file sharing require high bandwidth, a telephone call, for example, requires less than 100 Kbps, but is sensitive to delays, and very sensitive to jitter (delay variations). While most applications require high downstream to clients in remote offices, surveillance systems require the upstream direction as they are watched from monitors in the HQ, so the monitors actually download the video from the remote site.

In this chapter, we will focus on these voice, video, and multimedia applications, how they behave over network connections, and how to use Wireshark to troubleshoot problems when they don't work properly.

In this chapter, we will focus on **Session Initiation Protocol (SIP)**, **Real-time Transport Protocol / Real-time Transport Control Protocol (RTP/RTCP)**, **Real Time Streaming Protocol (RTSP)**, and other common multimedia protocols.

We will start this chapter by presenting the available tools that Wireshark provides us for troubleshooting voice and multimedia sessions. We will focus on how to resolve SIP problems and how to troubleshoot RTP/RTCP sessions, then we will learn how to troubleshoot video systems, including video conferencing and surveillance systems.

### Using Wireshark's features for telephony and multimedia analysis

First, let's see what tools are provided by Wireshark for monitoring voice, video, and multimedia.

#### Getting ready

While facing problems with voice calls, video-conference calls, or other multimedia sessions, connect your laptop with Wireshark and port mirror one of the following devices as shown in the following diagram:

![](img/da40f92928403866599198344271d63e_865_0.png)

Follow these steps to use Wireshark's features for telephony and multimedia analysis:

1. Monitor the local or remote clients **(1)** in cases where you suspect a specific client problem.
2. Monitor the local or remote **IP PBX** system **(2)** when you suspect a central problem that influences the entire IP Telephony network.
3. Monitor the connections to the router **(3 and 4)** while suspecting an interoffice connectivity problem.
4. Monitor the firewall on the LAN port **(5)** or on the connection to the service provider **(6)**. This connection will usually be over the Internet, but can also be on a direct line to the provider.

#### How to do it...

In the Wireshark window, open the **Telephony** menu, as shown in the following screenshot:

![](img/da40f92928403866599198344271d63e_866_0.png)

For telephony networks, use the following menus:

1. To view RTP information, navigate to **RTP | Show All Streams** as shown in the following screenshot:

   ![](img/da40f92928403866599198344271d63e_867_0.png)

2. The following window will open:

   ![](img/da40f92928403866599198344271d63e_867_1.png)

3. In the RTP Streams window, you will see the following details:
   - The source IP address and UDP port
   - The destination IP address and UDP port
   - Synchronization Source (SSRC), which is an RTP stream identifier
   - RTP payload type (usually codec type)
   - Stream data, which is the total amount of captured packets, packet loss, maximum time between packets, maximum, and mean jitter
   - Problem? that indicates a general problem in the stream

   In the lower part of the window, you have the following buttons:
   - Unselect: When you select a stream by clicking on its line, the Unselect button will cancel the selection.
   - Find reverse: On a voice or multimedia call, a reverse stream is the stream in the opposite direction (which will be highlighted in light gray).
   - **Save as**: This button can be used to save a stream in the `rtpdump` format. For information about the format, go to http://www.cs.columbia.edu/irt/software/rtptools/.
   - **Mark packets**: not functioning.
   - **Prepare filter**: This prepares a display filter in the display filter window.
   - **Copy**: This option copies the RTP streams to a text file. For doing so, click on **Copy**, open a text editor, and paste the content to the text file.
   - **Analyze**: When you click on a stream, and then click on the **Analyze** button, it opens the **RTP Stream Analysis** window. The same window can be opened by clicking on an RTP packet and navigating to **Telephony | RTP | Stream Analysis** from the menu.
   - **Close**: Clicking on this button closes the window.

4. To view SIP information, navigate to **Telephony | SIP**. Enter `ip` (or `udp` or `sip`) in the **SIP Packet Counter** window that opens, and the window **SIP statistics with filter: ip** will open as shown in the following screenshot:

   ![](img/da40f92928403866599198344271d63e_868_0.png)

   In the window, you will get the following SIP statistics:
   - Number of packets sent with SIP response codes (numbered as 1 in the preceding screenshot)
   - Total number of each one of the SIP methods (these are SIP commands) that were sent (numbered as 2 in the preceding screenshot)
   - Minimum, maximum, and average session setup times (numbered as 3 in the preceding screenshot)

5. For RTSP statistics, navigate to **Telephony | RTSP | Packet Counter**, write `ip`, `rtsp`, or just leave it blank in the pop up that comes up, and then click on **Create Stat** that opens a window as shown in the following screenshot:

   ![](img/da40f92928403866599198344271d63e_869_0.png)

6. For watching which telephone calls were captured, navigate to **Telephony | VoIP Calls**. A window as shown in the following screenshot will open:

   ![](img/da40f92928403866599198344271d63e_870_0.png)

7. In the VoIP calls, you see the following parameters:
   - The start and end time that give the duration of the call (numbered as 1 in the preceding screenshot)
   - The IP address from where the session had started (numbered as 2 in the preceding screenshot)
   - The SIP address from where the session started (numbered as 3 in the preceding screenshot)
   - The SIP address for whom the session was intended (numbered as 4 in the preceding screenshot)
   - The protocol of the session (usually SIP), the number of protocol packets, and the session status (numbered as 5 in the preceding screenshot)

#### How it works...

For transferring voice, video, or multimedia, we need two functions to be performed. The first one is to carry the media stream, which is mostly voice or video, and the second one is the signaling, which is to establish and terminate the call, to invite participants to the call, and so on. Two protocol suites were proposed over the years for the signaling:

- The **ITU-T** suite of protocols, including **H.323** as an umbrella protocol for the suite, **H.225** for registration and address resolution, **H.245** for control, and some others.
- The **IETF** suite of protocols including SIP as a signaling protocol (RFC 3261 with later updates) and **Session Description Protocol (SDP)** that describes the session parameters (RFC 4566).

The ITU-T set of protocols phased out in the last few years, and the majority of the applications today are using the IETF set of protocols, on which we will focus in this chapter. In the following diagram you see the structure of the IETF protocol suite.

For the stream transfer, both suites use RTP and RTCP (RFC 3550 with later updates). RTP is used for the media transfer, and RTCP for controlling the quality of the stream.

There are several protocols for transferring multimedia sessions over an IP-based network, as shown in the following diagram:

![](img/da40f92928403866599198344271d63e_871_0.png)
