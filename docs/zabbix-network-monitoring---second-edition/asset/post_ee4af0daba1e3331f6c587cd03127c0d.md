
If your Zabbix installation uses LDAP for user authentication, setting Frontend access to Internal for a user group will make all users in that group authenticate against the internal Zabbix password storage. It is not a failover option—internal authentication will always be used. This is useful if you want to provide access to users that are not in the LDAP directory, or create emergency accounts that you can pull out of a safe when LDAP goes down. Such an approach will not work with HTTP authentication, as it happens before Zabbix gets to decide anything about the authentication backend.

![](img/f5313a804532fabacc8d07cc8607eeba_212_0.png)

- **Enabled**: With a single option, all the users in this group can be disabled or enabled. As the predefined groups might tell you, this is a nice way to easily disable individual user accounts by simply adding them to a group that has this checkbox unmarked. We want our user to be able to log in, so this option will stay marked.
- **Debug mode**: This option gives users access to frontend debug information. It is mostly useful for Zabbix developers. We will discuss debug mode in Appendix A, Troubleshooting.

With the main settings covered, let's switch to the Permissions tab:

![](img/f5313a804532fabacc8d07cc8607eeba_212_1.png)

Now that's more like it! We can finally see controls for various permission levels. There are three sections, labeled READ-WRITE, READ ONLY, and DENY. Each has buttons named Add and Delete selected, which seem to modify the respective permission. Our user had no permissions to see anything, so we will want to add some kind of permissions to the first two boxes. Click on Add below the READ-WRITE box. This opens a new window with some options.

It also provides us with another valuable bit of information. If you look at the window contents carefully, you'll notice something common for all of these entries: they are all host groups. We finally have got the essential information together — in Zabbix, permissions can be set for user groups on host groups only.

Mark the checkbox next to SNMP devices, and click on the Select button.

We can now see SNMP devices added to the READ-WRITE box. Next, click on Add below the READ ONLY box. A popup identical to the previous one will open. This time, mark the checkbox next to the Linux servers entry, and then click on Select.

Now, the READ ONLY box has Linux servers listed. The final form should look like this:

![](img/f5313a804532fabacc8d07cc8607eeba_213_0.png)

Let's look at the Calculated permissions section, just below the controls we used:

![](img/f5313a804532fabacc8d07cc8607eeba_213_1.png)

## Managing Hosts, Users, and Permissions

This view shows effective user rights. We can see what the exact access permissions will look like: which hosts will be allowed read and write access, which will have read only, and for which there will be no access at all. This looks about right, so click on the **Add** button at the bottom. The group will be successfully added, and we will be able to see it in the group list:

| Our users | Users 1 | monitoring_user (monitoring user) | System default | Disabled | Enabled |
|---|---|---|---|---|---|

Let's get back to Browser 2. Navigate to **Monitoring | Latest data**. Click on **Select** next to the **Host groups** field. Great, both of the groups we selected when configuring the permissions are available. Mark the checkboxes next to them and click on **Select**. Then, click on **Filter**. Now, our new user can view data from all the hosts. But we also added write permissions to one group for this user, so what's up with the **Configuration** menu? Let's recall the user-creation process — wasn't there something about user types? Right, we were able to choose between three user types, and we chose **Zabbix user**, which, as discussed, was not allowed to access configuration.

> It is important to keep in mind that, at this time, a Zabbix user that has write access granted will not be able to configure things in the frontend, but they will get write access through the API. This could cause security issues. We will discuss the API in *Chapter 21, Working Closely with Data*.

To continue exploring user permissions, we'll create another, more powerful user. In Browser 1, go to **Administration | Users**, and click on the **Create user** button. Fill in these values:

- **Alias**: Enter advanced_user.
- **Name**: Enter advanced.
- **Surname**: Enter user.
- **Groups**: Click on **Add**, mark the checkbox next to **Zabbix administrators**, and click on **Select**.
- **Password**: Enter a password in both fields. You can use the same password as for `monitoring_user` to make it easier to remember.
- **Refresh**: Enter 60.
- **URL (after login)**: Let's have this user view a different page right after logging in. Event history might do — enter `events.php` here.

Now, switch to the Permissions tab and select Zabbix Admin from the User type dropdown. This is will make quite a large difference, as we will soon see:

![](img/f5313a804532fabacc8d07cc8607eeba_215_0.png)

When done, click on the Add button.

Let's use Browser 2 now. In the upper-right corner, click the logout icon, and then log in as advanced_user. This user will land in the event history page, and this time, we can see the Configuration section. That's because we set the user type to Zabbix Admin. Let's check out what we have available there—open Configuration | Hosts:

![](img/f5313a804532fabacc8d07cc8607eeba_215_1.png)

How could there be no hosts available? We set this user as the Zabbix Admin type. We probably should look at the user list back in Browser 1:

| ALIAS | NAME | SURNAME | USER TYPE | GROUPS | IS ONLINE? | LOGIN | FRONTEND ACCESS | DEBUG MODE | STATUS |
|---|---|---|---|---|---|---|---|---|---|
| Admin | Zabbix | Administrator | Zabbix Super Admin | Zabbix administrators | Yes (2016-03-24 16:01:12) | Ok | System default | Disabled | Enabled |
| advanced_user | advanced | user | Zabbix Admin | Zabbix administrators | No | Ok | System default | Disabled | Enabled |
| guest |  |  | Zabbix User | Guests | No | Ok | System default | Disabled | Enabled |
| monitoring_user | monitoring | user | Zabbix User | Our users, Zabbix administrators | Yes (2016-03-24 16:01:08) | Ok | System default | Disabled | Enabled |

Here, we can easily spot our mistake—we added `advanced_user` to the **Zabbix administrators** group, but we set permissions for the **Our users** group. We'll fix that now, but this time, we'll use the user properties form. Click on **advanced_user** in the **ALIAS** column, and in the resulting form, click on **Add** next to the Groups field. Mark the checkbox next to **Our users**, and then click on **Select**:

![](img/f5313a804532fabacc8d07cc8607eeba_216_0.png)

When done, click on **Update**. In Browser 2, simply refresh the host's **Configuration** tab—it should reveal two hosts, **SNMP device** and **snmptraps**, which **advanced_user** can configure.

Suddenly, we notice that we have granted configuration access to the `snmptraps` host this way, which we consider an important host that should not be messed with and that neither of our two users should have access to anyway. How can we easily restrict access to this host while still keeping it in the **SNMP devices** group?

In Browser 1, navigate to **Configuration | Host groups** and click on **Create host group**. Enter the following details:

- **Group name:** Enter `Important SNMP hosts`
- **Hosts:** Filter the **Other hosts** listbox with the **Group** dropdown to display **SNMP devices**, select `snmptraps`, and then click on the **◀** button

When done, click on **Add**.

Open **Administration | User groups**, click on **Our users** in the **NAME** column, and switch to the **Permissions** tab. In the group details, click on the **Add** button below the **DENY** box. In the resulting window, mark the checkbox next to **Important SNMP hosts**, click on **Select**, and then click on the **Update** button.

Now is the time to look at Browser 2. It should still show the host configuration with two hosts. Refresh the list, and the `snmptraps` host will disappear. After our changes, `advanced_user` has configuration access only to the **SNMP device** host, and there will be no access to the monitoring of the `snmptraps` host at all, because we used deny. For `monitoring_user`, nothing has changed—there was no access to the **SNMP devices** group before.
