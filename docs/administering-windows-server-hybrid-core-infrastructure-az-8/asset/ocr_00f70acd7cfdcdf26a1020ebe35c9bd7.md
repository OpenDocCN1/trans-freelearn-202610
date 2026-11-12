The VM configuration version can be updated with the following PowerShell cmdlet:

```
`Update-VMVersion \<vmname\>`
```

To update the version, the VM must be turned off. It is not possible to downgrade a VM configuration version once it has been upgraded.

The following VM configuration versions are supported for each Hyper-V host Windows version:

+   - Windows Server 2012 R2, 2016, 2019, and 2022
+   - Windows 10 20H2, Enterprise 2016 LTSC, and 2019 LTSC
+   - Windows 11

For additional reference, the following are some examples of the minimum configuration versions for compatibility with certain Hyper-V requirements:

+   - Hibernation support: Version 9.0
+   - Hot add/remove memory: Version 6.2
+   - Large memory VMs: Version 8.0
+   - Nested virtualization: Version 8.0
+   - PowerShell Direct: Version 6.2
+   - Secure Boot for Linux VMs: Version 6.2
+   - Virtual processor count: Version 8.0
+   - Virtual Trusted Platform Module (vTPM): Version 7.0

The configuration version is important to know since VMs created on earlier Hyper-V hosts may not work on newer Hyper-V hosts with a newer OS version. You may need to move a VM between Hyper-V hosts, and you will want to know that the hosts support the VM’s configuration. A host that doesn’t support the VM configuration version of the VM will not allow that VM to start.

# VM settings

The VM’s configuration and properties settings are grouped into Hardware and Management; they are represented for each generation type, as shown in the following figure: