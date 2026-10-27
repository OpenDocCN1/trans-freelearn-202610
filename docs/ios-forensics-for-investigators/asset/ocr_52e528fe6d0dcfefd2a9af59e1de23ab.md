to Checkm8.

To perform a BFU acquisition, the device is initially jailbroken using checkra1n, which doesn't require it to be unlocked. Then, the **keychain** is partially extracted, and a filesystem acquisition is performed using a tool such as the Elcomsoft iOS Forensic Toolkit. Some forensic tools, such as the **Cellebrite UFED**, don't require the device to be jailbroken, as the Checkm8 exploit is performed directly within the tool.

# Performing a BFU acquisition using the Elcomsoft iOS Forensic Toolkit

The following steps describe the process of performing a BFU acquisition using the Elcomsoft iOS Forensic Toolkit:

+   1. First, put the device in recovery mode and start checkra1n.
+   2. Follow the instructions to put the device in DFU mode and apply the temporary jailbreak. Make sure that the device is connected using a USB-A cable, as using a USB-C cable will not work.
+   3. Launch the Elcomsoft iOS Forensic Toolkit and choose the **K – Keychain** option.
+   4. When the tool prompts you for the passcode, skip this step by pressing *Enter*.
+   5. When the process completes, choose the **F – File System** option from the main menu to acquire the device's filesystem.
+   6. When the tool prompts you for the passcode, press *Enter* and wait for the process to finish.

The following screenshot illustrates the process: