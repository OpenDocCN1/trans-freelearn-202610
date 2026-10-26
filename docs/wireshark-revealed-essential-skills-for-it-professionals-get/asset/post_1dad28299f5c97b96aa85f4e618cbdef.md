
## An example capture filter

Though you have a variety of filters available in Wireshark itself, which can give you an overview of the BPF syntax, to access the present filters by default, go to **Capture | Capture Filters** or click on the **Capture Options** button in the main toolbar and then click on **Capture Filter**. From the same window, we have an option to create new filters that we already discussed.

Refer to the following table for sample capture filters:

| Filters | Description |
|---|---|
| host 192.168.1.1 | All traffic associated with host 192.168.1.1 |
| port 8080 | All traffic associated with port 8080 |
| src host 192.168.1.1 | All traffic originating from host 192.168.1.1 |
| dst host 192.168.1.1 | All traffic destined to host 192.168.1.1 |
| src port 53 | All traffic originating from port 53 |
| dst port 21 | All traffic destined to port 21 |
| src 192.168.1.1 and tcp port 21 | All traffic originating from 192.168.1.1 and associated with port 21 |
| dst 192.168.1.1 or dst 192.168.1.2 | All traffic destined to 192.168.1.1 or destined to host 192.168.1.2 |
| not port 80 | All traffic not associated with port 80 |
| not src host 192.168.1.1 | All traffic not originating from host 192.168.1.1 |
| not port 21 and not port 22 | All traffic not associated with port 21 or port 22 |
| tcp | All tcp traffic |
| Ipv6 | All ipv6 traffic |
| tcp or udp | All TCP or UDP traffic |
| host www.google.com | All traffic to and from Google's IP address |
| ether host 07:34:aa:b6:78:89 | All traffic associated with the specified MAC address |

### Note

It is essential to know about the BPF syntax. As and when you get into Wireshark in more detail, you will feel its importance. I would suggest that you practice it once when you are comfortable with the syntax.

## Capture filters that use protocol header values

Capture filters can be created on the basis of offset values present in protocol header fields. The syntax to create such filters looks like `proto[offset:size(optional)]=value`. Here, `proto` is any protocol that you want to filter, `offset` is the position of the corresponding value in the header, `size` is the length of the data you are looking for, and `value` is the data you want to find.

Say, for instance, we want to capture only ICMP reply packets; now, if you observe the following figure, you will note that the ICMP header type is located at the first place and the offset counting starts from 0. So, the offset value will be 0 in this case, and the size of the field is 1 bytes. We have all the required information to create a capture filter, so now, the resulting expression will look like `icmp[0:1]=0`.

![](img/da40f92928403866599198344271d63e_1073_0.png)

Figure 2.9: ICMP reply

Let's try to apply the same to Wireshark; we will then ping www.google.com to check whether it works.

![](img/da40f92928403866599198344271d63e_1074_0.png)

Figure 2.10 : ICMP capture filter

Let's ping www.google.com and check whether it works.

```
charits-MacBook-Pro:~ NotFound$ ping www.google.com
PING www.google.com (74.125.130.104): 56 data bytes
64 bytes from 74.125.130.104: icmp_seq=0 ttl=40 time=350.085 ms
64 bytes from 74.125.130.104: icmp_seq=1 ttl=40 time=559.549 ms
64 bytes from 74.125.130.104: icmp_seq=2 ttl=40 time=282.911 ms
64 bytes from 74.125.130.104: icmp_seq=3 ttl=40 time=420.467 ms
64 bytes from 74.125.130.104: icmp_seq=4 ttl=40 time=311.638 ms
64 bytes from 74.125.130.104: icmp_seq=5 ttl=40 time=539.921 ms
```

Figure 2.11: Browse google.com

As a result, Wireshark will capture only the ICMP reply packets. Using the same technique, you can filter out traffic on the basis of the protocol header value:

![](img/da40f92928403866599198344271d63e_1075_0.png)

The following table lists some sample bytes-based capture filters for TCP and ICMP; try practicing them too:

