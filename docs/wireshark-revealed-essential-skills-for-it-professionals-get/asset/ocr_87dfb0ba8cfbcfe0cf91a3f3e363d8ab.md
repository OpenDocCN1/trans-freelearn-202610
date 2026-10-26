view and use the time values in the **TCP** section.

# Conversation versus displayed packet time options

The difference between time displays for a conversation versus a displayed packet time option is perhaps subtle but important.

As illustrated previously, if you are using one of the displayed packet time options, the time value shown for a given packet will be the elapsed time since the previous packet was displayed in the **Packet List** pane. This time value option has no useful value until you apply a display filter, after which you can easily see the elapsed time between each packet being displayed with no other mental math or adjustments necessary. This is very useful if you're sequentially filtering, clearing, and viewing more than one conversation using, for example, a **tcp.stream==xx** display filter setting.

If you are not using a display filter, however, there may be packets from multiple conversations displayed in the **Packet List** pane. If you are using one of the conversations time displays, the time value shown for a given packet will be the elapsed time since the previous packet for that conversation, regardless of other packets that may be interspersed and visible between the packet you're looking at and the previous packet in that conversation. This allows a quick perusal of conversation packet times without having to apply a display filter.