+   3. From the same pop-up panel, select the **Verity Server** tab. Move `wjc-broker` from **Brokers AVAILABLE** to **Brokers SELECTED**. Click **OK** to apply the setting and close the pop-up panel.

This concludes configuring the Domain for CSE, and we'll move on to configuring our single Object Store, Object Store One, `0Store1`.

# ## Configuring an Object Store

Select the **Object Store One** node in the tree, right-click, and select **Properties**. From the pop-up panel, select the **CBR** tab. There is only one field that you can edit on this panel. Set the **CBR Locale** field to **uni**. Click **OK** to apply the setting and close the pop-up panel.

![](bbox=[0.2, 0.37, 0.28, 0.44])

> **uni** stands for Unicode and is generally the best choice for mixed-languages support. If you think you don't need mixed-languages support, there's a pretty good chance that you are mistaken, even if all of your users have the same locale settings in their environments. In any case, if you are tempted to use a different CBR locale, you should first read the K2 locale customization guide, since it's a reasonably complicated topic.

An **Index Area** is the CE's way of referring to CSE locations. Create a Verity Index Area for this Object Store by right-clicking Object Store One and selecting **New | Index Area**. The wizard for creating an index area will appear.

+   1. Click next past the first few panels, and, when prompted, type the name `0Store1 Index Area` as the name of the Index Area.

+   2. On the panel **Enter Verity Directories**, enter values chosen in the earlier section while configuring the CSE via the K2 Dashboard. See the figure below. Notice that it looks like we entered the simple directory name `FileNet_FileSystem_PushAPI` for **Template Type** rather than the full path. Actually, we entered the alias name that we assigned earlier, and it just happens to be the same as the simple directory name.