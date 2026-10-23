
Simply download the GitHub repository, run pip install -r requirements.txt, and specify your VirusTotal (public) API key in the munin.ini file, which you can get for free by signing up at https://www.virustotal.com/gui/join-us. With the list of unique SHA256 hashes saved into the startup-files_hashes.txt file, we can run munin to verify the list against the VirusTotal database (and others if you define the API key for them) by running the following command:

```
pac@KVM0101011:~/book/munin-master$ python3 munin.py -f ../startup-files_hashes.txt
```

The process will take some time to complete and the results flow onto the screen as they come in:

![](img/c8ffa9959721097701584fcc95101362_550_0.png)

Figure 12.12 – munin running hash lookups

After a while, we stumble upon a known malicious hash:

![](img/c8ffa9959721097701584fcc95101362_551_0.png)

Figure 12.13 – munin found a malicious hash value

To tie this back to an executable file location and a system, we search the domain-computers_startup-apps_20201212.txt file for the hash value. The search finds a single occurrence, tied to the nat-service startup entry on Workstation12:

```
...
WORKSTATION12
...
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
...
    nat-service
    "C:\Windows\System32\backdoor.exe"
    c:\windows\system32\backdoor.exe
    4/14/2010 5:06 PM
    MD5:      5756B26A8494B1374112658ABEDC1C89
    SHA1:     70482989ED88E68F450B3CA134E62FC267B6C9E1
    PESHA1:   5D913696AD765F9FD5568B0FD63A6EF2AFDCB18F
    SHA256:   0F4E17318FC19930A09CDD055974DDE48CE180E861F5FF4F13ADEA5CB11521BC
    PESHA256: 3DE0995290E7F42C5CF5D9465448F593106E2A0E658AF9272B49FCDF4EC18851
    IMPHASH:  B4C6FFF030479AA3B12625BBE67BF4914
...
```

We just found another artifact to add to our list:

**Artifact 8 – Workstation12:** `C:\Windows\System32\backdoor.exe`

This concludes the search for artifacts around potential malicious executables in our environment. I hope you see how powerful this technique is. In a few steps, we go out, collect all startup processes in our environment, filter out the potentially malicious one, verify this with VirusTotal lookups, and pinpoint any potentially malicious content.

Next, we are going to use found artifacts and prove or disprove whether they indicate maliciousness.

## Investigation and forensics of suspicious endpoints

It is time to start answering the question of whether there is malicious executable code running in our environment. For this, we will take a closer look at the interesting artifacts we found throughout this exercise. This is the list of artifacts we have found so far:

- **Artifact 1 – Workstation12:** `c:\windows\system32\wbem\unsecapp.exe`
- **Artifact 2 – Workstation12:** `c:\windows\system32\dllhost.exe`
- **Artifact 3 – Workstation12:** `c:\windows\system32\backdoor.exe`
- **Artifact 4 – Workstation12:** `nat-service c:\windows\system32\backdoor.exe`
- **Artifact 5 – Workstation12:** `fun-service c:\windows\system32\backdoor-v2.exe`
- **Artifact 6 – FT-DIR1:** `C:\PROGRA~2\ROCKWE~1\RSLinx\RSLINX.EXE`
- **Artifact 7 – Workstation1:** `C:\Users\engineer-1\AppData\Local\Microsoft\OneDrive\OneDrive.exe`
- **Artifact 8 – Workstation12:** `C:\Windows\System32\backdoor.exe`

Armed with this list of suspicious locations, we will take our hunt to the suspect systems.

## Using discovered indicators of compromise to search the environment for additional suspect systems

Now that we have narrowed down our list of suspicious artifacts to a single verified malicious executable, backdoor.exe, as was found on Workstation12, let’s see whether we have this executable running or present on other systems.

If we were to just search for the backdoor.exe filename on all the hard drives in the environment, we would miss copies of the malware that have a different name. What we want to do is look for something that makes the executable stand out from the crowd, such as a specific string or sequence of bytes that is unique to that executable, and ideally is difficult to change. This is how, fundamentally, antivirus scanners work. They have a large database of unique strings/byte sequences, called **patterns**. If a specific pattern is found within a file, this would be an indicator that the file is the malware that the pattern was extracted from.

Short of writing our own virus scanner pattern rules, we can use a tool that was designed to run a similar search on hard drives, called YARA. We will discuss YARA next.

## Using YARA to find malicious executables

