![](bbox=[0.195, 0.082, 0.805, 0.428])

Notice how the syntax we used created a submenu – the slash is used as a separator. We could group Ping, Traceroute, and Top CPU using processes as Diagnostics, add more entries in the Management section, and create a useful toolset. Note that we can also use zabbix_get on the server here and poll individual items that we might not want to monitor constantly. Entries can be nested this way as many times as needed, but beware of creating too many levels. Such mouseover menus are hard to use beyond the first few levels, as it is too easy to make a wrong move and suddenly all submenus are closed.

Regarding the Reboot entry, if it seemed a bit risky to add, fear not – it does not work anyway. First, we had to use sudo for it in the command. Second, we had to configure sudoers to actually allow the running of that command by the zabbix user.

# Reusing global scripts in actions

Some of the global scripts added this way only make sense when used interactively – most of the data gathering or diagnostic ones would probably fall under this category. But our Reboot entry might be reused in action operations, too. Instead of configuring such commands individually in global scripts and each action, we would have a single place to control how the rebooting happens. Maybe we want to change the reboot command to issue a pending reboot in 10 minutes. That way a system administrator who might be working on the system has some time to cancel the reboot and investigate the problem in more detail.