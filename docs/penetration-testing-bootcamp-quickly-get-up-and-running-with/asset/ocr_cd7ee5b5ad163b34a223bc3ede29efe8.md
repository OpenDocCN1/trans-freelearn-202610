```
[i] Will use wget
[i] Using GNU wget: /usr/bin/wget
[i] Configured NVT http feed: http://www.openvas.org/openvas-nvt-feed-current.tar.bz2
[i] Downloading to: /tmp/openvas-nvt-sync.GSNdJ1PXPO/openvas-feed-2017-01-24-11451.tar.bz2
```

Next, you need to make sure that necessary services are working and listening on the correct ports. The last thing you want to do is to start working on it and spend hours troubleshooting a stopped service. There are three services that you need to make sure are working, and they consist of the manager, the scanner, and the GSAD service. Using the netstat command, we will verify that these services are listening before we attempt to connect in:

```
root@kali:/etc/ssh# netstat -antp
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:9390          0.0.0.0:*               LISTEN      23062/openvasmd
tcp        0      0 127.0.0.1:9391          0.0.0.0:*               LISTEN      23030/openvasd: Wa
tcp        0      0 127.0.0.1:80            0.0.0.0:*               LISTEN      23071/gsad
tcp        0      0 127.0.0.1:9392          0.0.0.0:*               LISTEN      23068/gsad
```

If for some reason you don't have these services open, you can start OpenVAS manually through the openvas-start command:

```
root@kali:~# openvas-start
Starting OpenVas Services
root@kali:~# netstat -antp
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:9390          0.0.0.0:*               LISTEN      1692/openvasmd
tcp        0      0 127.0.0.1:9391          0.0.0.0:*               LISTEN      1729/openvassd: Rel
tcp        0      0 127.0.0.1:9392          0.0.0.0:*               LISTEN      1683/gsad
tcp        0      0 127.0.0.1:80            0.0.0.0:*               LISTEN      1707/gsad
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      590/sshd
tcp        0      0 192.168.33.27:22        192.168.33.3:49389      ESTABLISHED 1635/sshd: root@pts
tcp6       0      0 :::22                   :::*                    LISTEN      590/sshd
```