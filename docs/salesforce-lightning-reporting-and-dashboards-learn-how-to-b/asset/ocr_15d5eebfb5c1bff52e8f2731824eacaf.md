The users with the **Manage Dynamic Dashboards** permission are able to create and edit dynamic dashboards. A dynamic dashboard is marked with **The logged in user selected in View Dashboard As**, and each user will see different data in the dashboard based on their data visibility. This is different from a normal dashboard, where all users see the same data based on one person's data visibility.

For users with the **View My Team's Dashboards** permission, when users open a dynamic dashboard with **Allow dashboard viewers to change this (dynamic dashboard) enabled**, they are able to select another user under their team from the role hierarchy, so all data in the dashboard will be refreshed with the visibility of that user. This includes user data and data shared with them and the data under that user's team. Furthermore, users with the **View All Data** permission will be able to select any users as dashboard viewers in the dynamic dashboard.

# Limitations of the dynamic dashboard

The number of dynamic dashboards you can configure depends on your Salesforce edition; you can have up to five dynamic dashboards for the Enterprise Edition, up to 10 dynamic dashboards for the Unlimited and Performance Editions, and three for the Developer Edition. If you need additional dynamic dashboards, you can reach your account executive to purchase them.

You can't save a dynamic dashboard to a personal folder. The dynamic dashboard must be stored in a folder with shared access.

You cannot schedule a dynamic dashboard; dashboards have to be refreshed manually.

# Hands-on exercise for creating a dynamic dashboard

Let's consider following activities for a hands-on implementation: change a normal dashboard to a dynamic dashboard and let the user choose the running user in the dashboard.