From the CrowdStrike site, it is defined as follows:

> CrowdResponse is a modular Windows console application designed
> 
> to aid in the gathering of host information for incident response
> 
> engagements. The tool was originally written to support our CrowdStrike
> 
> Services team during their incident response engagements. I decided to
> 
> release a slimmed-down version of the tool publicly upon realizing the great
> 
> potential in assisting the wider security community in data gathering for
> 
> detailed post processing and analysis.

CrowdResponse has a variety of functionality implemented through modules and is well worth checking out. For this exercise, we will be using the YARA functionality only. The way we are going to leverage this functionality is through Python scripts that use `psexec.exe` to run CrowdResponse on all our domain computers, gather the data, and create a very convenient report detailing any findings.

So, let's get started. Follow these instructions to get the `yara_scanner` Python script installed and running:

+   1. We will be running this from the domain controller of OT-DOMAIN, OT-DC1. Install Python 3 on that server.
+   2. The Python tool in question can be downloaded from `https://github.com/tsale/yara_scanner`. Download and extract it to a temporary directory (workdir).
+   3. Download `psexec.exe` from `https://docs.microsoft.com/en-us/sysinternals/downloads/psexec` and extract in the same directory as the `yara_scanner` script.
+   4. Create the `c:\CrowdResponse` folder, download the CrowdResponse files from `https://www.crowdstrike.com/resources/community-tools/crowdresponse/`, and unpack them in the `c:\CrowdResponse` folder.
+   5. Create a `c:\CrowdResponse\results` subfolder.
+   6. We need to make some minor changes to the `yara_scanner.py` script. They are at the top of the file, highlighted in the output as follows:

```
import subprocess
import argparse
import time

crowdresponseEXE = "C:\\CrowdResponse\\CrowdResponse.exe"
# Specify the directory of the CrowdResponse executable.
```