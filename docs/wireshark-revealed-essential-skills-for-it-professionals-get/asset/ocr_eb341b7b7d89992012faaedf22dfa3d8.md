![](bbox=[0.073, 0.064, 0.927, 0.397])

Figure 4.18: HTTP redirection

As you can see, a TCP handshake was initiated with the old server at 104 followed by an HTTP GET request. The server at 104 responded with a 302 Found response in packet 21, which is an indication of redirection. Our request was sent to the new server located at 103 with whom we again initiated the TCP three-way handshake (packet 31). After packet 31, the destination field was changed to the new server's address.

On investigating packet 21 further, we can see the content that redirected our request to the new server. Expand the Line-based text data section under the HTTP section of the details pane for packet 21. Refer to the following screenshot:

```
Line-based text data: text/html
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">\n
<html><head>\n
<title>302 Found</title>\n
</head><body>\n
<h1>Found</h1>\n
<p>The document has moved <a href="http://192.168.1.103">here</a>.</p>\n
<hr>\n
<address>Apache/2.2.22 (Debian) Server at 192.168.1.104 Port 80</address>\n
</body></html>\n
```