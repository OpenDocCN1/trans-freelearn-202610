
b) After opening Process Explorer, unhide the lower panel to reveal the dlls for the selected process by enabling dlls from View | Lower Pane View | DLLs:

![](img/c8ffa9959721097701584fcc95101362_497_0.png)

Figure 11.47 – Process Explorer – show dll pane

c) Click on any process and then enter Ctrl + F (find) and search for ipr ip:

![](img/c8ffa9959721097701584fcc95101362_497_1.png)

Figure 11.48 – Process Explorer – find ipripa.dll

d) As you can see, Process Explorer found a dll named ipripa.dll mapped within the address space of a svchost process with PID 1044.

3. If you recall from the OllyDbg screenshot, OllyDbg displays process IDs in hex, so with 1044 being 0x414 in hexadecimal, we should attach to the process with ID 00000414. It will take some effort for OllyDbg to attach to the service; nothing seems to happen (except for some text scrolling by in the bottom left of the GUI) but the process will eventually attach. As soon as it does, hit F9 or the Run button to resume the process.

## Important note

If you don't do this, the system can become unresponsive because this service process is responsible for many functions related to various parts of the operating system.

4. At this point, we should have OllyDbg attached to the svchost process (this process is hosting several services, including the malicious one we are going to debug), with the process running (indicated by the word Running in the bottom right corner of the GUI):

![](img/c8ffa9959721097701584fcc95101362_498_0.png)

Figure 11.49 – OllyDbg – attached to svchost

5. It is now time to find IPRIPa.dll in memory. Open the memory window of OllyDbg (View | Memory or Alt + M). Now scroll through the output of the memory window and find the .text memory section (the executable part of the dll) for IPRIPa.dll:

![](img/c8ffa9959721097701584fcc95101362_499_0.png)

Figure 11.50 – OllyDbg – svchost memory modules

6. Right-click on the section and choose View in Disassembler to direct the disassembler output to the beginning of the text section and the GUI to switch to the disassembler view:

![](img/c8ffa9959721097701584fcc95101362_499_1.png)

Figure 11.51 – OllyDbg – ipripa.dll start

7. We are going to set a breakpoint at a point in the malware code just before it goes out to retrieve the serve.html file. In order to figure out where we need to place this breakpoint, we switch back to IDA Pro and find the part in the `ServiceMain` subroutine that is responsible for this function:

```
loc_10004566:
xor ebp, ebp
push ebp ; dwFlags
push ebp ; lpszProxyBypass
push ebp ; lpszProxy
push ebp ; dwAccessType
push offset szAgent ; lpszAgent
call ds:InternetOpenA
cmp eax, ebp
mov [esp+28h+hInternet], eax
jnz short loc_10004584
```

```
loc_10004584:
push ebp ; dwContext
push 400000h ; dwFlags
mov ecx, offset szPassword
push 3 ; dwService
push ecx ; lpszPassword
push ecx ; lpszUserName
push 50h ; nServerPort
push offset szServerName ; "very-malicious-website.com"
push eax ; hInternet
call ds:InternetConnectA
mov ebp, eax
test ebp, ebp
jz loc_1000462D
```

Figure 11.52 – Malware – beaconing function location

8. The preceding screenshot shows our interesting location – `loc_10004566`; this is where the parameters for the `InternetOpenA` API call are filled, and it comes right after the `sleep` function at the bottom of `ServiceMain`. Placing a breakpoint here would allow us to interrupt the program just before the beacon goes out. In order to find `loc_10004566` in OllyDbg, we instruct the debugger to go to that location by entering `Ctrl + G` and typing in the memory location (10004566):

![](img/c8ffa9959721097701584fcc95101362_501_0.png)

Figure 11.53 – OllyDbg – Go to beaconing function

9. Once we click OK, the disassembler window will be in the right spot:

![](img/c8ffa9959721097701584fcc95101362_501_1.png)

Figure 11.54 – OllyDbg – Beaconing function location

