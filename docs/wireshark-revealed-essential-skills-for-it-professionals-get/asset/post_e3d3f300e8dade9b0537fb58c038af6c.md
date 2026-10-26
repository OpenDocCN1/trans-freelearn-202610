
# Configuring IO Graphs with filters for measuring network performance issues

In this recipe we will learn how to use the IO Graph tool and how to configure it for network troubleshooting.

# How to do it...

Run the IO Graph tool and you will get the following window:

![](img/da40f92928403866599198344271d63e_490_0.png)

On the upper part of the window, you will get the graph highlighted as area 1. On the lower-left part, highlighted as area 2, you will get the filters that enable you to configure display filters, which will enable specific graphs. On the right-hand side of the window, highlighted as areas 3 and 4, you will get the **X-Axis** and **Y-Axis** configuration. Let's see what we can configure and how to do it.

## Filter configuration

1. In the filter window, fill in a filter in the display-filter format. Only the packets that pass this filter will be taken into account for this graph. You have five optional filters to configure here.

2. Choose the type of graph you want to present: **Line**, **Impulse**, **FBar**, or **Dot**.

3. Click on the **Graph** button. This is required in order to activate the filter graph. Don't forget it.

# X-Axis configuration

1. Choose a value to enter in **Tick interval:**. The scale can be between 0.001 seconds and 10 minutes.

# Tip

If, for example, we get a peak of 1,000 packets/second when the tick interval X Axis is configured with 1-second intervals, it means that in the last second we've got 1,000 packets. When we change the tick interval for X Axis to 0.1-second intervals, the peak will be different because now we see how many packets were captured in the last 0.1 second.

2. Choose the **Pixels per tick:** value to configure the pixels per tick interval.

3. Mark the **View as time of day** button for choosing the time of day format instead of time since the beginning of capture.

# Y-Axis configuration

1. Choose the value for **Unit:** from **Packets/Tick**, **Bytes/Tick**, **Bits/Tick**, or **Advanced...** for choosing the Y-Axis scale.

2. Choose **Scale:** for the Y Axis. You can choose it to be **Linear** or change it to **Logarithmic**. You can also leave it as **Automatic** or change it to manual values when required.

3. Choose a value for **Smooth:** if you want to see a running average; that is, in every tick interval you will see the average of the past ticks. You can choose values from 4 to 1,024 to smooth the graph.

![](img/da40f92928403866599198344271d63e_493_0.png)

In the second graph, we see the traffic in bits/sec. Here, we see the bandwidth required from the network while using it to connect to the Internet; that is, an asymmetrical bandwidth when most of the traffic is in the download direction.

# There's more...

Let's have a look at another example here. This is an example of a file download in FTP when 10.0.52.164 downloads a file. Again, you can see that in order to get the traffic on the network, we changed Unit: under Y-Axis to Bits/Tick. Packets/Tick is also important and we will see implementations for it in the applications chapters (chapters 7-14) later in the book.

![](img/da40f92928403866599198344271d63e_494_0.png)

# Throughput measurements with IO Graph

IO Graph is a convenient tool for measuring the throughput of a network. Using it, we can measure the traffic and throughput of any predefined filter. In this recipe we will see some examples for measuring the throughput of a network.

# Getting ready

Connect your laptop with Wireshark to a network with a port mirror to the link you want to measure, as you learned in Chapter 1, Introducing Wireshark. Start a new capture or open an existing file, and open the **IO Graphs** tool from the **Statistics** menu.

While measuring the throughput, we can measure the throughput on a communication line between end devices (PC to server, phone to phone, PC to the Internet, and so on) or to a specific application.

![](img/da40f92928403866599198344271d63e_496_0.png)

The process of isolating network problems starts from measuring traffic over a link between end devices on single connections and seeing where it comes from.

Some typical measurements are host-to-host traffic, all the traffic to a specific server, all the traffic to a specific application on a specific server, all the TCP performance phenomena on a specific server, and more.

# How to do it...

In this recipe, we will provide some basic filters for measuring traffic in the network.

## Measuring throughput between end devices

To measure the throughput between end devices, simply configure a display filter between their IP addresses.

For example, to see the traffic between 10.2.10.101 and 10.2.10.240, configure the filter:

```
ip.addr eq 10.2.10.240 and ip.addr eq 10.2.10.240.
```

You can either type the filter in the IO Graph's Filter: box or perform the following steps:

- 1. Place the cursor on a packet in a specific connection.
- 2. Right-click on it and navigate to Conversation filter | IP. The filter string will appear in the upper display filter box.
- 3. Copy the filter string from the upper display filter box to one of the IO Graph Filter: boxes.
- 4. Click on the filter graph button in the IO Graphs window to activate it.

![](img/da40f92928403866599198344271d63e_498_0.png)

## Measuring application throughput

In order to configure the performance measurement of a specific application, you can configure a filter that contains specific port numbers or a specific connection.

There are several ways to isolate an application graph. Here's one of them:

- 1. In the captured data, click on any packet that belongs to the traffic stream. In TCP it will be a specific connection; in UDP it will be just a stream between two IP/Port pairs.
- 2. Right-click on it and choose **Follow TCP stream** or **Follow UDP stream**. You will get `tcp.streameq<number>` or `udp.streameq<number>`. `<number>` is simply the number of the stream in the capture file.
- 3. Copy the string to the filter window in the **IO Graphs** window and you will get the graph of the specific stream.

![](img/da40f92928403866599198344271d63e_499_0.png)

If you want a graph for specific data on the stream, add information to the filter. For example (in the previous illustration):

- `tcp.streameq 2 and tcp.analysis.retransmissions` will give all the TCP retransmissions on the specific stream (indicating, for example, a slow network, errors, or packet loss)
- `tcp.streameq 2 and tcp.analysis.zero_window` will give all the TCP zero window phenomena on the specific stream (indicating a slow end device)

# How it works...

The power of the IO Graph tool comes from the fact that you can configure any display filter and see it as a graph in various shapes and configurations. Any parameter in a packet can be filtered and monitored in this way.

# There's more...

Some examples for parameters that can be monitored are explained in this section.

## Graph SMS usage – finding SMS messages sent by a specific subscriber

- 1. To configure the filter, choose **SMPP (Short Message Peer to Peer protocol)** packets with the command `Submit_SM`. This is the SMPP command that sends the SMS.
- 2. Type `smpp.destination_addr == "phone number"` in the filter. The filter `smpp.destination_addr == "972527098241"` was configured in the example.

## Graphing number of accesses to the Google web page

- 1. Open the **IO Graphs** window. You can do it during the capture to view online statistics or open a saved capture file.
- 2. Configure the filter `http.host contains "<name>"`, in our case, `http.host contains "google"`.
- 3. In the packet list you will see (while configuring the same filter) the information shown in the following screenshot:

![](img/da40f92928403866599198344271d63e_501_0.png)

- 4. In the **IO Graphs** window, you will see the following graph:
