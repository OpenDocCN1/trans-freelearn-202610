We need to enable auto login so that we do not need to manually log in every Pi once we fire it up. Auto login is enabled by default in the latest versions of the Raspbian operating system, so we don't need to perform the following procedure on the latest version. To enable auto login, exit the configuration menu, but don't reboot yet. To set the auto login, open the inittab file with the following command:

```
sudo nano /etc/inittab
```

Comment out the following line:

```
1:2345:respawn:/sbin/getty --noclear 38400 tty1
```

This is done so that it looks like this:

```
#1:2345:respawn:/sbin/getty --noclear 38400 tty1
```

Then, add the following line:

```
1:2345:respawn:/bin/login -f pi tty1 </dev/tty1 >/dev/tty1 2>&1
```

![](bbox=[0.186, 0.416, 0.813, 0.726])