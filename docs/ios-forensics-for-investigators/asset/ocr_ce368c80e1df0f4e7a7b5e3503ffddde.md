still obtained during a filesystem acquisition, but the examiner is also granted access to raw files stored within the device. This allows investigators to access, for example, databases of all third-party applications without restrictions, and recovery of deleted artifacts can also be attempted.

Although there are many tools that allow you to perform this kind of acquisition, Apple restricts direct access to the device's filesystem, so you will have to find a workaround to gain full access to the device before a full filesystem acquisition can be performed.

So, you may be wondering – how do I access the device filesystem? Is a **jailbreak** required? Although jailbreaking is one of the available options, it is not the only one.

At the time of writing, there are three different solutions to gain access to the device's filesystem:

+   - Using tools that leverage Checkm8, a BootROM exploit
+   - Using an extraction agent
+   - Jailbreaking the device

# Checkm8

The publication of the Checkm8 vulnerability in 2019 by the hacker @axi0mX, which is based on an unpatchable vulnerability present in the BootROM of millions of iOS devices, is arguably the biggest game changer ever in the forensics industry.

Checkm8 is compatible with A5 to A11-based devices; this includes all devices starting from the iPhone 5s up to the iPhone X, as there is no working implementation for 4S hardware.

Before we get into the details, it's useful to understand how the iOS boot procedure works and why the Checkm8 vulnerability cannot be patched.