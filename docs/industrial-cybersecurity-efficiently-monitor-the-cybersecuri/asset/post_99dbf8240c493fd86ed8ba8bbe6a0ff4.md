
## modbus-cli

modbus-cli is a Ruby application that you can add to Kali Linux with the following command:

```
pac@KVM0101011:~$ sudo gem install modbus-cli
Successfully installed modbus-cli-0.0.14
Parsing documentation for modbus-cli-0.0.14
Done installing documentation for modbus-cli after 0 seconds
1 gem installed
```

Once installed, the command to run the Ruby tool is modbus:

```
pac@KVM0101011:~$ modbus
Usage:
    modbus [OPTIONS] SUBCOMMAND [ARG] ...

Parameters:
    SUBCOMMAND
    [ARG] ...    subcommand arguments

Subcommands:
    read        from the device
    write       to the device
    dump        copy contents of read file to the device

Options:
    -h, --help  print help
```

As you can see, the tool allows us to read and write to a Modbus server. Let's see whether we can read work register 10:

```
pac@KVM0101011:~$ modbus read -help
Usage:
    modbus read [OPTIONS] HOST ADDRESS COUNT

Parameters:
    HOST                        IP address or hostname for the Modbus device
    ADDRESS                     Start address (eg %M100,
```

| %MW100, 101, 400101) | |
| COUNT | number of data to read |
| Options: | |
| -w, --word integers | use unsigned 16 bit |
| -i, --int | use signed 16 bit integers |
| -d, --dword integers | use unsigned 32 bit |
| -f, --float point values | use signed 32 bit floating |
| --modicon (eg. coil: 101, word: 400001) | use Modicon addressing |
| --schneider (eg. coil: %M100, word: %MW0, float: %MF0, dword: %MD0) | use Schneider addressing |
| -s, --slave ID | use slave id (default: 1) |
| -p, --port | use TCP port (default: 502) |
| -o, --output FILE | write results to file |
| -D, --debug | show debug messages |
| -T, --timeout when talking to the slave | Specify the timeout in seconds |
| -C, --connect-timeout TIMEOUT seconds when connecting to TCP socket | Specify the timeout in |
| -h, --help | print help |

The following example shows the modbus command reading MW10:

```
pac@KVM0101011:~$ modbus read 172.25.100.21 %MW10 1
%MW10        34
```

Just like in the packet capture, we successfully read Marker Word 10 (%MW10). Now, let's see whether we can overwrite the register:

```
pac@KVM0101011:~$ modbus write -help
Usage:
    modbus write [OPTIONS] HOST ADDRESS VALUES ...
Parameters:
    HOST                        IP address or hostname for the Modbus device
```

And things are really this easy, because Modbus commands are sent in cleartext over the network and the service doesn’t require any form of authentication or authorization to read and write registers – anyone with the IP address of a Modbus server can attack it.

The next industrial protocol we will look at is **EtherNet/IP (ENIP)**.

## Getting EtherNet/IP information

EtherNet/IP is an industrial protocol that implements the **Common Industrial Protocol (CIP)** on top of the Ethernet communications standard. CIP has been previously implemented over serial communication (for example, DeviceNet), as well as over Coaxial communication media (ControlNet). With EtherNet/IP, the command protocol can now be encapsulated in network frames/packets and can therefore be routed across subnets and even the internet.

By default, EtherNet/IP uses TCP port 44818, so we can grep any systems that use EtherNet/IP with the following command (grep’s -B flag indicates how many lines before the occurrence should be displayed):

```
pac@KVM0101011:~$ cat ics-services.txt | grep 44818 -B5
Nmap scan report for 172.25.100.11
Host is up (0.00051s latency).
Not shown: 65533 closed ports
PORT      STATE SERVICE          VERSION
80/tcp    open  http             GoAhead WebServer
44818/tcp open  EtherNet-IP-2
--
Nmap scan report for 172.25.100.12
Host is up (0.00040s latency).
Not shown: 65533 closed ports
PORT      STATE SERVICE          VERSION
80/tcp    open  http             GoAhead WebServer
44818/tcp open  EtherNet-IP-2
--
Nmap scan report for 172.25.100.23
Host is up (0.00050s latency).
Not shown: 65534 closed ports
PORT      STATE SERVICE          VERSION
44818/tcp open  EtherNet-IP-2
```

Now that we know which systems on our ICS network expose the EtherNet/IP service, we can use our trusted Nmap scanner and use a script to find out about the service running on those three IPs.

## Nmap's enip-info script

Let's see what scripts ship with Nmap that allow us to pull EtherNet/IP (enip) information:

```
pac@KVM0101011:~$ ls /usr/share/nmap/scripts/ | grep -e enip
-rw-r--r-- 1 root 57K Jul 13 03:03 enip-info.nse
```

