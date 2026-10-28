Notice how the colors were automatically assigned. When multiple items are added to a custom graph in one go, Zabbix chooses colors from a predefined list. In this case the CPU load and the incoming traffic got very similar colors. Click on the colored rectangle in the COLOR column next to the incoming traffic item and choose some shade of green.

As our graph now has more than just the CPU load, change the Name field to CPU load & traffic. While we're still in the graph editing form, select the Filled region in the Draw style dropdown for both network traffic items, then click on Update. Check the graph in the Monitoring | Graphs section:

![](bbox=[0.07, 0.35, 0.92, 0.61])

Hey, that's quite ugly. Network traffic values are much larger than system load ones, thus even the system load trigger line can be barely seen at the very bottom of the graph. The Y axis labels are not clear either — they're just some "K". Let's try to fix this back in the graph configuration. For the CPU load item, change the Y AXIS SIDE dropdown to Right, then click on Update:

![](bbox=[0.12, 0.79, 0.88, 0.84])

> We could have changed the network traffic items, too. In this case, that would have been two extra clicks, though.