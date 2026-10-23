# Reversing ControlLogix PLC firmware

Depending on how far the customer wants us to go, reverse engineering automation equipment firmware might be something we need to do. The following is a simple example where we will download a firmware file, extract it, and start reversing it in Ida Pro. Follow these instructions to get started:

+   1.  We will be concentrating on revision 28.012 of an L75 controller. Download the appropriate firmware updater file from the Rockwell download site:

![](bbox=[0.13, 0.255, 0.83, 0.53])

Figure 17.37 – Downloading the 1756-L75 firmware

+   2.  The firmware comes as a `1756-L75_28.012.dmk` file, which can be extracted with binwalk (https://github.com/ReFirmLabs/binwalk). Binwalk is a tool for searching a given binary image for embedded files and executable code.

+   3.  Let’s extract any recognized files in the firmware package with `binwalk -e 1756-L75_28.012.dmk`:

```
```
[...]
root@KALI:~/hacking/PLC$ # ls
[...]
root@KALI:~/hacking/PLC$ # binwalk -e 1756-L75_28.012.dmk
DECIMAL         HEXDECIMAL    DESCRIPTION
89      0x59              Zip archive data, at least v3.0 to extract, name: .reLS/reLS
3756    0xE2A             Zip archive data, at least v3.0 to extract, name: RE.37542.dsr
951     0x3B7             Zip archive data, at least v3.0 to extract, name: RE.37543.dsr
1488    0x5D8             Zip archive data, at least v3.0 to extract, name: package/services/digital-signature/certificate/swypepSigned_charlCclass.cer
2759    0xAC3             Zip archive data, at least v3.0 to extract, name: package/services/digital-signature/.rels/origin.psdord.reLS
9453    0x24D3            Zip archive data, at least v3.0 to extract, name: package/services/digital-signature/.rels/origin.psdord.reLS
1515    0x5E3             Zip archive data, at least v3.0 to extract, name: RE.37533.xps
1541    0x60D             Zip archive data, at least v3.0 to extract, name: RE.37531.bin
1338759 1138759           Zip archive data, at least v3.0 to extract, name: content.text
1338765 1138765           Zip archive data, at least v3.0 to extract, name: package/services/digital-signature/.reLS/mSig/rePldB7o.hzJz10Bm06ng.pdsclks.reLS
1339437 B412D231          Zip archive data, at least v3.0 to extract, name: package/services/digital-signature/.reLS/mSig/rePldB7o.hzJz10Bm06ng.pdsclks.reLS
1287955 1287955           Zip archive data, at least v3.0 to extract, name: RE.37537.bin
2582463 2582463           Zip archive data, at least v3.0 to extract, name: ControlLogix.zip
263834D 263834D           End of Zip archive, footer length: 22
[...]
root@KALI:~/hacking/PLC$ # ls
[...]
-rw------- 1 pac pac 2.0M Mar 21 15:54 1756-L75_28.012.dmk
-rw-r--r-- 1 pac pac 6.0K Mar 21 11:09 1756-L75_28.012.dmk.extracted
```
```

Figure 17.38 – Using binwalk to extract embedded firmware files