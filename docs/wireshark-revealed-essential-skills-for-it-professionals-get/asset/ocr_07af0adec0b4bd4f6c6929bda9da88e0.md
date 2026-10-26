# Getting ready

In these types of problems, you will usually be called on to solve the network is very slow or network has stopped working problems.

Several important facts to remember are:

+   - Broadcasts are not forwarded by routers.
+   - Broadcasts are not forwarded between VLANs (this is why VLANs are called **broadcast domains**), so every VLAN is a single broadcast domain.
+   - Error packets are not forwarded by LAN switches (at least not through the good ones).
+   - Multicasts are forwarded through switches, unless configured otherwise.
+   - Multicasts are forwarded through routers only if the routers are configured to do so.
+   - A reasonable number of broadcasts are transmitted in every network. This is how networks work. Too many broadcasts could be a problem.

# Tip

There is a difference between too many broadcasts and a broadcast storm. Too many broadcasts (for example, a few hundred per second) can load the network but still, in most cases, users will not notice it. Broadcast storms will lock out the network completely.