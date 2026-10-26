# There's more...

Delay can come from several sources:

+   - **Coding delay:** This is the delay that comes from the digital processing of the voice signals.
+   - **Handling delay (packetization):** This delay is the time that it takes to build packets and insert voice information into them.
+   - **Serialization delay:** This is the fixed delay that occurs when sending packets over the communication line. This delay depends on packet size and line speed.
+   - **Typical delays (round trip):** This is the delay that you can expect when pinging over a communication line (all the following points refer to unloaded lines):
+     - **Over a LAN:** The delay is less than 1 ms.
+     - **Over a WAN connection:** The delay is 1–2 ms in a short-range connection (up to 250–300 km / 150–190 miles) and about 15–20 ms in long range connections (for example, US coast to coast). In older networks you can add several tens of milliseconds to these numbers.
+     - **For home connections, usually xDSL or CaTV:** The delay is somewhere between 10 and 25 ms.
+     - **For inter-continent connections:** The delay is somewhere between 100 and 200 ms.
+     - **For cellular connections:** The delay ranges from 300 ms to 600 ms for old 2.5G networks (GPRS or CDMA 1X), 120 to 150 ms for 3.0G (UMTS or EVDO), 60 to 100 ms for HSDPA, HSUPA, and HSPA+, and goes down to 20 to 50 ms for LTE networks.
+     - **For satellite communications:** The delay is 500 to 600 ms.

# Tip

The delay over a communication line is the sum of the time that it takes the light signal to cross the distance and time consumed by switching or routing delays on the service provider network. While technologies since the early 2000s (for example, MPLS or Carrier Ethernet) are implemented fast switches and routers, technologies older than 2000 (such as Frame Relay or ATM) have slower switching times and therefore will have higher delays.