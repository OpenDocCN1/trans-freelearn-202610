
of options specified. You can refer to it for examples of the topics on **Capture Options**.

![](img/da40f92928403866599198344271d63e_110_0.png)

As seen in the preceding screenshot, the **Capture Options** window displays the available interfaces and their IP addresses and allows you to select one or more of these interfaces to perform the capture. Wireshark can capture from multiple interfaces simultaneously, as well as from virtual interfaces. The primary advantage of starting with the **Capture Interfaces** window is the availability of the packet counters to aid in identifying active interfaces, a feature not available in the **Capture Options** window.

Otherwise, if you know which interface you’ll want to use, you can skip using the **Capture Interfaces** window and start here.

Clicking on the **Manage Interfaces** button in the **Capture Options** window brings up an **Interface Management** window. From the **Local Interfaces** tab, you can select to hide interfaces that you do not wish to see displayed in the **Capture Interfaces** and **Capture Options** windows.

There is an option to quickly enable **Capture on all interfaces** and a **Use promiscuous mode on all interfaces** option that is enabled by default. In most cases, this option should be left enabled so that the chosen interface(s) can capture and save all the packets seen. Otherwise, only the packets that are being sent to the Wireshark workstation's MAC address, broadcast, and/or multicast packets will be seen and captured, which basically negates its usefulness as a capture device. The **Compile selected BPFs** button provides a machine language display of the compiled capture filter, but has no other functional purpose.

## Note

The **Capture Filter** field has a highlighting feature that indicates valid versus invalid filter syntax. A green background indicates a good filter and a red background indicates an invalid or incomplete filter.

## Using capture filters

Capture filters are used to reduce the amount of traffic saved during a packet capture. In practice, capture filters should be used sparingly, if used at all, to help make sure that no packets that are important for an analysis are inadvertently missed because they fall outside the capture filter parameters. Remember that you can always filter out unwanted traffic from a capture, but you can't do anything about missed packets once the capture is finished. If you're unsure about a capture, perform the capture again with a more generous capture filter or none at all.

One scenario where a capture filter is appropriate is when you want to let a capture run for a long period of time. Then, you should filter out as much extraneous traffic as possible to keep capture file sizes under control. However, take care to make sure the capture filter you apply doesn't exclude any traffic that may be useful for the analysis.

It's usually a good idea to do some trial captures when using capture filters to verify that the filter is working as desired before doing the official capture that you want to keep.

## Configuring capture filters

Wireshark provides a **Capture Filter** window that makes it easy to select a preconfigured capture filter, or you can configure your own based on your needs.

Click on the **Capture Filter** button in the **Capture Options** window to open the **Capture Filters** window. From this window, you can select from a number of useful preconfigured capture filters, create a new and unique capture filter for your purposes, or delete unwanted or erroneous filters. Creating a new filter only involves giving the filter a name, entering the capture filter syntax, clicking on **New** to save the filter, and then finally clicking on **OK**. Alternatively, you can click on an existing filter and then click on **New**, which will create a copy of that filter at the bottom of the list that can then be modified for your purposes.

The following screenshot illustrates a typical **Capture Filter** window. In this case, a capture filter that will only allow traffic to and from a specific Ethernet MAC address has been selected:

![](img/da40f92928403866599198344271d63e_113_0.png)

Wireshark's capture filters use a syntax that is known as the **Berkley Packet Filter (BPF)** format, which has legacy roots in the Unix world and is still in use today with packet-level drivers. Note that the syntax used to capture filters in Wireshark differs significantly from the syntax used for display filters.

The default selection of capture filters from the **Capture Filter** window is helpful in providing examples of capture filter syntax. Some additional examples of capture filter syntax and examples of that syntax are outlined in the following table:

| Description | Syntax | Examples |
|---|---|---|
| Filter on an Ethernet MAC address | `ether host xx:xx:xx:xx:xx:xx` | `ether host 00:1c:25:99:db:85` |
| Filter to capture just the traffic from or to a MAC address | `ether src or ether dst` | `ether src 00:1c:25:99:db:85` |
| Filter on an IP address or hostname | `host xxx.xxx.xxx.xxx` | `host 192.168.1.115`<br>`host www.wireshark.org` |
| Filter to capture just the traffic between two IP addresses | `src host and dst host` | `host 192.168.1.115 and host 10.1.1.125` |
| Filter traffic in one direction only between two hosts | `src host ... and dst host ...` | `src host 192.168.1.115 and dst host 10.1.1.125` |
| Filter based on a port number | `port, dst port, and src port` | `port 53` |
| Filter for DNS packets |  | `port 67` |
| Filter for DHCP packets |  |  |
| Filter based on a protocol | `arp, icmp, ip, udp, tcp, http, ipv6, and icmp6` | `http` |
| Filter for HTTP traffic only |  |  |
| Capture filter logical operators | `=, !=, >, <, >=, <=, !, not, &&, and, ||, or` | `not arp and port not 53`<br>`! arp && port ! 53` |
| Filter to exclude ARP and DNS packets |  |  |

More information and examples of capture filters can be found on the Wireshark wiki at [http://wiki.wireshark.org/CaptureFilters](http://wiki.wireshark.org/CaptureFilters) and the protocol-specific capture filter syntax is included in the reference information found at [http://wiki.wireshark.org/ProtocolReference](http://wiki.wireshark.org/ProtocolReference).

## Capture options

The **Wireshark Capture Options** window offers a variety of controls to configure captures to suit a particular need.

### Capturing filenames and locations

Clicking on the **Browse** button on the **File** option allows you to navigate to a chosen directory in which you can store the capture files and enter a filename for the capture files.

When the **File** option is used, Wireshark will append a file number and date-time stamp to the filename you specify and will not provide a file extension. You should specify a `.pcapng` extension in the filename. This is better illustrated with an example.

The user provided directory and filename is `C:\Wireshark\long_capture.pcapng`, and Wireshark will create and save packets to files in the format `C:\Wireshark\long_capture_00001_20140724132952.pcapng`.

If Wireshark is configured to capture to more than one file (this will be discussed later), the file numbers and date-time stamps will be incremented accordingly as the capture progresses, for example, `long_capture_00002_20140724133343.pcapng` and `long_capture_00003_20140724133612.pcapng`.

### Multiple file options

Wireshark can be configured to save packets to multiple files to allow long-term captures, and offers a number of options to control how this is accomplished.

Selecting the **Use multiple files** option causes the appropriate underlying controls to become active as specific options are enabled. You can choose to start a new (next) file when each file reaches a given size and/or after a configurable period.

### Note

Wireshark can become very sluggish or might even crash when working with capture file sizes of much greater than 100 MB, so you should consider this as a good maximum file size.

### Ring buffer
