# TCP resets and why they happen

During a normal operation, TCP will open a connection with SYN signals, and close the connection with FIN signals. One of the characters of TCP is the possibility to close a connection faster due to a problem or just for better efficiency.

In this recipe we will describe these cases, and how to understand exactly what happens, and if it is a regular condition or something went wrong.