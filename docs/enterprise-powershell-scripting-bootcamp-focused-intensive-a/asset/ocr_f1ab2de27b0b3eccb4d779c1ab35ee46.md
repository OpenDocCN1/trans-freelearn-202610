The system this example was run on had six devices connected, reflected in the output from the cmdlet. This differs from the get-disk cmdlet, and the WMI provider accounts for the empty CD-ROM/DVD drive.

# The DriveType property

The DriveType property represents a numerical value that translates to a type of drive. This value ranges from 0 to 6, depending on the type of physical device connected. The DriveType correlation is as follows:

+   - 0: Unknown device type
+   - 1: Drive does not have a root directory
+   - 2: Removable disk (USB key)
+   - 3: Local disk (hard drive/USB hard drive/virtual drive mount)
+   - 4: Network drive/mapped drive
+   - 5: Compact disk (CD/DVD drive)
+   - 6: RAM drive (memory mapped drive/PE OS drive)

To quickly determine the DriveType property correlation, you can perform the following switch command:

```
```powershell
$disks = get-wmiobject win32_logicaldisk
Foreach ($disk in $disks) {
    $driveletter = $disk.DeviceID
    switch ($disk.DriveType) {
        0 { $type = "Type Unknown." }
        1 { $type = "Doesn't have a Root Directory." }
        2 { $type = "Removable Disk (e.g. USB Key)" }
        3 { $type = "Local Disk (e.g. Hard Drive / USB hard drive / Virtual drive mount)" }
        4 { $type = "Network Drive (e.g. Mapped Drive)" }
        5 { $type = "Compact Disk (e.g. CD/DVD Drive)" }
        6 { $type = "RAM Disk (e.g. Memory Mapped Drive / PE OS Drive)" }
        default { $type = "Unable To Determine Drive Type!" }
    }
    Write-host "Drive: $driveletter | Disk Type: $type"
}
```
```