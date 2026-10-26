# How Wireshark saves timestamps

When packets are captured, Wireshark gives each packet a timestamp derived from the system clock of the machine from where the capture takes place. This timestamp is converted to Universal Coordinated Time (UTC) based on an offset calculated from the time zone setting and any Daylight Savings Time (DST) rules that apply for the capture machine, and then converted again to an epoch number (the UTC-based number of seconds since January 1, 1970). This is the time value that gets saved in the capture file for each packet. When Wireshark reads the capture file, it turns the epoch number back to the familiar date and time display, adjusted for the time zone and DST offsets for your machine.

This means that if a packet capture is conducted on a machine in Los Angeles, which has an offset from UTC of -8 hours, and you look at the same capture file on a machine in Berlin, which is UTC +1 hour (an overall difference of 9 hours, plus any DST differences), a packet that was captured at 10 a.m. local time in Los Angeles will display a timestamp of 7 p.m. in Berlin.

Examples of the timestamp displays in different time zones are shown in the following table:

| | Los Angeles | London | Berlin | Bangalore |
|---|---|---|---|---|
| Capture file time (UTC) | 10:00 | 10:00 | 10:00 | 10:00 |
| Local offset to UTC | -8 | 0 | +1 | +5:30 |
| Displayed time (local time) | 02:00 | 10:00 | 11:00 | 17:30 |

If you're going to look at a packet capture someone has sent you and the absolute time when an event occurred is important to the analysis, you'll need to know or ask what time zone the capture was taken in, determine the offset between your time zone and the capture location time zone, and mentally make the time difference adjustment for the timestamps that Wireshark will display. Otherwise, this difference won't matter as you're usually more interested in the elapsed time or the time between specific events in the capture.