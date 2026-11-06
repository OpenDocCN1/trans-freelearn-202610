We will create a new directory, download the ARM version of BTSync, and extract it as shown in the following command-line snippet:

```
mkdir ~/.btsync && cd ~/.btsync
wget http://download-cdn.getsyncapp.com/stable/linux-arm/BitTorrent-Sync_arm.tar.gz -O btsync_arm.tar.gz
tar -xvf btsync_arm.tar.gz
```

We will create a service file. In /etc/init.d, create the btsync file and copy the following text:

```
#!/bin/sh
# /etc/init.d/btsync
#
# Carry out specific functions when asked to by the system
case "$1" in
start)
    /opt/btsync/bin/btsync --config /opt/btsync/bin/btsync.conf
;;
stop)
    killall btsync
;;
*)
    echo "Usage: /etc/init.d/btsync {start|stop}"
    exit 1
;;
esac

exit 0
```

We need to set correct permissions for the new file:

```
sudo chmod 755 /etc/init.d/btsync
sudo update-rc.d btsync defaults
sudo service btsync start
```

BTSync will now start every time your Raspberry Pi 2 reboots. You can check out the configuration file at /opt/btsync/bin/btsync.conf but the defaults work pretty well.

You can now navigate a browser on the same network to your Pi's address at http://raspberrypi:8888/gui.