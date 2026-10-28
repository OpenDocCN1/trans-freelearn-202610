# Managing Hosts, Users, and Permissions

Switch to the **Periods** tab and click on **New**. In the **Date** field, set the date and time to the current values:

![](bbox=[0.077, 0.137, 0.915, 0.267])

Click on the small **Add** link—again, that one first, not the **Add** button. Now, switch to the **Hosts & Groups** tab. Make sure the **Group** dropdown in the **Other hosts** section says **Linux servers**, and choose **Another host**. Then, click on the button. Now, click on the large **Add** button. There should be two maintenance entries in the list now:

| NAME | TYPE | ACTIVE SINCE | ACTIVE TILL | STATE | DESCRIPTION |
|---|---|---|---|---|---|
| Maintenance with all data dropped | No data collection | 2016-03-23 00:00 | 2017-03-24 00:00 | Active | We don't need no data |
| Normal maintenance | With data collection | 2016-03-23 00:00 | 2017-03-24 00:00 | Active | We keep data during this maintenance |

Go to **Monitoring | Latest data**, and make sure **Linux servers** is selected in the **Host groups** field in the filter. Notice how data stopped coming in for the items in **Another host**—the timestamp is not being updated anymore. That's because of the maintenance without data collection that we created. As such, triggers will not fire, and problems for such hosts will not appear in the frontend, no matter what the filter settings are.

Let's take a quick look at **Configuration | Hosts**. This is another location where the maintenance status can be seen. Hosts that are in maintenance will have **In maintenance** listed in the **STATUS** column—this replaces the normal **Enabled** text:

| NAME | APPLICATIONS | ITEMS | TRIGGERS | GRAPHS | DISCOVERY | WEB | INTERFACE | TEMPLATES | STATUS |
|---|---|---|---|---|---|---|---|---|---|
| Another host | Applications | Items 12 | Triggers | Graphs | Discovery | Web | 192.168.56.11:10050 |  | In maintenance |

We discovered the way maintenance can affect data collection and the displaying of problems. Another important reason to use it is skipping or modifying notifications. We will discuss notifications in *Chapter 7, Acting upon Monitored Conditions.*