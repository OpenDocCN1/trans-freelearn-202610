# Discovering delay/jitter-related application problems

Jitter and delay can influence various types of applications. In applications that run over TCP, high delay reduces the effective throughput that can be sent and high jitter can cause packet losses and retransmissions. In multimedia applications that run over RTP, which runs over UDP, high jitter and delay can cause severe disturbances in the voice and video quality.

In this recipe, we will get into the details: the influence of behavior on TCP, and how it can influence the application behavior. RTP over UDP behavior was discussed in Chapter 12, SIP, Multimedia, and IP Telephony.