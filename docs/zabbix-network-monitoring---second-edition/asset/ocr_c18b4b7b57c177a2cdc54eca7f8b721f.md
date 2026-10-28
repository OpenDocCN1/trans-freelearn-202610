Both hosts now have some sort of background. What does this mean?

+   - The round background denotes the trigger status. If any trigger is not in the OK state, the trigger with the highest priority determines the color of the circle.
+   - The square background denotes the host status. Disabled hosts receive gray highlighting. Hosts that are in maintenance receive an orange background.

Click on **A test host**, then click on **Triggers**. In the trigger list, click on **No** in the **ACK** column, enter some message and click on **Acknowledge**. Check the map in the monitoring view again:

![](bbox=[0.076, 0.307, 0.923, 0.538])

The colored circle now has a thick, green border. This border denotes the acknowledgment status—if it's there, the problem is acknowledged.

Zabbix default icons currently are not well centered. This is most obvious when icon highlighting is used—notice how **Another host** is misaligned because of that shadow. For this icon, it's even more obvious in the problem highlighting. In the constructor of the **First map**, click on **A test host** and choose **Default** in the **Problem** dropdown in the **Icons** section. Click on **Apply** and then on **Update** for the map, then check the map in the monitoring section view:

![](bbox=[0.433, 0.748, 0.566, 0.84])