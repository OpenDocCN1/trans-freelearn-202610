Here, only two fields need to be filled in:

+   - **Name:** Enter `CPU load too high on A test host for last 3 minutes`
+   - **Expression:** Enter `{A Test Host:system.cpu.load.avg(180)}>1`

![](bbox=[0.229, 0.177, 0.771, 0.33])

It is important to get the expression correct down to the last symbol. Once done, click on the **Add** button at the bottom. Don't worry about understanding the exact trigger syntax yet; we will get to that later.

Notice how our trigger expressions refer to the item key, not the name. Whenever you have to reference an item inside Zabbix, it will be done by the item key.

The trigger list should be now displayed, with a single trigger — the one we just created. Let's take a look at what we just added: open **Monitoring | Triggers**. You should see our freshly added trigger, hopefully already updated, with a green **OK** flashing in the **STATUS** column:

![](bbox=[0.076, 0.574, 0.924, 0.629])

You might see **PROBLEM** in the **STATUS** field. This means exactly what the trigger name says — the `CPU load too high on A test host for last 3 minutes`.

Notice the big filter?

![](bbox=[0.076, 0.743, 0.924, 0.91])