Now, let's aim that script at the first IP we identified with TCP port 44818 open:

```
pac@KVM0101011:~$ sudo nmap -T3 172.25.100.11 -p 44818 --script enip-info
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-31 16:04 MDT
Nmap scan report for 172.25.100.11
Host is up (0.00076s latency).

PORT     STATE SERVICE
44818/tcp open  EtherNet-IP-2
|   enip-info:
|     type: Communications Adapter (12)
|     vendor: Rockwell Automation/Allen-Bradley (1)
|     productName: 1756-EN2T/B
|     serialNumber: 0x005eae98
|     productCode: 166
|     revision: 5.28
|     status: 0x0030
|     state: 0x03
|_    deviceIp: 172.25.100.11
MAC Address: 00:00:BC:5A:D0:56 (Rockwell Automation)
Nmap done: 1 IP address (1 host up) scanned in 0.41 seconds
```

We can see all kinds of interesting details here. Apparently, `172.25.100.11` is a communications adapter – a `1756-EN2T` series B to be precise. The revision the communications adapter is running on is `5.28`. This information allows us to look up any known vulnerabilities for the system we are investigating. Other information, such as `productCode` and `serialNumber`, allows us to add details to an asset management database.

Running the enip-enum script against 172.25.100.12 resulted in almost identical details, but running the script against 172.25.100.23 revealed some noteworthy information:

```
pac@KVM0101011:~$ sudo nmap -T3 172.25.100.23 -p 44818 --script enip-info
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-31 20:17 MDT
Nmap scan report for 172.25.100.23
Host is up (0.001s latency).

PORT      STATE SERVICE
44818/tcp open  EtherNet-IP-2
| enip-info:
|   type: Programmable Logic Controller (14)
|   vendor: Rockwell Automation/Allen-Bradley (1)
|   productName: 1999-56 Our Test Controller
|   serialNumber: 0x006c061a
|   productCode: 54
|   revision: 20.11
|   status: 0x3160
|   state: 0xff
|_  deviceIp: 0.0.0.0
MAC Address: 00:03:12:0B:02:AC (TRsystems GmbH)

Nmap done: 1 IP address (1 host up) scanned in 0.42 seconds
```

What we are seeing here is Python's cpppo module, which we installed on this machine as part of the lab setup (see Appendix 1), in action. The server service that is running on that machine is completely customizable, so we can return any information we want by specifying so in the cpppo.conf file.

So, we now have all the information we need to be able to search for vulnerabilities and feed them into our asset management system. Before we move on, let's look at how EtherNet/IP suffers from the same vulnerabilities as Modbus and most other industrial communications protocols, including a lack of confidentiality (cleartext protocol) and a lack of authentication (anyone can manipulate data). An attacker can sniff the network to identify key data/tags to use them in their attack. A particularly handy Python module, Pylogix, makes detecting this information even more straightforward by allowing us to ask the controller what tags are in its memory. We are going to look at Pylogix next.

## Pylogix

Pylogix (https://github.com/dmroeder/pylogix) is a Python module aimed at allowing Python scripts to interact with an Allen Bradley ControlLogix PLC. It makes interactions very straightforward and can be used to find out PLC details, find configured tags, and to read and write to any tag. You can create scripts to do all this or use an interactive Python shell to run commands one at a time – well… interactively.

To install Pylogix and IPython, run the following command on the Kali Linux machine:

```
pac@KVM0101011:-$ pip3 install --upgrade pylogix ipython
Defaulting to user installation because normal site-packages is not writeable
Collecting pylogix
  Downloading pylogix-0.7.0-py2.py3-none-any.whl (61 kB)
...
Installing collected packages: pylogix
Successfully installed pylogix-0.7.0
```

Now that Pylogix and IPython have been installed, we will use the two to connect to the Allen Bradley ControlLogix PLC at 172.25.100.11, read all the configured tags for the running program, select an interesting tag, and read and write values to that tag:

```
pac@KVM0101011:-$ ipython
Python 3.8.5 (default, Aug  2 2020, 15:09:07)
Type 'copyright', 'credits' or 'license' for more information
IPython 7.18.1 -- An enhanced Interactive Python. Type '?' for help.

In [1]: from pylogix import PLC

In [2]: comm = PLC()

In [3]: comm.IPAddress = '172.25.100.11'

In [4]: comm.GetTagList()
Out[4]: Response(TagName=None,
Value=[LgxTag(TagName=Program:MainProgram, InstanceID=7162,
SymbolType=104, DataTypeValue=104, DataType=, Array=0,
Struct=0, Size=0 AccessRight=None Internal=None Meta=None
Scope0=None Scope1=None Bytes=None), LgxTag(TagName=Boiler2_
```
