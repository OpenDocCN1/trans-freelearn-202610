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