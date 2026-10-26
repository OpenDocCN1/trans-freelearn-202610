![](bbox=[0.12, 0.05, 0.93, 0.17])

5. During the mail transfer, be aware that mail clients can easily fill a narrow-band communications line. You can check this by simply configuring the IO graphs with a filter on POP.

6. Always check for common TCP indications: retransmissions, zero-window, window-full, and others. They can indicate a busy communication line, slow server, and other problems coming from the communications lines or end nodes and servers. These problems will mostly cause slow connectivity.

# SMTP communications

SMTP is commonly used for the following purposes:

+   - Server to server communications, in which SMTP is the mail protocol that runs between the servers
+   - In some clients, POP3 or IMAP4 are configured for incoming messages (messages from the server to the client); while SMTP is configured for outgoing messages (messages from the client to the server)

When you suspect slow server-to-server communications, follow these steps to resolve the problems.

+   1. Check if the servers are located on the same site:
+      - If they are located on the same site, you probably have slow servers or another application problem. In most of the cases, the LAN will not cause any problems—especially when both servers are in the same data centre.
+      - If they are not located on the same site (when the servers are located in a remote site through WAN connections), check the load on the WAN connections. When sending large mails, they can easily block these lines—especially when they are narrow band (several Mbps).
+   2. First, look for TCP problems; and check if you see them only on SMTP or on all other applications. For example, in the following screenshot, you can see many TCP retransmissions: