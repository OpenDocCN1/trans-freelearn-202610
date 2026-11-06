```
sudo mount -t ext4 -v /dev/sda1 /nas/USB001/
cd USB001
sudo mkdir public
sudo mkdir work
```

# Remounting a disk after reboot

The quickest way to get your drives remounted after a reboot is to add a few lines to the fstab file, which controls the file system configuration and can be found at /etc/fstab.

Simply add the following lines. The first part is the partition, and the latter part is the mount point. You need to adjust these into your hardware. The final options are used for automounting. Do not change these options:

```
/dev/sda1 /media/USB001 auto noatime 0 0
```

# Accessing files

We will go over several ways of allowing access to files on the Raspberry Pi, the network, and the Internet. You should choose the method that suits you best, as enabling more than one way makes it easier to compromise your system over the network.

# The FTP service

The File Transfer Protocol specification was originally published in 1971, but we currently use a specification from 1985 that everybody should really start moving away from. FTP uses port 21.

A much newer specification, known as Secure FTP (SFTP), supports the IPv6 and Secure Socket Layer (SSL) encryption. Installing FTP will just be a waste of time as OpenSSL (SSH) comes with built-in support and is enabled by default to use SFTP. SFTP generally uses port 22, which is the same port as SSH.

You should create and use a separate user for the SFTP access. I will demonstrate how to connect to your Raspberry Pi with two popular clients using the root account for simplicity.