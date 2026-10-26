# Analyzing SIP connectivity

As we learned in the previous recipe, **SIP** (RFC 3261 and various extensions) is a signaling protocol that is used for creating, modifying, and terminating user sessions between one or more participants. While sending SIP requests, the session parameters are sent via **SDP** (**SDP**, RFC 4566) which enable users to agree on a set of compatible media types between them. When sessions are created, the voice or video is carried by RTP and optionally controlled by **RTCP** (RTCP is optional, and can be used by multimedia applications, but it is not a mandatory protocol).

SIP defines endpoints as **User Agents (UAs)**, and the process of creating a session involves UA negotiation in order to agree on a characterization of a session that they would like to create. For additional services such as locating session participants, registration, call forwarding, and others, SIP defines network hosts called **servers** to which UAs can send registrations, invitations to sessions, and other requests.

In this recipe, we will discuss the signaling part of the protocol suite, which is SIP, and how to use Wireshark in order to troubleshoot signaling problems, while in the next recipe, we will learn about **RTP** and **RTCP**.

Problems in voice and video over IP can be categorized in to two groups:

+   - **Problems of call establishment, modification, and termination**: These will be problems such as instances when you pick up the phone and you don't hear a dial tone, you hear the dial tone but cannot dial a number, you dial a number but the other side doesn't get the call, and so on. These types of problems are usually caused by signaling issues.
+   - **Problems of quality**: These are problems related to quality, such as voice quality, disturbances (like noise) during the call, video freezes, and so on, which are usually caused by networking problems, RTP problems, or various types of media issues.

In this recipe, we will discuss the first type, and in the next recipe the second type.

# Tip

It is important to note that not all problems in this area are networking problems, and in many cases, they will be a result of bad configuration of the equipment (for example, telephony switches or end devices).