
## Analyzing e-mail traffic and troubleshooting e-mail problems – POP, IMAP, and SMTP

The common mail protocols for mail client to server and server to server communications are **Post Office Protocol version 3 (POP3)**, **Simple Mail Transfer Protocol (SMTP)** and **Internet Message Access Protocol version 4 (IMAP4)**.

Another common method for accessing e-mails is web access to mail, in which you have common mail servers such as Gmail, Yahoo!, and Hotmail. Some examples include **Outlook Web Access (OWA)** and **RPC over HTTPS** for the Outlook web client from Microsoft and others.

In this recipe, we will talk about the most common client-server and server-server protocols: POP3 and SMTP. We will also look at some typical problems by using the other methods.

## Getting ready

When users are complaining about mail problems, first check if there are any obvious problems such as wrong username, bad password, and authentication protocols that are not configured. If none, connect Wireshark with port mirror to the complaining client; and if there are many of them, configure port mirror to the common server or the communications line connecting to it (when there is a remote server).

![](img/da40f92928403866599198344271d63e_827_0.png)

5. During the mail transfer, be aware that mail clients can easily fill a narrow-band communications line. You can check this by simply configuring the IO graphs with a filter on POP.

6. Always check for common TCP indications: retransmissions, zero-window, window-full, and others. They can indicate a busy communication line, slow server, and other problems coming from the communications lines or end nodes and servers. These problems will mostly cause slow connectivity.

## SMTP communications

SMTP is commonly used for the following purposes:

- Server to server communications, in which SMTP is the mail protocol that runs between the servers
- In some clients, POP3 or IMAP4 are configured for incoming messages (messages from the server to the client); while SMTP is configured for outgoing messages (messages from the client to the server)

When you suspect slow server-to-server communications, follow these steps to resolve the problems.

- 1. Check if the servers are located on the same site:
  - If they are located on the same site, you probably have slow servers or another application problem. In most of the cases, the LAN will not cause any problems—especially when both servers are in the same data centre.
  - If they are not located on the same site (when the servers are located in a remote site through WAN connections), check the load on the WAN connections. When sending large mails, they can easily block these lines—especially when they are narrow band (several Mbps).
- 2. First, look for TCP problems; and check if you see them only on SMTP or on all other applications. For example, in the following screenshot, you can see many TCP retransmissions:

![](img/da40f92928403866599198344271d63e_828_0.png)

3. Check if they are because of a slow SMTP server. Is it a mail problem? When you look at the following screenshot, you see that I've used the TCP Conversation statistics. After checking the Limit to display filter checkbox and clicking on Packets at the top of the window (to get the list from the higher amount of packets), we can see that only 793 packets are SMTP from the retransmitted packets. There are 9014 packets retransmitted between 172.16.30.247 and 172.16.30.2 on port 445 (Microsoft DS), 2319 packets are retransmitted between 172.16.30.180 and 192.5.11.198 on port 80 (HTTP), and so on.

![](img/da40f92928403866599198344271d63e_828_1.png)

4. In this case, SMTP is influenced only by bad communications. It is not an SMTP problem.

5. Check for SMTP errors. In the following screenshot, you see an error code 451, which is also called the local error in processing server error. Also, a list of errors is listed.

## Tip

When something goes wrong, in most cases the server or the client will tell you about it. You just have to look at the messages and Google them. We will see many examples of this later.

You can also find a list of SMTP status codes in RFC 1893 (http://www.ietf.org/rfc/rfc1893.txt).

![](img/da40f92928403866599198344271d63e_829_0.png)

- 1. When you want to know which errors have been sent by the two sides, configure a filter as shown in the following screenshot:

![](img/da40f92928403866599198344271d63e_829_1.png)

- 2. Here you can see various events:
  - **Code 421:** This indicates that the mail service is probably unavailable (1).
  - **Code 452:** This indicates that the server cannot respond, and tells you to try again later. This happens due to load on the server or a server problem (2).
  - **Code 451:** (code 250 is shown in the screenshot, see the following note) This indicates the user over quota (3).
  - **Code 452:** This indicates that the mailbox size limit has been exceeded (4).
  - **Code 450:** (code 250 is shown in the screenshot, see the following note) This indicates that the host was not found (5).

## Tip

In SMTP (like in many other protocols), you can get several error codes in the same message. What you see in the packet list in Wireshark can be the first one, or a partial list of it. To see the full list of errors in the SMTP message, go to the packet details and open the specific packet, as in the following screenshot.

When you see too many codes, it indicates unavailability of the server. check with the server administrator.

![](img/da40f92928403866599198344271d63e_830_0.png)

## Some other methods and problems

Some other common methods that I mentioned earlier are web mail and RPC over HTTP:

- In web mail, we connect to the server with HTTPS; therefore this is exactly like working with HTTPS, as described in [Chapter 10](#), HTTP and DNS. After logging in to the server, if any problems occur, they will be HTTPS problems.
- RPC over HTTPS will be same. Since RPC is a protocol which usually loads the network, it is considered to be sensitive to high delays and jitter. Microsoft came up with a solution to work with their Outlook client over HTTPS and not with the standard RPC. Again, since communication runs over HTTPS, problems will be HTTPS problems.

## How it works...

Mail clients will mostly use POP3 for communications with the server. In some cases, they will use SMTP as well. IMAP4 is used when server manipulation is required, for example, when you need to see messages that exist on a remote server without downloading them to the client. Server to server communications are usually implemented by SMTP.

## Tip

The difference between IMAP and POP is that in IMAP the mail is always stored on the server. If you delete it, it will be unavailable from any other machine. In POP, deleting a downloaded email may or may not delete that e-mail on the server.

In general, SMTP status codes are divided into three categories, which are structured in a way that helps you understand what exactly went wrong. The method and details of SMTP status codes is discussed in the following section.

## POP3

POP3 is an application layer protocol used by mail clients to retrieve e-mail messages from the server. A typical POP3 session will look like the following screenshot:

![](img/da40f92928403866599198344271d63e_832_0.png)

- 1. The client opens a TCP connection to the server.
- 2. The server sends an OK message to the client (**OK Messaging Multiplexor**).
- 3. The user sends the username and password.
- 4. The protocol operations begin. **NOOP** (no operation) is a message sent to keep the connection open, **STAT** (status) is sent from the client to the server to query the message status. The server answers with the number of messages and their total size (in packet 1042, **OK 0 0** means no messages and it has total size zero).
- 5. When there are no mail messages on the server, the client sends a **QUIT** message (1048), the server confirms it (packet 1136) and the TCP connection is closed (packets 1137, 1138, and 1227).

In the case of encrypted connection, it will look nearly the same (see the following screenshot). After the connection establishment (1), there are several POP messages (2), TLS connection establishment (3), and then the encrypted application data.

![](img/da40f92928403866599198344271d63e_833_0.png)

### SMTP and SMTP error codes (RFC3463)

The structure of SMTP status codes is as follows:

```
class . subject . detail
```

For example, when you see status code 450, it means the following:

- **Class 4** indicates that it is a temporary problem
