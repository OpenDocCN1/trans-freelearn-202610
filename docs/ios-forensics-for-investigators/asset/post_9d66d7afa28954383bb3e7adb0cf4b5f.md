
On Windows, you can view a file's metadata by right-clicking on the media and selecting **Properties**. Then, click on the **Details** tab and scroll down to view the EXIF data. The exiftool free tool can also be used for the same purpose. It can be downloaded from http://exiftool.org and is compatible with both Windows and macOS.

On macOS, you can view EXIF simply by opening the file using **Preview**. Once open, click on **Tools** and select **Show Inspector**. Then, click on the **Exif** tab.

On most Linux distributions, the exif command-line tool can be used for this purpose.

By now, you should have a general understanding of what EXIF metadata is, how it can be beneficial to an investigation, and how to analyze it.

## Analyzing user viewing activity

So far, we have focused on analyzing photo and video media assets; however, the investigator may want to understand not only what media was stored on a device but also what media the user viewed.

This may include any of the following:

- Audio/video streamed through Safari or other browsers
- Music played through Apple Music or third-party apps such as Spotify
- Videos played through third-party apps such as YouTube and Netflix

The KnowledgeC.db database, which we discussed in *Chapter 5*, *Pattern-of-Life Forensics*, tracks most of the user's day-to-day activity, including events related to audio or video playback.

The table of interest is the ZOBJECT table, which stores device events, organizing them by *stream name*. Every time iOS detects that the user has initiated media playback, a /media/nowPlaying event is triggered.

## Chapter 10: Analyzing Third-Party Apps

The App Store, which was released in 2008, allows users to download third-party apps on their iOS devices. As of 2021, there were more than 4.5 million apps available for download. Once an app is available on the store, developers can publish updates containing bug fixes or new features that will automatically be installed on the device. This poses a problem from a forensic viewpoint, as many forensic tools aren't able to keep up with the speed at which apps are updated, while some applications are simply not supported altogether.

When a third-party application needs to be analyzed for an investigation, the examiner will most likely have to resort to manually researching, validating, and analyzing the artifacts, and this is exactly what we'll be discussing in this chapter.

We will start by learning how to understand which apps are installed on a device, where they are located, and where the investigator should be looking for any artifacts. Then, we will introduce dynamic application analysis and discuss several tools that can be used for researching and validating purposes. Finally, we'll take an in-depth look at some of the most popular third-party apps, such as Facebook, Twitter, Instagram, YouTube, and others.

In this chapter, we will cover the following topics:

- Introducing iOS applications
- Dynamic application analysis
- Practical third-party applications forensics

### Introducing iOS applications

Figure 10.1 – The data container's directory structure

The Documents folder contains user-created artifacts, the Library folder is used by the application to store its data, settings, and preferences, the tmp folder stores any temporary files, and the SystemData folder is typically empty.

As shown in the previous screenshot, the Applications folder contains a subfolder for each app that is installed on the device; however, since the folders are named by the application's GUID, it's impossible to understand which app is which simply by looking at the directory's structure.

In the following sections, we'll learn how to associate an application with its GUID to understand what apps are installed on a device.

### Identifying installed applications

The applicationState.db database, which is located at /private/var/mobile/Library/FrontBoard/, keeps track of Bundle IDs for any application that is installed on the device, including deleted ones. By querying this database, the examiner can understand not only what apps are currently installed on a device, but also where their artifacts are located.

The database contains three tables of interest:
application_identifier_tab, key_tab, and kvs. The first table contains a list of applications that, at some point in time, were installed on the device, identified by their Bundle ID, also called their application identifier.

The following screenshot shows the table's content, based on our example dataset:

![](img/0b17a17d4cb56b1c25763a396dd83117_250_0.png)

Figure 10.2 – The application_identifier table from applicationState.db

The kvs table contains two columns of interest: application_identifier, which maps the record to the Bundle ID in the previous table, and a column named value, which contains a binary BLOB.

The key_tab table is a guide that describes the content of the BLOB. Delving deep into this table is beyond the scope of this section, but keep in mind that a key_tab value of 1 indicates that the corresponding application is still installed on the device.

Based on this logic, the following SQL query will extract the Bundle IDs of all the applications that are currently installed on the device:

```
SELECT
application_identifier_tab.[application_identifier],
```
```
kvs.[value]
FROM kvs, key_tab,application_identifier_tab
WHERE key_tab.[id] = '1'
AND kvs.[key] = key_tab.[id]
AND application_identifier_tab.[id] = kvs.
[application_identifier]
ORDER BY application_identifier_tab.[id];
```

The result of this query is shown in the following screenshot:

![](img/0b17a17d4cb56b1c25763a396dd83117_251_0.png)

Figure 10.3 – The query prints bundle IDs for all the installed applications

As you can see, the query prints the Bundle IDs for all the installed apps, including those developed by Apple that come pre-installed on the device.

The value column contains a BLOB that we will use in the next section to find where an application stores its data.

### Tracking application GUIDs

The following screenshot shows the binary BLOB displayed through a hex viewer:

Type of data currently in cell: Binary

1061 byte(s)

Figure 10.5 – The BLOB contains a binary PLIST

By looking at the data, it's possible to make out some strings, such as paths and locations.

Now, let's extract the BLOB and view the resulting PLIST. Please refer to Chapter 4, Working with Common iOS Artifacts, to learn how to convert a BPLIST into an XML-based PLIST.

The following screenshot shows the PLIST after the data was converted into XML and then base-64 decoded:

![](img/0b17a17d4cb56b1c25763a396dd83117_254_0.png)

Figure 10.6 – The PLIST contains details related to the app's containers

As you can see, we can extract the path to the app's data container from the PLIST. In our example dataset, if we locate the `/private/var/mobile/Containers/Data/Application/71B2EBD8-B6DF-4118-908C-7B2976A3019E` folder, we will find Instagram's artifacts.

So far, we have learned how to identify what apps are installed on an iOS device and where the data is stored. In the following sections, we'll dive deep into analyzing third-party applications.

### Dynamic application analysis

Third-party applications can be a huge source of evidential artifacts since most apps collect, store, and process a tremendous amount of data from their users. Unfortunately, investigators will most likely have to resort to manually analyzing these applications, as forensic tools cannot possibly support each update of every app. Even if a tool *does* support a certain application, a manual examination should still be carried out to validate the tool's results and to ensure that all the data was parsed correctly.

Often, before the examination can be performed, the examiner will have to do some research on the application of interest to understand how it works, what data it stores, and where it stores it. This entails using a research device to install the app and analyze it.

When analyzing mobile applications, there is no standard process that an investigator should take to examine the data since each application performs differently; in this chapter, we will learn about one of the possible methodologies we can use, known as **dynamic analysis**.

Generally speaking, there are two approaches to application analysis:

- **Static analysis** involves taking an in-depth look at an application's code to identify known functions and API calls that can help determine exactly how the app works.
- **Dynamic analysis** entails running the application in a controlled environment and analyzing the app's behavior to understand how it
