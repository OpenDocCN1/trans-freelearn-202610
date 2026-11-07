# Global privileges

Global privileges determine the user's access to all databases. Hence, these are sometimes known as "superuser privileges". A normal user should not have any of these privileges unless there is a good reason.

Of course, if we are really creating a superuser, we will select every global privilege that he/she needs. These privileges are further divided into **Data**, **Structure**, and **Administration** groups.

In our example, **bill** will not have any global privileges.

# Resource limits

We can limit the resources used by this user on this server (for example, the maximum queries per hour). Zero means no limit. We will not impose any resources limits on **bill**.

The following screenshot shows the status of the screen, just before hitting **Go** to create this user's definition (with the fields that are left being set to default):

![](bbox=[0.181, 0.458, 0.818, 0.761])