| Filter | Description |
|---|---|
| icmp[0] = 0 | ICMP request packets |
| icmp[0:1] = 8 | ICMP reply packets |
| icmp[0:1] = 3 | ICMP destination host unreachable packets |
| tcp[13] = 2 | TCP SYN flag packets only |
| tcp[13] = 18 | TCP SYN/ACK flag packets only |
| tcp[13] = 32 | TCP URG flag set packets only |

## Display filters

Display filters are much more flexible and powerful when compared to capture filters. Display filters do not discard any packets; instead, the packets are hidden to make viewing convenient. Discarding packets is not a very effective practice because, once the packets are dropped, they cannot be recovered. When you apply the display filter, only those packets that meet the specification of your filter will be displayed. In the second column of the status bar of the Wireshark window, you will see a number of packets displayed after you apply a filter.

A display filter can be used for a capture file in the **Filter** dialog box located above the **Packet List Pane**. Display filters are more popular than capture filters. The syntax used for display filters can be easily adapted and applied. For new users, a display filter is like a super power that gives you the functionality of hiding inappropriate packets in run-time that do not meet your requirements as per the current scenario.

Display filters can be created on the basis of several different constraints such as the IP address, protocols, port numbers, and header values in specific protocols. There are lot of conditional tools and concatenation operators that can be used to create complex expressions. You can combine different sets of expressions to get more specific sets of packets that we are looking for. Each and every packet shown in the **Packet List Pane** can be filtered using the fields that a packet contains.

Display filters do not delete data; instead, packets are hidden, which can be made visible again once the filter in the **Filter** dialog above the list pane is cleared. For instance, to display only ICMP packets, just enter `ICMP` in the filter dialog and click on **Apply**; it's really simple, isn't? If you want to see all packets again, just click on the **Clear** button and everything will be back to normal.

Wireshark has a very awesome feature that can assist you while creating your filter. Just click on the **Expression** button at the end of the **Filter** dialog box, choose the protocol you want to filter, and specify the value if there is one.

Using the **filter expression** dialog is really easy, and if you are a beginner, then this is a boon for you. Let's learn how to use the expression dialog.

![](img/da40f92928403866599198344271d63e_1077_0.png)

### Figure 2.12 : The filter expression

- 1. As show in the preceding screenshot, click on the Expression button.
- 2. Now, you will be presented with the Expression window like the one shown in the following screenshot:

![](img/da40f92928403866599198344271d63e_1078_0.png)

- 3. For example, if you want to see only packets associated with ip:192.168.1.1, then just scroll down in the Field Name to find IPv4. Then, expand the section and choose the ip.addr option.
- 4. Then, from the Relation box next to it, choose the operator you wish to add in your expression.
- 5. At last, write the IP you are looking for in the Value (IPv4 address) box.
- 6. At last, just click on OK. If you've followed all the steps up to here correctly, then you would be able to see the packets originated from the ip that you mentioned (change 192.168.1.1 to your IP address).
- 7. Below the Value box, there is a Predefined value box that is used when a certain protocol restricts us to use only a specific set of values. You can choose a value form here.
- 8. Below the Predefined Value box, there is a Range box that allows us to enter a range of values such as 1-78, 0-5, 120-255 if the protocol allows the same.

This is one of the easiest ways to create a display filter; there is one more way following which we can also create such filters. Entering filters manually can drastically increase the speed of your work, but it requires a bit more skill than there are in a novice user.

Before we start digging into creating filters manually, I want you to know about a few more things, such as comparison and logical operators. These can be used to create simple and the most complex filters for Wireshark.

The following table lists the comparison operators used to create display filters:

| Operator | Description |
|---|---|
| ==/eq | Equal to |
| !=/ne | Not equal to |
| </lt | Less than |
| <=/le | Less than equal to |
| >/gt | Greater than |
| >=/ge | Greater than equal to |

Next, let's have a look at the logical operators that are used to combine different conditions together. The following table lists all of them:

| Operator | Description |
|---|---|
| AND/&& | The AND logical operator is used when we want both parts of the expression to state true. For example, the ip.src==192.168.1.1 and tcp filters would only display packets originated from ip 192.168.1.1 and associated with the tcp protocol. Only the packets that match both the expressions will be shown. |
| OR/|| | The OR logical operator is used when we just focus on one condition to be true at a time; if both are true, even then it's ok. For example, the port 53 or port 80 filters would display all packets associated with |
