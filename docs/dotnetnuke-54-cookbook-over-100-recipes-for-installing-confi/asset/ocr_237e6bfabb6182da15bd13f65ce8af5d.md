6. To see the other log message we created, select **Event Viewer** from the **Admin** menu:

![](bbox=[0.171, 0.115, 0.829, 0.347])

# How it works...

The AddLog routine is overloaded to accept different parameters. In this case we passed the employee object as part of the log message. This is very useful for debugging when we want to see the status of an entire object. AddLog will automatically convert the values in the given object to an XML string and display it in the event log.

# There's more...

When using the `EventLogController` there are many types of events you can log. These are color coded in the Log Viewer and you can filter the messages by type. In this recipe, we logged our message using `ADMIN_ALERT` as the event type but there are many more event types you can use. Here are a few examples:

+   - `ADMIN_ALERT`
+   - `HOST_ALERT`
+   - `APPLICATION_START`
+   - `APPLICATION_END`
+   - `USER_CREATED`
+   - `PORTAL_CREATED`
+   - `MODULE_CREATED`
+   - `TAB_CREATED`
+   - `LOGIN_SUCCESS`
+   - `LOGIN_FAILURE`