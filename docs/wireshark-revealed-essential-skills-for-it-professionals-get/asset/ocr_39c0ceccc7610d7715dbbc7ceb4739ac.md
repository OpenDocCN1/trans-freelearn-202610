# How to do it...

+   1. String matching filters comes to check a specific string in the packet header. It comes in the following format:
+   
+       `proto [Offset: bytes]`
+   
+       With this filter we can create filters for strings over IP, TCP, and UDP.
+   2. For IP string-matching filters you can create the following filter:
+   
+       `ip [Offset:Bytes]`
+   3. For matching application data, that is, to look into the application data that is carried by TCP or UDP, the most common uses of it are: `tcp[Offset:Bytes]` Or `udp[Offset:Bytes]`.