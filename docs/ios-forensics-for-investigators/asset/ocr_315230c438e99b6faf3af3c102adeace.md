The following is a list of requirements to use this method:

+   - Supported iOS devices: all devices starting from the iPhone 5s up to iPhone 12.
+   - Supported iOS versions: iOS 9.0 through to iOS 14.3.
+   - Using an **Apple ID** registered in Apple's Developer Program is strongly recommended for installing the agent on the device, as this relieves the need to open internet access on the device.

Currently, *Elcomsoft iOS Forensic Toolkit* and *Belkasoft Evidence Center X* are the only tools that provide agent-based extraction capabilities.

# # Jailbreaking the device

Finally, let's talk about jailbreaks! We've seen how it's possible to extract a full filesystem from an iOS device by exploiting the CheckM8 vulnerability or by performing an agent-based acquisition. However, as we've learned, both these methods have their limitations, and there could be some cases where jailbreaking the device is your only option to perform a filesystem acquisition.

Applying a jailbreak will leave some permanent traces on the device, so this option should only be considered as a last resort and only if you have the legal authority to do so.

At the moment, the most popular jailbreaks are **checkra1n** and **unc0ver**. Both these jailbreaks allow investigators to install third-party tools such as SSH, which will give unrestricted access to the device's filesystem.

In the following table, you can check the compatibility for both these jailbreaks: