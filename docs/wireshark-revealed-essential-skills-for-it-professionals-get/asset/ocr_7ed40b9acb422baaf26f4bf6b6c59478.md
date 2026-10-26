+   - To transfer all the packets to any HTTP server: `tcp.dstport == 80`
+   - To check all the connections opened from a specific host (if in a form of scan, can be a worm!): `ip.src==10.0.0.5 && tcp.flags.syn==1 && tcp.flags.ack==0`
+   - To check all the cookies sent from and to a client: `ip.src==10.0.0.3 && (http.cookie || http.set_cookie)`