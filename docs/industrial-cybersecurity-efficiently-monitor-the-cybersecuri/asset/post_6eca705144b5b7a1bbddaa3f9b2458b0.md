
Computer programs written in a compiled code programming language (C, C++, Delphi, and so on) go through a **compiling process**. This process takes the easily readable source code and compiles it into a form that the targeted processor can understand and execute, assembly code. Assembly code will be further optimized for the target platform by the assembler into machine code, at which point only computers can make sense out of it. Now taking that machine code and turning it back into source code is a difficult process and only specialized tools can do a partial job. The nuances of the original code, such as comments, variable names, and the like, are lost in translation and would have to be rebuilt somehow. Taking the machine code and turning it into assembly code is a much easier job and can be achieved with a disassembler. There are many types of disassembler, implementing various techniques to recover the machine code, and a discussion about the various types and techniques is outside the scope of this book and would probably bore you to death.

To get our feet wet with looking at recovered machine code, we will be discussing the IDA Pro disassembler. You can download a copy of the IDA Pro freeware version from `https://www.hex-rays.com/products/ida/support/download_freeware/`. This version is missing many of the bells and whistles of the full-fledged production but there is still enough for us to get going.

For more information and tutorials on how to use Ida Pro, head on over to `https://resources.infosecinstitute.com/topic/basics-of-ida-pro-2/.`

We are going to install the free edition of IDA Pro on the REMnux VM. Follow along with the instructions to get IDA Pro installed and running:

- 1. Download the Linux version to the VM from the Hex-Rays website and install it by running the following commands:

```
remnux@remnux:~/workdir$ cd ~/Downloads/
remnux@remnux:~/Downloads$ ls
idafree70_linux.run
remnux@remnux:~/Downloads$ chmod +x idafree70_linux.run
remnux@remnux:~/Downloads$ sudo ./idafree70_linux.run
```

- 2. Now follow the instructions of the installer to get IDA Pro installed on the REMnux VM:

![](img/c8ffa9959721097701584fcc95101362_482_0.png)

- 3. We can now run IDA Pro by navigating to /opt/idafree-7.0/ and running the following:

```
remnux@remnux:/opt/idafree-7.0$ ./ida64
```

- 4. This will show the initial screen for IDA and allows us to start using this fantastic tool:

![](img/c8ffa9959721097701584fcc95101362_482_1.png)

In the preceding figure, we can see the four main interface sections of IDA Pro. Area 1 is the functions screen; this is where IDA Pro lists any discovered program subroutines. In area 2, we have an overview graph of the subroutine we are currently looking at; there is a draggable square in this panel that allows us to quickly navigate around the function (subroutine). Area 3, the view, shows the internals of the current subroutine; it shows the code IDA managed to disassemble and all the markup and annotations it placed in there during the disassembly process to help make things clearer. You can change the way the subroutine is displayed by selecting the view as focus (click somewhere in area 3) and hitting the spacebar. This allows us to look at the subroutine in a **function block diagram** versus a **listing** of code:

![](img/c8ffa9959721097701584fcc95101362_484_0.png)

Figure 11.32 – IDA Pro – switch disassembly view

The last area in this GUI, 4, is the output window, where events and information are shown during the interactions with the program. This area is also the script input section. IDA Pro supports both scripting with its own IDC engine as well as Python (although limited to IDC for the freeware version). We will not be using scripts during our brief overview with IDA, but the functionality makes the tool even more versatile.

## How the malware installs

Time to see what this baby can do… The keen reader probably already noticed the functions window, including functions with the names that showed up in our strings and exports analysis before. There are functions called `Install` and `InstallA`. They are as good a place to start our investigation as anywhere.

Looking at these two functions, we see that `InstallA` is merely a wrapper for `Install`:

![](img/c8ffa9959721097701584fcc95101362_485_0.png)

Figure 11.33 – Malware – InstallA routine

And when we inspect the `Install` function, we can see a bunch of code which at first sight might look like a bunch of gibberish. It will take time to get familiar with reverse engineering (malicious) code and I will certainly not be able to make you a pro in this short section; however, in a lot of cases it is enough to be able to find the general pattern in these exercises. A good strategy to get an idea of what an executable’s intentions are is to follow the calls. By looking at the (API) calls an executable makes, you can deduce what it is trying to do. Scrolling through the executable code, we see calls to the registry functions we saw during the strings and imports analysis:

![](img/c8ffa9959721097701584fcc95101362_485_1.png)

Figure 11.34 – Malware – Windows registry functions

What this section tells us is that the subroutine is trying to open the `SOFTWARE\Microsoft\Windows NT\CurrentVersion\Svchost` registry and will quit (throw an exception) if it doesn't exist. The executable is interested in the `netvscs` registry value that sits under the `Svchost` key as that is the purpose of the next part of the code. As you will notice, the code is not that hard to read; you can deduce the purpose of the API call by its name or get more information by doing a quick search online. Additionally, IDA Pro adds comments to parameters and other bits of information around the API calls, helping you see what is used as parameters and variables for the calls. If you come across variables that you want to know the value for (and the hints are truncated), you can double-click on the variable name to be taken to the declaration, as shown for the `SubKey` parameter in the following figure:

![](img/c8ffa9959721097701584fcc95101362_486_0.png)

Figure 11.35 – Malware – viewing string contents

If we use the **follow the calls** techniques throughout the rest of the `Install` subroutine, we can see how the malware tries to create a service by means of the `SCManager` (the services manager interface):

![](img/c8ffa9959721097701584fcc95101362_486_1.png)

Figure 11.36 – Malware – service creation

Notice how the service name (lpServiceName) is tied to the [ebp+Str2] variable, which was set to IPRIP earlier in the program:

![](img/c8ffa9959721097701584fcc95101362_487_0.png)

*Figure 11.37 – Malware – service name variable*

If we look for this service on the infected computer (HMI-2), we see it is installed and running:

```
C:\Documents and Settings\Administrator>sc query iprip

SERVICE_NAME: iprip
        TYPE               : 20  WIN32_SHARE_PROCESS
        STATE              : 4  RUNNING
                             (STOPPABLE, PAUSABLE, ACCEPTS_
SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
C:\Documents and Settings\Administrator>
```

We just found another IOC:

- **IOC-5 – Service name:** `IPRIP`

In the next section of the subroutine, we can see the malware setting service-specific registry values such as name, description, image path (the current location of the dll we are examining), and other service-specific details such as startup type, permissions, and so on. After setting all this, the subroutine exits. If all the code executed successfully, the service is installed with these specifics:

- **Service name:** `IPRIP`
- **Display name:** `Intranet Network Awareness (INA+)`
- **Description:** Depends INA+, collects and stores network configuration and location information, and notifies applications when this information changes.
