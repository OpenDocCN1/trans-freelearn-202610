# # Dissecting FTP communications

There are two types of mode a client uses to communicate with the server: active and passive. Both of them have a different approach to send and receive data. In earlier versions, active mode was in use by default, but these days, you can see passive mode in use by default. I will discuss each of them using my own virtual network where I have a FTP server (VSFTPD) configured on the `172.16.136.129` IP and a client at `172.16.136.1`. The following sections described the flow and show how the client and server will behave in the active and passive modes.

# ## Passive mode

+   - The client sends a SYN request to the server running at port 21.
+   - The client receives SYN/ACK from the server over a temporary port used.
+   - The client sends ACK to the server to confirm that the channel will be used for sending commands. Refer to the following screenshot:

![](bbox=[0.122, 0.426, 0.927, 0.47])

+   - Now, the client will be shown a welcome banner and will be asked for the assigned credentials:

![](bbox=[0.122, 0.575, 0.927, 0.694])

Figure 4.5: Server showing welcome banner and asking for credentials

+   - Normally, passive mode must be on by default. Performing a directory listing will tell you that the **Extended passive (ESPV)** mode is in use. In this mode, the client requests the server to listen on the data port and wait for the connection. In return, the server informs the client about the TCP port number used for the connection. Please refer to the below screenshot.