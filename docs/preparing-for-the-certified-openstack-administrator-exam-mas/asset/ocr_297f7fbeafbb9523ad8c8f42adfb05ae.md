You can log in to the provided environment with the following credentials:

User Name: `admin`

Password: `admin`

Congratulations! You have been properly authenticated by Keystone and have been granted access to the Horizon dashboard overview screen. Figure 3.6 shows you how clicking on the Connect button automatically sends an API request containing your domain, username, and password to the Keystone API endpoint. Once connected you will automatically be scoped to a project to which you belong.

![](bbox=[0.07, 0.29, 0.95, 0.6])

Figure 3.6: Interacting with Keystone by logging into the Horizon dashboard

![](bbox=[0.1, 0.67, 0.18, 0.75])

You may have noticed that the login screen does not have a field for entering the domain name. This is because multi-domain is turned off in the Horizon dashboard. If you are an advanced user and would like to change this, go to `/etc/openstack-dashboard/local_settings.py` and set `OPENSTACK_KEYSTONE_MULTIDOMAIN_SUPPORT` to True. Make sure you restart Apache by running `sudo systemctl restart apache2`.