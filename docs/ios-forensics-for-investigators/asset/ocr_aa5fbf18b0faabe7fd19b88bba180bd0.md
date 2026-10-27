these devices will have to resort to sending the phone to a third-party lab that offers forensic services, such as Cellebrite Advanced Services.

If a device cannot be acquired *directly*, it may still be possible to extract its data *indirectly*, by locating and analyzing the device's **backup**, which may be stored on a host computer or in the **cloud**.

In the next section of this chapter, we'll introduce iTunes backups, and we'll learn how to extract and analyze them.

# Introducing iTunes backups

Investigating a device that is locked by an unknown passcode can quickly bring an investigation to a halt if the iPhone in question is one of the newer models, such as the **iPhone 13** or the **iPhone 12**; however, if the suspect has backed up their device to their desktop or laptop and the investigator has access to this machine, the backup can easily be recovered, extracted, and analyzed.

Another reason why examining backups may be beneficial is that the user may have deleted some data from their mobile device, but that doesn't delete the data that resides within the backup: it's quite common, in fact, that backup files contain data that the user believes no longer exists.

Local backups, also called **iTunes backups**, are essentially a logical acquisition of the device and typically contain contacts, SMS messages, media files, logs, databases, keychains, preferences, configurations, browsing artifacts, and location data, for example. A backup can be created manually by the user using **iTunes**, or the device can be configured to automatically create a new backup when it is plugged into a machine. On macOS computers, iTunes is no longer required, and the backup process can be started directly from **Finder**.

When an iTunes backup is created, iOS allows the user to enhance security by encrypting the backup with a password. Once the user sets the password, all subsequent backups will be encrypted, and the files will only be accessible to the examiner if the password is known. However, password