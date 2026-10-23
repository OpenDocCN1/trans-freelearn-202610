All very useful information to store as a baseline or snapshot. To make our lives easier when hunting down malicious executables, we need a key piece of information, though: the location the process was started from, the path. The way we can get that detail is through running wmic on the target computer, discussed next.

# Searching for suspicious process executable paths

As we have discussed before, seeing where a process starts from (the executable location/path) can give away malicious applications. We can use a Windows native tool, the Windows Management and Instrumentation Command (wmic.exe), to pull a list of processes and their path via a PowerShell iteration command (foreach):

```
PS C:\workdir> get-adcomputer -filter * | select -ExpandProperty Name | foreach { write $_ ; .\PsExec.exe -nobanner \\$_ wmic process get ProcessID,ExecutablePath }
FT-DIR1
```

| ExecutablePath | ProcessId |
|---|---|
| C:\Windows\system32\winlogon.exe | 488 |
| C:\Windows\system32\lsass.exe | 576 |
| C:\Windows\system32\svchost.exe | 656 |
| C:\Windows\system32\svchost.exe | 708 |
| C:\Windows\system32\LogonUI.exe | 792 |
| C:\Windows\system32\dwm.exe | 804 |
| C:\Windows\system32\svchost.exe | 844 |
| ... |  |

The results should be pasted (or piped) into a text file (> Domain-Computers_Running-processes_20201212.txt).

Time to do some digging for suspicious content. To start, we will search through the processes file (using grep) for uncommon locations process executable files are started from. The first search we perform is on the temp string, to see whether we have any running processes in the environment that started from an executable in a temp directory:

```
pac@KVM0101011:~/book$ grep -i temp Domain-Computers_Running-processes_20201212.txt
pac@KVM0101011:~/book$
```