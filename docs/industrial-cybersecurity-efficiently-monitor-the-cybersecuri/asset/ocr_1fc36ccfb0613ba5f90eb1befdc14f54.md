Now that we have an indication of the services that are running on the detected systems on our ICS network, based on guesses by the Nmap tool, we have a clearer picture of what the systems are based on. We did this by examining the presence of well-known ports/services. Next, we are going to start investigating individual systems and services with the aim to find more nuanced details about them. Before we do that, though, I'd like to point out an additional way we can perform service detection that can help us identify what is behind an open port on a system, namely, banner grabbing.

# Banner grabbing – What is running on open ports?

Banners are responses from network services that inform the requestor about the service they are communicating with. The most obvious example of a banner is when you log in to a Telnet or SSH server. The first thing the Telnet or SSH server responds with is its banner. For example, the following is the response of an SSH server when it’s being connected to Telnet (Telnet is a great tool for banner grabbing but should be avoided for any remote access activities due to its lack of encryption):

```
```
> telnet ssh-server1 22
SSH-2.0-OpenSSH_7.2p2 Ubuntu-4ubuntu2.10
—
```
```

The highlighted text in the preceding code is the SSH service banner. You can see that there is a lot of information to be found in this banner, such as the version of the SSH service and the OS the service is running on.

To further illustrate the value of banners, the following example shows three scans that were performed on a system: a default Nmap scan, a service scan, and a scan that used the Nmap script for banner grabbing (Nmap scripts will be discussed in more detail in the upcoming sections). This first scan method is a regular Nmap scan without any scripts defined:

```
```
mac@KVM010101:~$ nmap 172.25.100.222
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-29 15:32 MDT
Nmap scan report for 172.25.100.222
Host is up (0.0000030s latency).
Not shown: 998 closed ports
PORT STATE SERVICE
22/tcp open ssh
80/tcp open http
Nmap done: 1 IP address (1 host up) scanned in 0.14 seconds
```
```