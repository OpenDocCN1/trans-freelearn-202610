This produces the expected results on the login screen:

![](bbox=[0.25, 0.135, 0.715, 0.415])

Figure 4.36 – Customizing the login screen

However, now there is a problem; we have lost the interface for auth objects. Previously, the admin app discovered the models registered in reviews/admin.py and django.contrib.auth.admin through the auto-discovery process, but now we have overridden this behavior by creating a new AdminSite:

![](bbox=[0.145, 0.555, 0.82, 0.84])

Figure 4.37 – Customized AdminSite is missing Authentication and Authorization