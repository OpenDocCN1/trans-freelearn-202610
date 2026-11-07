![](bbox=[0.21, 0.1, 0.79, 0.49])

6. Once completed, the Kali VM will now boot. Log in with the credentials we used with the Raspberry Pi image:

```
Username:root
Password:toor
```

7. You need to then change the default password that was set to make sure no one can get into it. Open up a terminal within the Kali Linux VM and use the following command:

```
Passwd<INSERTPASSWORDHERE>
```

8. Make sure the software is up to date, like you did for the Raspberry Pi. To accomplish this, you can use the following commands:

```
apt-get update
apt-get upgrade
apt-get dist-upgrade
```

9. Once this is complete, the laptop VM is ready to go.