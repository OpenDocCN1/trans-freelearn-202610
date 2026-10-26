# Response

+   - As you can see, after the fourth packet, the server acknowledges the client's request to get to the server's web root directory. The server starts transmitting the resource that client requested for. The sixth packet in the list pane is what the client received, a status code followed by a short message, including the content of the resource requested. Refer to the following Figure 4.16 illustrating the HTTP response:

![](bbox=[0.122, 0.253, 0.926, 0.494])

Figure 4.16: HTTP response

+   - As a part of TCP communication, the client will acknowledge every packet sent by the server. It can be seen in the seventh packet that the client is trying to send an ACK for the resource it received.
+   - Let's dissect the response elements for packet number six. The first line consists of three arguments sent in response. They denote the HTTP protocol version in use, the status code (304 in our case, which specifies that the requested resource did not change since the time mentioned in the Date parameter), and finally, a brief description about the status code (Not Modified in our case).
+   - In the third line, the Server parameter mentions the name and version of the web server running. We can see that Apache/2.2.22 is the server that is located at 172.16.136.129.
+   - The fourth and fifth lines state that the server wishes to keep the connection alive. The duration for which the server wishes to do so is also mentioned in the next line of the parameters sent in response to us. Rest of the content is mentioned in the next few lines are some configuration parameters.