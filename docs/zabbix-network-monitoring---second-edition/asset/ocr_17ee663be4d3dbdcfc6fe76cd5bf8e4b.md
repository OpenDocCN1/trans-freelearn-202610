The most common method is e-mail. If you had an action set up properly when we first configured a fully working chain of item-trigger-action in Chapter 2, Getting Your First Notification, you received an e-mail whenever we started or stopped a service, created the test file, and so on. Let's look at what actions can do in more detail.

# Limiting conditions when alerts are sent

Our previous action, created in Chapter 2, Getting Your First Notification, matched any event, as we had not limited its scope in any way. Now we could try matching only a specific condition. Navigate to Configuration | Actions, then click on Create action.

> The following activities rely on a correctly configured e-mail setup (done in Chapter 2, Getting Your First Notification), and a user group Our users (added in Chapter 5, Managing Hosts, Users, and Permissions).

In the Name field, enter SNMP action. Now switch to the Conditions tab. By default, there are two conditions already added – why so?

![](bbox=[0.07, 0.46, 0.92, 0.73])

The conditions here are added because they are likely to be useful for a new action for most users:

+   - Maintenance status not in maintenance: This condition ensures that during active maintenance, no operations will be performed. It can be safely removed to ignore maintenance. For example, technical experts might want to receive notifications even during active maintenance, but helpdesk members may not.