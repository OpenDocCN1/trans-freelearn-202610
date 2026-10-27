

cracking can be attempted, and this will be covered in detail later on in the chapter.

It's important to understand that since iOS 13, iTunes backups will not contain user-sensitive data (such as call logs, keychain, and Safari history) if encryption is not enabled.

## Locating backup files

An iTunes backup is essentially a folder named with a GUID that contains numerous subfolders and files. The following screenshot should give you an idea of what it looks like:

![](img/0b17a17d4cb56b1c25763a396dd83117_288_0.png)

Figure 11.4 – File structure of an iTunes backup

On Windows machines, iOS backups are typically stored at C:\Users\<user>\AppData\Roaming\Apple Computer\MobileSync\Backup. If, however, the Windows app version of iTunes is installed, local backups will be stored at C:\Users\<user>\Apple\MobileSync\Backup.

On machines running macOS, local backups will be stored at /Users/<user>/Library/Application Support/MobileSync/Backup/.

By examining these locations, the investigator may find multiple backups of the same device, or backups for different devices.

In the root folder of each backup, there are four files of interest:

- Info.plist: This property list contains details from the device such as the IMEI, ICCID, model number, the list of all installed applications, and the timestamp of when the backup was created.
- Manifest.db: The Manifest database is a normal SQLite database, but it will be encrypted if a password was specified for the backup. The main table of interest, Files, contains a list of all files included in a backup and their properties, such as fileID, domain, and path.
- Manifest.plist: The Manifest property list stores information for every application that is installed on the device, such as the path to the app's containers. This file also contains the IsEncrypted key, which indicates whether the backup was encrypted or not, and the WasPasscodeSet key, which tells us whether the device had a passcode.
- Status.plist: This property list contains generic metadata, such as the backup's unique identifier and the timestamp of when it was completed.

Since we now have all the information from a backup, we can start putting the filesystem back together using our forensic tools.

## Analyzing iTunes backups

Once the investigator has located an iTunes backup, the filesystem has to be rebuilt and the files will need to be decrypted. Although this can also be done manually, the easiest solution is to import the backup into any mobile forensic software, such as Cellebrite Physical Analyzer or **Elcomsoft Phone Viewer**, and analyze the data as usual.

To import an iOS backup into Cellebrite Physical Analyzer, follow these steps:

- 1. Launch the tool and from the main menu, choose **File | Open case**.
- 2. Then, in **Case wizard**, click on **Add**.
- 3. Choose **Common source | Backup | iTunes backup**.
- 4. Select the folder that contains the backup and click **Continue**.

Once Cellebrite Physical Analyzer has finished parsing the files, the artifacts can be examined as if they were a normal logical acquisition. The following screenshot shows the correct option within **Case wizard**:

![](img/0b17a17d4cb56b1c25763a396dd83117_290_0.png)

Figure 11.5 – Loading an iTunes backup into Cellebrite Physical Analyzer

Previously, we learned that iTunes backups can be encrypted by setting a custom password. On such occasions, if the password is not known, the

![](img/0b17a17d4cb56b1c25763a396dd83117_293_0.png)

Figure 11.7 – Set up a dictionary attack or a brute-force attack

- 4. You can set additional parameters, such as the password length, common character sets, or custom character sets. Then, click Done.
- 5. Click on Start recovery to begin the process.

From the research that has been carried out, it's recommended to attempt brute-forcing a four-digit password first, followed by a six-digit password and a dictionary attack, as these are the most common and easily crackable.

Unfortunately, there are some cases in which recovering the password to an iTunes backup is simply not possible, or there may even be no local backup available. In these situations, one of the possible courses of action is to attempt to download and extract backups or data synced to iCloud.

The final section of this chapter will discuss iCloud forensics.

## Introducing iCloud forensics

In October 2011, Apple introduced **iCloud**, a cloud-based platform that allows users to store and share files between their devices, and backup their data. iCloud is integrated directly into iOS and is accessible from Windows machines, macOS computers, or directly from the web by browsing to https://www.icloud.com.

From a forensic viewpoint, cloud forensics is arguably the future of mobile forensics as it allows investigators to access data that may not even be stored on the device itself. As the majority of new devices do not (yet) support jailbreaks and full filesystem acquisitions, performing a cloud acquisition is a great alternative.

Before we dive deep into the technical details of extracting data from iCloud, it's important to understand exactly what kind of data we can expect to find, starting with **iCloud backups**.

### iCloud backups

Since the release of iOS 5 in 2011, Apple allows users to back up their devices automatically to their iCloud accounts. These backups are similar to the iTunes backups we discussed in the previous section and include most of the data that is stored on a device, such as application data, device settings, call logs, messages, notes, and Safari history.

The user can elect to include or exclude certain elements from the backup directly from the device by navigating to **Settings | [User] | iCloud | Manage storage | Backups.**

To perform an iCloud backup, there is no need to connect the device to a computer, as the backup process happens automatically during the night when the device is charging and connected to a Wi-Fi network. The process can also be started manually from the device.

Cloud backups are made incrementally to save space and time, and typically, at least two or three backups are retained in the cloud. All data is securely encrypted both in transit and in storage, and the encryption key is stored by Apple in the user's iCloud account. More sensitive data, such as Apple Card transactions and health data, is secured with a stronger form of protection known as **end-to-end encryption**; this data is encrypted using keys derived from information unique to the device, so nobody – not even Apple – has access to this data.

Tip

When an investigation involves a locked device that cannot be acquired using any of the methods described in previous chapters, one of the possible workarounds is to take advantage of iCloud's automatic backup capabilities. Putting the device close to a known Wi-Fi network and charging it for a few hours will trigger the creation of a new backup that can then be downloaded and analyzed, revealing the data stored on the device.

Backups are not the only thing that is stored in iCloud, as iOS also provides automatic syncing capabilities for certain data categories.

### iCloud synced data

Syncing data between iOS devices has become a seamless experience for the user thanks to iCloud. The following is a list of the most common categories that are synced to the cloud:

- Account information
- User settings
- Mail
- Call history
- Messages
- Contacts, notes, and calendars
- Safari history, tabs, and bookmarks
- iCloud Keychain
- Apple Health

The user has a certain degree of control over what data is synchronized to iCloud, as syncing can be enabled on a per-app setting directly from the device. The following screenshot shows iCloud syncing settings, which can be accessed by going to **Settings** | **[User]** | **iCloud** and toggling individual apps in the **Apps using iCloud** section:

