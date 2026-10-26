# # Wireshark time display options

There are a wide variety of packet time displays available for use in Wireshark. By default, Wireshark provides a **Time** column in the **Packet List** pane configured to display **Seconds Since Beginning of Capture** with microsecond resolution (123.123456) for each packet.

However, the way in which time is displayed in this column can be changed by selecting the desired format from the **Time Display Format** option in the **View** menu, which is illustrated in the following screenshot:

![](bbox=[0.073, 0.305, 0.586, 0.644])

If the **Seconds Since Beginning of Capture** option is in use, the first packet in a capture displays a time value of **0.000000**; all other packets are timed in reference to this first packet such that the elapsed time from the beginning of the capture is displayed.

The time display menu options provide examples of their formats and are fairly self-explanatory, except perhaps **Seconds Since Previous Captured Packet** and **Seconds Since Previous Displayed Packet**. The **Seconds Since Previous Captured Packet** option provides the elapsed time between each captured packet, while the **Seconds Since Previous Displayed Packet** option displays the elapsed time from the previous packet that is visible when a display filter is applied.