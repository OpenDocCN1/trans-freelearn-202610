# # Summary

The Statistics menu in Wireshark contains options that can give us insight from a unique perspective. In this chapter, we've discussed features such as Summary, Conversations, Endpoints, and Graphs.

Summary is an informational feature, which offers a granular form of data, filters, and the trace file that you are working with. The Conversations window details data regarding the communication that happens between two or more hosts. The Endpoints dialog gives an overview of the devices connected to the network and communicating. The Protocol Hierarchy window gives an idea about the protocols being used in the communication, that is, it gives us a picture of the distribution of protocols used by the hosts for communication.

Graphs are a pictorial way of representing the statistics regarding packets. We can easily figure out if something is wrong with our network; we can match network performances and troubleshoot general day-to-day problems that occur.

IO graphs tell us the basic status of a network, and let us create filters. Matching network performances and differentiating a specific protocol becomes easy due to these. The Flow graph depicts the flow of data in a column-based manner and creates a simple interface to understand the flow of packets in a network. TCP stream graphs are a couple of types, but their objective is to depict the throughput of our network, that is, to know how much data is traveling over a particular period of time.

Using the Follow TCP Stream option, you can reassemble the packets listed in a raw data form, which can be easily read. There are different options that are available to change the form to ASCII, Hex, and many others.

The Expert Infos dialog tells you the information that can be usual and unusual. All of them are related to your packets; information is generated with the help of protocol dissectiors, which translate the packets to a normal form, and if they find something unusual, then it will be listed in a section and under a category inside the dialog.

Command-line tools also get installed when you install Wireshark. The most common tool used is Tshark, which works in a similar way to Wireshark and tcpdump. It uses the pcap library that is used by other major protocol analyzers. With tshark, you can listen to live networks or work along with an already saved capture file. The Filtering and