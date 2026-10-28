![](bbox=[0.176, 0.084, 0.824, 0.212])

It's complaining that an item that would have to be created based on the LLD item prototype already exists. That is correct; we created an item exactly like that earlier, when we manually added items for interface monitoring.

> If an LLD rule attempts to create items that have already been created, the discovery fails and no items will be created.

The same as always, item uniqueness is determined by the item key, including all the parameters. Unfortunately, there is no way to merge manually configured items with LLD-generated ones. There is also no easy way to keep the collected history. We could change the item key either for the existing item or for the item prototype slightly and keep the manually added item for historic purposes and then remove it later when the new, LLD-generated item has collected enough historical data. In this case, we could apply a small hack to the existing item key. Navigate to **Configuration | Templates**, and click on **Items** next to **C_Template_Linux**. Click on **Incoming traffic on interface eth0** in the **NAME** column. In the properties, make these changes:

+   - **Name:** `Incoming traffic on interface $1 (manual)`
+   - **Key:** `net.if.in[enp0s8,]`

![](bbox=[0.216, 0.656, 0.784, 0.751])

That is, add `(manual)` to the name and a trailing comma inside the square brackets. The first change was not strictly required, but it will allow us to identify these items. The second change does not change anything functionally—as the item will still collect exactly the same information. We changed the item key, though. Even a small change like this results in the key being different, and the discovery rule should be able to create those items now. When done, click on **Update**. Now, make the same changes to the outgoing network traffic item and the loopback interface item.