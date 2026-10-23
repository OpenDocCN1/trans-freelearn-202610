```
#
#start service dns
**start service http**
#start service https
...
#####################################
# service_bind_address
# IP address to bind services to
# Syntax: service_bind_address <IP address>
# Default: 127.0.0.1
**service_bind_address 172.25.100.222**
...
```

4. Save the configuration file and then start the service with the following command (inetsim):

```
remnux@remnux:~$ **inetsim**
INetSim 1.3.2 (2020-05-19) by Matthias Eckert & Thomas Hungerberg
Using log directory: /var/log/inetsim/
Using data directory: /var/lib/inetsim/
Using report directory: /var/log/inetsim/report/
Using configuration file: /etc/inetsim/inetsim.conf
Parsing configuration file.
Configuration file parsed successfully.
=== INetSim main process started (PID 2559) ===
Session ID: 2559
Listening on: 172.25.100.222
Real Date/Time: 2020-10-02 19:39:53
Fake Date/Time: 2020-10-02 19:39:53 (Delta: 0 seconds)
Forking services...
* http_80_tcp - started (PID 2561)
done.
**Simulation running.**
```