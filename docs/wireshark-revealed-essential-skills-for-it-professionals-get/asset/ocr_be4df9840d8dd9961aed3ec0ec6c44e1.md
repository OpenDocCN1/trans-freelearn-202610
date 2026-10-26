# Wireshark command-line utilities

When you install Wireshark, a range of command-line tools also gets installed, including:

+   - `capinfos.exe`: This prints information about trace files
+   - `dumpcap.exe`: This captures packets and saves to a libpcap format file
+   - `editcap.exe`: This splits a trace file, alters timestamps, and removes duplicate packets
+   - `mergecap.exe`: This merges two or more packet files into one file
+   - `rawshark.exe`: This reads a stream of packets and prints field descriptions
+   - `text2pcap.exe`: This reads an ASCII hex dump and writes a libpcap file
+   - `tshark.exe`: This captures network packets or displays data from a saved trace file

The `Wireshark.exe` file launches the GUI version you're familiar with, but you can also launch Wireshark from the command line with a number of parameters; type `Wireshark -h` for a list of options and/or create shortcuts to launch Wireshark with any of those options.

# Note

It is very helpful to add the Wireshark program directory to your system's `PATH` statement so that you can execute any of the command-line utilities from any working directory.