**YARA** is a tool that can be used to identify files that meet certain conditions and contain certain patterns. YARA is a tool aimed at (but not limited to) malware researchers trying to identify and classify malware samples. YARA allows you to detect malware (or whatever file you are looking for) with the help of textual or binary patterns (descriptions). Each description, or rule, consists of a set of strings and a Boolean expression that determine its logic. You can define the definition in so-called YARA rule files. More information on **YARA**, as well as a download link, can be found here: https://virustotal.github.io/yara/.

Let’s see the tool in action. We will be creating a YARA rule file, test.rule, and entering a test rule to illustrate how YARA finds patterns.

Create a file called test.rule and enter the following content:

```text
rule ExampleRule
{
	strings:
		$my_ascii_string = "secret_to_find"
		// The 'wide' keyword indicates the string is unicode
		$my_unicode_string = "secret to find" wide

		// looking for hex value with wildcards
		$my_hex_string = { FF FE 73 00 65 00 63 00 }

	condition:
		$my_ascii_string or $my_unicode_string or $my_hex_string
}
```

The rule defines three types of patterns: an ASCII string, a Unicode string, and a hex string. Then, the condition statement tells YARA that if any one of those three patterns is found, it should report a positive hit for the file it is scanning.

Let's see this rule in action:

```
pac@KVM0010110 : ~/workdir/yara-rules :
> yara -s -r .\test.rule ..\temp\
ExampleRule ..\temp\mine
0x2:$my_unicode_string: s\x00e\x00c\x00r\x00e\x00t\x00 \x00t\x00o\x00 \x00f\x00i\x00n\x00d\x00
0x0:$my_hex_string: FF FE 73 00 65 00 63 00
```

YARA is called with the following flags:

- -s: To print the strings found
- -r: Scan recursively

If we are looking at the ..\temp\mine file in hexadecimal format via the hexdump utility, we can see the pattern YARA reported on:

```
pac@KVM0010110 : ~/workdir/yara-rules :
> HexDump ..\temp\mine
00: FF FE 73 00 65 00 63 00 - 72 00 65 00 74 00 20 00 | s e c r e t
10: 74 00 6F 00 20 00 66 00 - 69 00 6E 00 64 00 | t o f i n d |
```

That covers our quick introduction to YARA. Let's see it in action to uncover copies of backdoor.exe in our environment.

## Using file strings as an indicator of compromise

In order for this to work, we must find a unique pattern to look for. Let's see whether there are specific strings present in the executable file that we can use:

```
pac@KVM0010110 : ~/workdir\
> strings backdoor.exe

!This program cannot be run in DOS mode.
Rich}E
.text
'.rdata
@.atbu
PAYLOAD:
ExitProcess
VirtualAlloc
KERNEL32.dll
AQAPRQVH1
JJM1
RAQH
AXAX^YZAXAYAZH
XAYZH
ws2_32
09ATI
YPPM1
unMa
j@AYh
KERNEL32.dll
VirtualAlloc
ExitProcess
```

That is a short list of strings for this executable. I suspect the file is packed somehow. We should stay clear of using strings such as VirtualAlloc or ExitProcess for your YARA rule as those will probably trigger hits on a bunch of executables. We will use the j@AYh string for the occasion. It should be unique enough for the job.

Creating a YARA rule out of the string is very straightforward. Just add it as a string definition and set a condition to find that string as is shown here:

```yara
rule backdoorExecutable
{
    strings:
        $string = "j@AYh"
    condition:
        $string
}
```

Save the file as `backdoor.yar`. We can now use the custom rule to find copies of `backdoor.exe`:

```
yara -s -r c:\ c:\workdir\backdoor.yar
```

We run the YARA rule on the `backdoor.exe` file to test whether it works:

```
pac@KVM0010110 : ~\workdir\
> yara -s -r backdoor.yar ./
backdoorExecutable ./backdoor.exe
0x19a2:$string: j@AYh
```

As a final check, when we run the YARA rule on a clean system, it shows that the rule is unique enough not to discover false positives.

We are now ready to start interrogating the environment to see whether we have any other systems with the infection.

## Remote scanning with `yara_scanner` using CrowdResponse

We could use `psexec.exe` and copy over and run YARA and scan all domain computers that way. That is a good option; however, in order to show another method and because reporting capabilities are non-existent in YARA, we will look at using CrowdStrike's **CrowdResponse** instead ([https://www.crowdstrike.com/resources/community-tools/crowdresponse/](https://www.crowdstrike.com/resources/community-tools/crowdresponse/)).
