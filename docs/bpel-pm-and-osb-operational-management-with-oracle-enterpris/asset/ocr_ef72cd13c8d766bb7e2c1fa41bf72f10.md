> This is a view showing all of the monitored Middleware components in the enterprise. Grid Control can discover and manage several types of Application Servers such as Oracle Application Server, WebLogic, WebSphere, and JBoss. There are one or more target types and one or more targets for each piece of middleware that is discovered and managed. For each target, a summary of status with details, alerts, policy violations, CPU usage, and memory usage on the host are tabulated.

3. From the dropdown on the right, add an Oracle WebLogic Server Domain target. Click on Go, as shown in the following screenshot:

![](bbox=[0.1, 0.32, 0.9, 0.57])

4. On the Host step, fill in the fields as shown in the next screenshot:

+   - Pick the Administration Server Host from the flashlight.
+   - Version as 10.x (WebLogic Server version 7.x and above are supported).
+   - Port as 7021 (this is the default admin port).
+   - Administration Server Username as weblogic.
+   - Password as `<Your WebLogic admin password>`
+   - Check the Save as Preferred Credentials box.
+   - Administration Server Home Directory as `/u01/homes/osb_wls/wlserver_10.3/server/lib`.
+   - Leave the other fields blank.
+   - Click on Next.