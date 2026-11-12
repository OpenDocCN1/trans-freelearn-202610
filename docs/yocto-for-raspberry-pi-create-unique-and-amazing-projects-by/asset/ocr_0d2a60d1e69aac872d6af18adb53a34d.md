```
```
service_stop()
{
    echo -n "stopping $DESC: $NAME... "
    start-stop-daemon -K -x $DAEMON
    echo "done."
}

case $1 in
    stop)
        service_stop
        echo ""
        ;;
    start)
        service_start
        echo ""
        ;;
    restart)
        service_stop
        service_start
        echo ""
        ;;
    *)
        usage
        ;;
esac
exit 0
```
```

# Explanation

Let's look at how the code works:

+   - The DAEMON variable specifies the binary to launch (/usr/bin/node)
+   - The NAME variable specifies the script name
+   - The DESC variable contains some information about DAEMON (node)
+   - The ARGS variable contains the arguments to pass to DAEMON (node)

If you want to test this script on the Raspberry Pi, use these commands:

```
```
$ scp server.init root@ip_address_of_rpi:/etc/init.d
$ cd /etc/init.d
$ update-rc.d server.init defaults
```
```

This command stops the application:

```
```
/etc/init.d/server.init stop
```
```

This one restarts it:

```
```
/etc/init.d/server.init restart
```
```