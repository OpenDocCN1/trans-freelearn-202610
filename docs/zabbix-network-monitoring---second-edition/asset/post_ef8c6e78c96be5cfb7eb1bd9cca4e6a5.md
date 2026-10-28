
# Summary

First, we learned to monitor web pages based on various parameters, including response time, transfer speed, HTTP return code, and text, contained in the page itself. We also found out about how to set up multiple scenarios and steps in them as well as setting up variables to be used in all steps. As a more advanced example, we logged in to the Zabbix frontend and logged out of it. For that to work, we extracted the session ID and reused it in subsequent steps. With this knowledge, it should be possible to monitor most of the functionality web pages have.

For production systems, there usually will be way more applications, scenarios, and steps. Web monitoring can be used for many different purposes, the most popular being site availability and performance, but there are many different cases one could monitor, including things such as watching the Slashdot front page for a company name and replacing the usual first web page with a more simple one to withstand the coming load—slashdotting—easier.

As a simpler alternative, we also explored web page items on the agent side. They have three features:

- Retrieving full page contents
- Finding out page load time
- Extracting a string from the page using regular expressions

Web scenarios are only available on the server side, while the simpler items are only available on the agent side.

Having mostly concentrated on Linux system monitoring so far, we'll depart from that in the next chapter and look at *Chapter 14, Monitoring Windows*. We'll look at the native agent for Windows, performance counter and **Windows Management Instrumentation (WMI)** monitoring, and service discovery and Windows Event Log monitoring.

The Windows build of the Zabbix agent can be obtained from two official locations—either from the download page at http://www.zabbix.com/download.php, or from the source archive. While the practice of keeping binaries in the sources is not suggested, that's how Zabbix does it and sometimes, we can use it to our advantage. If you installed from source, it might be a good idea to use the Windows agent binary from the same archive so that the versions match. The agent executable is located in the subdirectory bin/win32 or bin/win64—choose the one that is appropriate for your architecture. If you installed from the packages, visit the download page and grab the Windows agent archive—but make sure to use the same or older major version of the Zabbix server. With the agent at hand one way or another, place it in the same directory on the Windows machine. For simplicity, we'll use C:\zabbix this time, but you are free to use any other directory. We will also need the configuration file, so grab the example provided at conf/zabbix_agentd.win.conf if you used the binary from the sources, or from the conf/ directory inside the archive if you downloaded the binaries from the Zabbix website. Place the configuration file in the same directory—there should be two files now. Before we continue with the agent itself, let's figure out whether we need to alter the configuration in any way. Open C:\zabbix\zabbix_agentd.win.conf in your favorite text editor and look for any parameters we might want to change. First, the log file location isn't quite right—it's set to C:\zabbix_agentd.log, so let's change it to read:

```
LogFile=c:\zabbix\zabbix_agentd.log
```

You can use both forward and back slashes on the Windows Zabbix agent daemon command line and in the configuration file.

We have already learned that the server line, which currently reads Server=127.0.0.1, will have to be changed. Replace the 127.0.0.1 part with the IP address of your Zabbix server. And to be sure that active items will work as expected, let's check the Hostname directive—it is set to the Windows host by default and we could leave it like that. Another parameter for active checks was ServerActive. Replace 127.0.0.1 here with the Zabbix server IP address as well and save the file.

If we were to start our agent now, it would automatically register on the Zabbix server, based on the configuration we created in Chapter 12, Automating Configuration. While it would be convenient, we want to test things in a stricter fashion this time—go to Configuration | Actions, switch to Auto registration in the Event source dropdown, and click on Enabled next to Testing registration—that should disable the autoregistration we set up earlier.

Now let's try to start the agent up. Start the Windows `cmd.exe` and execute:

```
C:\zabbix>zabbix_agentd.exe -c c:/zabbix/zabbix_agentd.win.conf
```

> 💡 You might have to prefix the commands with `.`\\` on some versions of Windows.
> 
> If you see no output or another window appears very briefly, you should start the command prompt as the admin user. In recent versions of Windows, the menu entry is called **Command Prompt (Admin)**.

The agent daemon refuses to start up:

```
zabbix_agentd.exe [6348]: use foreground option to run Zabbix agent as console application
```

Let's find out how we can supply the foreground option, then. The agent daemon executable on Windows has additional options that can be passed to it, so execute it in the command prompt (when located in the directory where `zabbix_agentd.exe` resides):

```
C:\zabbix>zabbix_agentd.exe --help
```

Looking at the **Options** section, the foreground parameter is listed there:

| Option | Description |
|---|---|
| `-f --foreground` | Run Zabbix agent in foreground |

Let's try to use that option:

```
C:\zabbix>zabbix_agentd.exe --foreground -c c:/zabbix/zabbix_agentd.win.conf
Starting Zabbix Agent [Windows host]. Zabbix 3.0.0 (revision 58455).
Press Ctrl+C to exit.
```

Looks like the agent is started up. For a quick test, try running the following from the Zabbix server. On the Zabbix server, execute:

```
$ zabbix_get -s <Windows host IP> -k system.cpu.load
0.316667
```

# Monitoring Windows

The agent is running and we can query values from it—looks great. There's one issue, though—we are currently running it in our terminal. If we were to close the terminal, the agent wouldn't run anymore. If the system was rebooted, the agent would not be started automatically. Running the agent in the foreground is nice, but we can also run it as a Windows service. How, exactly? First, stop it by pressing **Ctrl + C**, then look at the `--help` output again. Among all the parameters, we are interested in the **Functions** section this time:

```
Functions:
-i --install     Install Zabbix agent as service
-d --uninstall   Uninstall Zabbix agent from service
-s --start       Start Zabbix agent service
-x --stop        Stop Zabbix agent service
```

> The `--multiple-agents` in the Options section is intended to run multiple agents on the same system as separate Windows services. If used, the service name will include the Hostname parameter value from the specified configuration file in the service name.

The Zabbix agent daemon for Windows includes the functionality to install it as a standard Windows service, which is controlled by the options in this section. Unless you are simply doing some testing, you'll want to properly install it, so let's do that now:

```
C:\zabbix>zabbix_agentd.exe -c c:/zabbix/zabbix_agentd.win.conf -i
```

A confirmation dialog might come up at this time. Click on **Yes**. If you were running the command prompt as an administrative user, installing the service should succeed:

```
zabbix_agentd.exe [6248]: service [Zabbix Agent] installed successfully
zabbix_agentd.exe [6248]: event source [Zabbix Agent] installed successfully
```

If not, this command might fail with the following:

```
zabbix_agentd.exe [3464]: ERROR: cannot connect to Service Manager:
[0x00000005] Access is denied.
```

In this case, you should either run the command prompt as an administrative user, or allow the program to run as the administrative user. To do the second, right-click on `zabbix_agentd.exe`, and choose **Troubleshoot compatibility**. In the resulting window, click on **Troubleshoot program** and mark **The program requires additional permissions**:

![](img/f5313a804532fabacc8d07cc8607eeba_529_0.png)

Click on **Next**, then **Test the program**, and **Next** again. In the final window, choose **Yes**, save these settings for this program, then click on **Close**.

> 💡 If running the agent daemon seems to have no input or shows a window very briefly, use the administrative command prompt as suggested earlier.
