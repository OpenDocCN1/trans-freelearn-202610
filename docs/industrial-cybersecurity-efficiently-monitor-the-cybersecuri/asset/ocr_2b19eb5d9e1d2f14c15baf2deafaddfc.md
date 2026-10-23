We can spot several additional entries when we compare the baseline against the current snapshot. There are the entries for `symon64.exe` and `ossec-agent.exe`, the executables we installed as part of the Wazuh setup. Then there are new entries for `unseapp.exe`, `dllhost.exe`, `flexsrv.exe`, and `backdoor.exe`. Knowing that `Workstation12` is a Rockwell Programming terminal and has had some updates and additions to the Rockwell Automation FactoryTalk software bundle (that `flexsrv.exe` is part of), we can rule out that entry as being suspicious. This leaves us with the following three artifacts that we should note down as potentially malicious executables:

+   * **Artifact 1 – Workstation12:** `c:\windows\system32\wbem\unseapp.exe`
+   * **Artifact 2 – Workstation12:** `c:\windows\system32\dllhost.exe`
+   * **Artifact 3 – Workstation12:** `c:\windows\system32\backdoor.exe`

The next comparison we will look at shows changes in the programs that start with the Microsoft Windows **Operating System (OS)**.

# ## Services and startup programs

Two common ways malware implements persistence to service reboots on a Windows computer is by using any of the automatic startup locations (run registry keys, startup apps, and so on) or creating a service. We should include these areas of comparison in our search for malicious artifacts.

The **Services** section for the comparison at hand only shows the additional **Symon** and `ossec-agent` entries. Looking at the startup items shows us some interesting new entries though:

![](bbox=[0.15, 0.58, 0.87, 0.66])

Two new items are starting up with Windows, with the first one correlating to the running `backdoor.exe` process we saw earlier. We are going to add these entries to our suspicious artifacts list:

+   * **Artifact 4 – Workstation12:**
+       - `nat-service`
+       - `c:\windows\system32\backdoor.exe`