10. This section of the code should look familiar (compare it to the preceding IDA Pro screenshot). We click F2 or right-click Breakpoint | Toggle to set a breakpoint on execution at this place in the dll. All we need to do now is to wait for the sleep timer to end and the dll to start its beaconing routine, at which point our OllyDbg screen should look like the following:

![](img/c8ffa9959721097701584fcc95101362_502_0.png)

Figure 11.55 – OllyDbg – beaconing function - breakpoint hit

11. Notice the Paused message in the bottom right, indicating the process execution flow is interrupted and we have broken into the malware function. We can now use debugging functions such as single stepping, tracing, viewing of registers, and memory to get a better understanding of the code. F8 (step over) will execute one instruction (the one highlighted by EIP) but will not enter subroutine (follow calls). As an example, after pushing F8, the program will execute line 10004566 - XOR EBP, EBP and the highlight will be on line 10004568 - PUSH EBP, which will be the next instruction executed:

![](img/c8ffa9959721097701584fcc95101362_502_1.png)

Figure 11.56 – OllyDbg – beaconing function – single stepping

12. Hit **F8** a few times, until we land just after the InternetOpenA function:

![](img/c8ffa9959721097701584fcc95101362_503_0.png)

Figure 11.57 – OllyDbg – beaconing function – InternetOpenA function

13. Notice how on the right side, during stepping through the program, the registers change and after the call to InternetOpenA the LastErr section shows the result of the call – `ERROR_SUCCESS`, the call was successful. The registers are the windows into program execution, they store variables, parameters, results, and other information. OllyDbg allows the manipulation of register values, which can help reverse malware.

I am going to reiterate that this exercise isn’t aimed at making you a professional reverse engineer or malware analyst but merely a means to get you familiar with concepts and methods. Find a good book to read on the subject if you are interested in learning more.

14. We will continue to step through the program (*F8*) until we complete the `InternetReadFile` call (which retrieves the `serve.html` file). We can see the call completed successfully and a handle (reference ID) is stored in EAX. When continue stepping through after the retrieval, we can see that the program flow really doesn't do anything with the retrieved `serve.html` file, it just repeats downloading the file over and over:

![](img/c8ffa9959721097701584fcc95101362_504_0.png)

That finishes the debugging section of the malware analysis part of our threat hunting exercise. Next, we will look at a different approach to dynamic analysis, interacting with the malware via network services under our control.

# Fooling the malware into revealing its secrets

As a final exercise, we are going to trick the malware in giving up its secrets. We will be setting up a specialized web server that we configure to respond to the beaconing request with exactly what the malware is requesting, the `serve.html` file. We do this to see whether there are any specifics about the request that we can use as IOCs. Additionally, if during malware analysis we would have run into a situation where the malware didn't go into certain code paths because the file returned from the command and control server didn't have the necessary command, variables, or text, we could set up a crafted response in the same way as we will be doing our fooling session.

For us to be able to trick the malware to connect to us, we need to set up the environment that allows us to trick the malware to connect to a system under our control. We can accomplish this by responding to the DNS request the malware sends out to retrieve the `very-malicious-website.com` query, with the IP of the REMnux machine. We will be doing that in the next section.

# Using pfSense to fake the DNS query

In order to have our lab pfSense firewall that we deployed as part of exercise 2 in Chapter 6, Passive Security Monitoring, send custom DNS responses, we will add a host override entry in the pfSense DNS resolver configuration. Follow along with the instructions to get a host override in place on our pfSense firewall:

1. Log in to the pfSense web portal (https://172.25.100.1/) and navigate to Services – DNS Resolver. Make sure the service is enabled, then scroll down to the bottom where the host and domain override sections are:

![](img/c8ffa9959721097701584fcc95101362_505_0.png)

Figure 11.59 – pfSense – DNS overrides

2. Click on Add in the Host Overrides section and enter the following details to create a custom DNS response:

![](img/c8ffa9959721097701584fcc95101362_505_1.png)

Figure 11.60 – pfSense – add DNS host override
