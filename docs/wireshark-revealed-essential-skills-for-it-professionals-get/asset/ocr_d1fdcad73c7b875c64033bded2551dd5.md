Another condition to look for in the **Expert Info Notes** window includes the **TCP Zero Window** reports, which are caused by a receive buffer on the client or server being too full to accept any more data until the application has time to retrieve and process the data and make more room in the buffer. This isn't necessarily an error condition, but it can lead to substantial delays in transferring data, depending on how long it takes the buffer to get relieved.

You can measure this time by marking the TCP Zero Window packet with a time reference and looking at the elapsed relative time until a **TCP Window Update** packet is sent, which indicates the receiver is ready for more data. If this occurs frequently, or the delay between Zero Window and Window Update packets is long, you may need to inspect the host that is experiencing the full buffer condition to see whether there are any background processes that are adversely affecting the application that you're analyzing.

# Note

If you haven't added them already, you need to add the **Relative time** and **Delta time** columns in the **Packet List** pane. Navigate to **Edit | Preferences | Columns** to add these. Adding time columns was also explained in [Chapter 4](#), Configuring Wireshark.

You will probably see the connection reset (RST) messages in the **Warnings** tab. These are not indicators of an error condition if they occur at the end of a client-server exchange or session; they are normal indicators of sessions being terminated.

A very handy **Filter Expression** button you may want to add to Wireshark is a **TCP Issues** button using this display filter string as follows:

```
tcp.analysis.flags && !tcp.analysis.window_update &&
!tcp.analysis.keep_alive && !tcp.analysis.keep_alive_ack
```

This will filter and display most of the packets for which you will see the messages in the **Expert Info** window and provide a quick overview of any significant issues.

# Detecting and prioritizing delays

Since we're addressing application performance, the first step is to identify any delays in the packet flow so we can focus on the surrounding packets to identify the source and nature of the delay.

One of the quickest ways to identify delay events is to sort a **TCP Delta time** column (by clicking on the column header) so that the highest delay packets are arranged at the