
```
GET /poker-client/broadcast.htm HTTP/1.1
Accept: image/gif, image/jpeg, image/pjpeg, image/pjpeg, application/x-shockwave-flash, application/x-ms-application, application/x-ms-xbap, application/vnd.ms-xpsdocument, application/xhtml+xml, application/vnd.ms-excel, application/vnd.ms-powerpoint, application/msword, */*
Referer: http://www.888poker.com/poker-client/promotions.htm
Accept-Language: en-us
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; GTB7.1; Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1) ); .NET CLR 1.1.4322; .NET CLR 2.0.50727; OfficeLiveConnector.1.3; OfficeLivePatch.0.0; .NET CLR 3.0.4506.2152; .NET CLR 3.5.30729; InfoPath1)
Host: www.888poker.com
HTTP/1.1 404 Not Found
Date: Sun, 16 Oct 2011 09:11:58 GMT
Server: Microsoft-IIS/6.0
srv: 2344432
```

3. You can see the following conditions:

    - I tried to browse the URI /poker-client/broadcast.htm (marked as 1 and 3 in the preceding screenshot)
    - The URI was forwarded by the referrer:
      http://www.888poker.com/poker-client/promotions.htm (marked as 2 in the preceding screenshot)
    - The status code was 404 Not Found (marked as 4 in the preceding screenshot)

Just to clarify things, I was not playing Poker, I was working on a networking problem.

## Server errors

| Code | Status | Explanation | What to do |
|------|--------|-------------|------------|
| 500 | Internal server error | The web server encountered an unexpected condition that prevented it from carrying out the client request for access to the requested URL. | Response that is usually caused by a problem in your Perl code when a CGI program is run. |
| 501 | Not implemented | The request cannot be executed by the server. | A server problem. |
| 502 | Bad gateway | The server you're trying to reach is sending back errors. | A server problem. |
| 503 | Service unavailable | The service or file that is being requested is not currently available. | A server problem. |
| 504 | Gateway timeout | The gateway has timed out. This message is like the 408 timeout error, but this one occurs at the gateway of the server. | Server is down or nonresponsive. |
| 505 | HTTP version not supported | The HTTP protocol version that you want to use for communicating with the server is not supported by it. | Server does not support the HTTP version. |

You can get service unavailable (code 503) status due to various reasons. In the following example there is a small office that has the following complaint: they can browse Facebook, but the moment they click on a link on this site, they get the new page as blocked. In the following screenshot, you can see that the problem was simply a firewall that blocked it (obviously).

![](img/da40f92928403866599198344271d63e_779_0.png)

### How it works...

In standard HTTP browsing, you should see a very simple pattern as follows:

1. TCP opens the connection (three-way handshake).
2. HTTP sends a `GET` command.
3. Data is downloaded to your browser.

**Tip**

In most cases, opening a web page will open multiple connections—in many cases, tens of them. For example, when you open a news page ([www.cnn.com](http://www.cnn.com), [www.foxnews.com](http://www.foxnews.com), [www.bbc.co.uk](http://www.bbc.co.uk)), it opens the main page, news bars, commercials, temperature window, connections to other sites, and more. Don't be surprised if a single page will open nearly a hundred connections, or even more.

In case of a web page that opens multiple connections (as most web pages do), each connection requires a DNS query, response, TCP SYN-SYN/ACK-ACK, and HTTP GET; only then the data will start to appear on your screen.

### There's more...

When you don't see anything in the packet details pane, right-click on a packet and choose **Follow TCP stream**. This will give you a detailed window, (as in the preceding screenshot) which provides you with a lot of data for the connection.

Another tool that is widely used for HTTP is **Fiddler**. It can be found at <http://fiddler2.com/>. Fiddler is a free tool that is planned for HTTP debugging. It is not in the scope of this book.

## Exporting HTTP objects

Exporting HTTP objects is a simple feature for exporting HTTP statistics—websites and files accessed by HTTP.

### Getting ready

To export HTTP objects, choose **File | Export Objects | HTTP.**

### How to do it...

To export HTTP objects, follow these steps:

1. You can use this feature when capture is running, or you can save the captured file. You will get the following window:

    ![](img/da40f92928403866599198344271d63e_784_0.png)

2. From here you can get a list of the websites that were accessed, including the files that were accessed in each one of them. You can see the website, file types, size, and names.

3. You can use the Save As or Save All buttons for saving the data in a file.

4. In the Content Type column, you will see the following contents:
    - Text: text/plain, text/html, text/javascript—if it's a JavaScript, check what it is, it might be a security risk
    - Images: image/jpeg, image/gif, and other types of images—you can open it with a viewer
    - Applications: application/json, application/javascript, and other types of applications
    - Any other text file discovered by Wireshark

**Tip**

For the export HTTP objects feature to work, first go to TCP preferences and enable TCP packets reassembly (allow subdissector to reassemble TCP streams).

You will get a directory with all the objects captured in the capture file. Objects can be pictures (for example, packet 1052 and 1057 in the preceding screenshot), text (packets 1019, 1022, and others in the preceding screenshot), and others.

### How it works...

This feature scans HTTP streams in the currently opened capture file or the running capture, takes reassembled objects such as HTML documents, image files, executable files, and other readable formats, and lets you save them to a disk. The saved objects can then be opened with the proper viewer, or they can be executed in the case of executable files just by clicking on them. This feature can be helpful for various purposes, including eavesdropping and saving objects for backup (for example, files that were sent through e-mails).

### There's more...

You have several pieces of software that perform the same things graphically, some of them are as follows:

- **Xplico** ([http://www.xplico.org/](http://www.xplico.org/))
- **NetworkMiner** ([http://www.netresec.com/?page=NetworkMiner](http://www.netresec.com/?page=NetworkMiner))

**Tip**

When you see an unknown website with an application that you don't know, and a filename that looks suspicious—Google it; it might be a risk (we will get back to this in [Chapter 14, Understanding Network Security](#)).

## HTTP flow analysis and the Follow TCP Stream window

The **Follow TCP Stream** feature that was discussed in brief earlier in the book is a very helpful feature that can help you with in-depth understanding of the TCP flows that are captured when you monitor the network. In this recipe, we will see some of its advantages.

### Getting ready

Port mirror the device or link you want to monitor and start packet capture.

### How to do it...

1. For opening the Follow TCP Stream window, perform the following steps:
2. Right-click on one of the packets in the stream you want to view.
3. The stream you choose is filtered by the Wireshark. You will see this in the display filter bar that will show you the number of stream in the capture. You will get the following window:

    ![](img/da40f92928403866599198344271d63e_790_0.png)

4. You can see the stream details, for example:
    - The GET method (marked as 1 in the preceding screenshot)
    - The requested HOST (marked as 2 in the preceding screenshot)
    - The client type, Mozilla Firefox in this case, (marked as 3 in the preceding screenshot)
    - The referrer, Cisco in this case, (marked as 4 in the preceding screenshot)
    - The HTTP OK response (marked as 5 in the preceding screenshot)
    - The server type (marked as 6 in the preceding screenshot)

5. These are obvious examples. When having problems, or just issues to investigate, you will be able to see many types of parameters here that will indicate the following cases:
