The way the **Displayed Packet** option works is illustrated in the following screenshot. You can see how the **Captured Packet** timestamps continue to increment, while the **Displayed Packet** timestamps show the time since the last displayed packet.

| Captured Packet | Displayed Packet |
|---|---|
| 0.000000 | 0.000000 |
| 0.001000 | 0.001000 |
| 0.002000 | ----------- |
| 0.003000 | 0.002000 |
| 0.004000 | ----------- |
| 0.005000 | ----------- |
| 0.006000 | ----------- |
| 0.007000 | 0.004000 |

The time display precision options in the **Time Display Format** menu are also shown with examples of the display format and are self-explanatory, except for the **Automatic (File Format Precision)** setting, which requires a description.

Wireshark relies on the NIC driver and the capture devices' system clock for packet timestamps. The accuracy of these timestamps in terms of the precision and number of subsecond digits (milliseconds, microseconds, and nanoseconds) will vary, but usually a millisecond resolution is available. This precision value is saved in the capture file. The **Automatic (File Format Precision)** setting tells Wireshark to display timestamps using this precision value.

The ability to use the **Nanoseconds precision** setting depends on having an NIC driver that supports this level of precision. If you select this option and the capture file doesn't contain the higher resolution, the last three digits of each timestamp will be all zeroes.