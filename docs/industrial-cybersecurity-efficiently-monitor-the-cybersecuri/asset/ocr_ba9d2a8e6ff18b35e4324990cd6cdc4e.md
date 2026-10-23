Every result that is returned here is a direct connection to the internal (ICS) network of the organization. All we would have to do to start communicating with the Rockwell equipment is to aim our Factory Talk programming suite or the Python module Pylogix, which we discussed in Chapter 7, Active Security Monitoring (https://github.com/dmroeder/pylogix) at a search result and connect. Now, take a closer look at the results page. Notice a pattern in the statistics on the left-hand side?

# TOP ORGANIZATIONS

|  |  |
|---|---|---|
| Verizon Wireless | 101 |
| TE Data | 19 |
| AT&T Wireless | 14 |
| Bell Canada | 14 |
| Krakr1901 | 10 |

Figure 13.6 – Top organizations are cellular companies

Most of the organizations that the discovered IP addresses belong to are cellular providers! What compounds this issue is that these IP addresses do not show up on anyone’s documentation. An organization with a cellular connection like this is often not aware of the IP address it has, therefore, if an internet exposure check is performed, which I highly recommend doing at least yearly (https://threatgen.com/checking-the-internet-exposure-of-your-ics-using-shodan-io/), devices like these are not checked for because they don’t register as belonging to the organization.

You might recall me rambling about this exact scenario in Chapter 3, The Industrial Demilitarized Zone. I wanted to reiterate the potential impact of lax setups like these. They are truly the low-hanging fruit an attacker will look for first. If your stuff shows up on Shodan.io, you should consider yourself compromised! In general, you should prevent vendors from installing shadow internet connections like these on your industrial network and instead use a dedicated service in the IDMZ as explained in Chapter 3, The Industrial Demilitarized Zone.