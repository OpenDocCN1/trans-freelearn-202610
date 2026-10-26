# # Merging trace files with Mergecap

You can use Mergecap to merge two or more trace files into one file. The basic syntax is as follows:

```
mergecap -w <outfile.pcapng> infile1.pcapng infile2.pcapng …
```

For example:

```
mergecap -w merged.pcap source1.pcapng source2.pcapng
source3.pcapng
```

One useful option you sometimes may want to use in Mergecap (and several of the other command-line utilities) is -s <snaplen>. This will truncate the packets at the specified length past the start of each frame, resulting in a smaller file; a typical value for <snaplen> is 128 bytes:

```
mergecap -w merged_trimmed.pcapng -s 128 source1.pcapng
source2.pcapng
```