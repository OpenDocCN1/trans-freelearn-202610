allow investigators to perform full filesystem extractions without touching system and user partitions and without making any changes to the device as the exploit runs in memory.

In other words, on selected devices, the checkm8 vulnerability can be exploited to extract the full filesystem *without* actually jailbreaking the device. The following table shows the list of devices that are vulnerable to the checkm8 exploit:

| iPhone 4S | iPhone 5/5c/5s | iPhone 6/6s | iPhone SE |
|---|---|---|---|
| iPhone 7 | iPhone 8 | iPhone X | iPad 2nd–7th gen |
| iPad Mini 2nd and 3rd | iPad Air 1st and 2nd | iPad Pro | Apple Watch 1st–3rd |

Table 1.1 – Devices that are vulnerable to the checkm8 exploit

To exploit checkm8 for a filesystem extraction, your device must be compatible, and it must be running a supported iOS version. This is a major drawback as newer devices, such as the latest iPhone 13, are not supported. There are, however, other options.

In 2020, vendors such as Elcomsoft and Belkasoft introduced agent-based extraction, a new acquisition method that allows full filesystem extractions without jailbreaking the device. Once installed on the device, the agent escapes the sandbox through software exploits, gaining unrestricted access to the device and establishing a connection between the device and the computer. Agent-based extraction is forensically safe, and it is usually a lot faster and safer than most jailbreaks. At the time of writing, supported devices include all iPhones from the 5S up to the iPhone 12, running iOS versions 9.0 to 14.3.

In May 2020, a major update for the **unc0ver** jailbreak was released, adding support for devices based on A12-A13 chips. At the time of writing, unc0ver supports jailbreaking all devices from the iPhone 5s up to the iPhone 12. Supported iOS versions range from iOS 11 to iOS 14.3.

Although jailbreaking a device allows full filesystem extraction, it's not considered a forensically sound process. An investigator should consider