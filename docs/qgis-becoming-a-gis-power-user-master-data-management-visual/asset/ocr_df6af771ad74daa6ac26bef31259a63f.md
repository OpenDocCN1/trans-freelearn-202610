+   9. **GetFeatureInfo** precision is about how close a user has to click to query a location. If you have a lot of data, you probably want this number to be small; but if you have only a few features, making this bigger makes it easier for end users.
+   10. Set **Maximums for GetMap request** if you want to reduce the load on your server by limiting how much data a user can request at once. This is a good idea for a public server. 2560 x 2048, as shown in the screenshot, is enough pixels for an HD-resolution screen to be filled in a single request.
+   11. Next, take a look at the **WFS capabilities** section:

![](bbox=[0.17, 0.29, 0.83, 0.54])

> Only enable WFS if you want users to be able to request vector data as vectors. This can be more intensive than WMS on your bandwidth. Also, do not enable WFS-T features unless you secure your server to only permitted users.

+   1. Check the **Published** box next to any vector layers that you want to be usable over WFS.
+   2. To enable WFS-T, check the **Update**, **Insert**, and **Delete** checkboxes. As they are separate, you can choose to only allow new data (**Insert**), only allow edits to existing data (**Update**), or only allow removal of data (**Delete**). **Insert** would be the safest option as it prevents editing or deletion of existing data.