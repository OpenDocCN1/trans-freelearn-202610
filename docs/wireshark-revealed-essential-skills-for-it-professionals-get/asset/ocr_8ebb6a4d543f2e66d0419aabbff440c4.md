![](bbox=[0.073, 0.064, 0.926, 0.319])

Figure 7.30: Telnet traffic CTF1

Now, the next step would be to follow the TCP stream of these packets, which will reveal more information regarding the Telnet session.

This is what the question was about: leveraging the weakness in a remote administration service. Telnet sessions can be viewed in plain text format, and we finally leveraged the weakness to take advantage of viewing the session's information in plain text format. The flag is the password used by the user to log in to the Windows machine to perform maintenance activities.

FLAG : Sup3rs3cr3t

The following screenshot illustrates how the TCP stream windows will look after the packets are assembled. Also, the Telnet session's password can be seen clearly.