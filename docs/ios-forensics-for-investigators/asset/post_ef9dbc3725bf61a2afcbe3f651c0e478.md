
From a forensic viewpoint, iOS devices maintain the following:

- A list of *low-energy* Bluetooth devices that can connect to the user's device, also called **paired devices**. These are stored in the `/private/var/containers/Shared/SystemGroup/<GUID>/Library/Database/com.apple.MobileBluetooth.ledevices.paired.db` database, under the `PairedDevices` table. This table maintains a list of devices, their names, their **Media Access Control (MAC)** addresses, and their last-seen timestamps.
- Other Bluetooth paired devices (*not just low-energy*), which are stored in the `/private/var/containers/Shared/SystemGroup/<GUID>/Library/Preferences/com.apple.MobileBluetooth.devices.plist` file. This property list maintains a list of device names and last-detection times.
- A list of *seen* Bluetooth devices, which are other available Bluetooth-enabled devices that were close to the device but that did not connect. These devices can be found in the `/private/var/containers/Shared/SystemGroup/<GUID>/Library/Database/com.apple.MobileBluetooth.ledevices.other.db` database, under the `OtherDevices` table.
- A detailed log of Bluetooth-related events, such as **connection** and **disconnection** events. These are stored in the `knowledgeC.db` database.

As the first three files are trivial to query, we'll concentrate on extracting Bluetooth events from the `/private/var/mobile/Library/CoreDuet/Knowledge/knowledgeC.db` database, which we introduced in *Chapter 5, Pattern-of-Life Forensics.*

First, we must select the start and end dates for the events from the `ZOBJECT` table. Then, we must filter down the results by searching for the `/bluetooth/isConnected` stream. We must extract the event status and device details from the `ZSTRUCTUREDMETADATA` table. Finally, we must order the results chronologically.

The final query looks like this:

```
SELECT
DATETIME(ZOBJECT.ZSTARTDATE + 978307200, 'unixepoch',
'localtime') AS "Start",
DATETIME(ZOBJECT.ZENDDATE + 978307200, 'unixepoch',
'localtime') AS "End",
CASE
    WHEN ZOBJECT.ZVALUEINTEGER = 0 THEN "Disconnected"
    WHEN ZOBJECT.ZVALUEINTEGER = 1 THEN "Connected"
END AS "Status",
ZSTRUCTUREDMETADATA.Z_DKBLUETOOTHMETADATAKEY__ADDRESS AS "Address",
ZSTRUCTUREDMETADATA.Z_DKBLUETOOTHMETADATAKEY__NAME AS "Device"
FROM ZSTRUCTUREDMETADATA
LEFT JOIN ZOBJECT ON ZSTRUCTUREDMETADATA.Z_PK =
ZOBJECT.ZSTRUCTUREDMETADATA
WHERE ZOBJECT.ZSTREAMNAME = "/bluetooth/isConnected"
ORDER BY "Start" ASC;
```

Once the query completes, the investigator can retrieve the list of events that were logged every time the device connected or disconnected from a Bluetooth-enabled device, along with the device's MAC address; the results are shown in the following screenshot:

![](img/0b17a17d4cb56b1c25763a396dd83117_205_0.png)

Figure 7.7 – The query displaying Bluetooth connection events

Bluetooth artifacts can be an essential part of an investigation. By leveraging data about paired or seen devices, the investigator can assess not only whether the device was in use and connected to a certain device, but

also put the owner in an approximate location at a point in time, if the location of the Bluetooth device is known. In the next section, we'll introduce Safari forensics.

## Understanding Safari forensics

Analyzing artifacts that have been left by internet browsing activity is typically a crucial aspect of mobile forensic investigations. Almost every activity a suspect performs while using a browser on a mobile device leaves a trace on the device itself, including, of course, searching for information and browsing through web pages.

In this section, we'll focus on the browser that is built into all iOS devices, **Safari**. Keep in mind that dozens of third-party browsers can be installed on a device, such as **Chrome**, **Firefox**, and **Opera**: it's important to review these applications and analyze their artifacts as they could potentially contain important data. We'll learn how to analyze third-party applications in _Chapter 10, Analyzing Third-Party Apps_.

Browsing history is likely the most commonly recovered item, but other files should be reviewed too. Typically, the following data can be recovered by analyzing Safari artifacts:

- Browsing history
- Open/private tabs
- Bookmarks/saved pages
- Cookies
- Cache files

Most of Safari's artifacts are stored in `/private/var/mobile/Library/Safari/`. Within this folder, you will find the following relevant files:

/private/var/mobile/Containers/Data/Application/GUID/Library
/Cookies/Cookies.binarycookies

Safari cookies are stored in this file, in binary format. To understand the format specification, please refer to the following link: https://github.com/libyal/dtformats/blob/main/documentation/Safari%20Cookies.asciidoc. Cookies can also be extracted using **automated scripts**, such as the Safari-Binary-Cookie-Parser Python script by Mari DeGrazia. It can be downloaded from https://github.com/mdegrazia/Safari-Binary-Cookie-Parser.

```
/private/var/mobile/Containers/Data/Application/GUID/Library
/Safari/
```

This folder should be analyzed as it contains several useful files, such as Safari **thumbnails**, **preferences**, **auto-fill data**, and a **downloads** list.

Finally, let's cover two different ways of analyzing Safari browsing history.

## Analyzing Safari history

As we learned earlier, Safari stores browsing history in the /private/var/mobile/Library/Safari/History.db database. It's important to understand that this file doesn't just store browsing history from the local device – it also contains data that's been **synced from iCloud**, such as the browsing history from other devices. It's the examiner's job, when analyzing this data, to dissect which websites were visited on the device that is being examined and what data has been synced from different devices.

The following screenshot shows a query example that extracts all the relevant data and the resulting records:

```
LEFT JOIN
    ZSTRUCTUREDMETADATA
    ON ZOBJECT.ZSTRUCTUREDMETADATA =
ZSTRUCTUREDMETADATA.Z_PK
LEFT JOIN
    ZSOURCE
    ON ZOBJECT.ZSOURCE = ZSOURCE.Z_PK
WHERE
    ZSTREAMNAME IS "/safari/history"
ORDER BY DATE ASC;
```

First, we must select the timestamp of when the browser event occurred and transform it into a readable string by using the DATETIME function. Then, we must perform a JOIN on the ZSTRUCTUREDMETADATA table to extract the title of the web page. After that, we must select the URL and calculate the GMT offset. Finally, we must order the results chronologically.

The following screenshot shows the results of this query:

![](img/0b17a17d4cb56b1c25763a396dd83117_210_0.png)

Figure 7.9 — Analyzing the browsing history of KnowledgeC.db

When analyzing the user's browsing history, the investigator should always examine both databases since one of them could potentially contain unique data.

## Introducing private browsing

To finish this chapter, we will introduce **private browsing**, a feature that Apple introduced in iOS 5. When this feature is active, the details of the

user's browsing history are not stored and the websites that have been visited are not synced to iCloud.

> As stated in Apple's official documentation, *"Safari won't remember the pages you visited, your search history, or your AutoFill information AFTER you close a tab in private browsing mode."*

Theoretically, this poses a problem from a forensic perspective as both `history.db` and `KnowledgeC.db` do not store this data. There are, however, other options:

- You can analyze the cookies that are stored on the device. Safari's web cache could also reveal traces of websites that were browsed with private browsing active.
- The `BrowserState.db` database stores details of active tabs, **including private tabs**. For each tab, there is an entry in the `tab_sessions` table that contains a binary PLIST. Converting this PLIST into XML format will allow the investigator to parse through the file, which contains the history of the pages that were browsed within that tab. This will only work if the private tab is still open in Safari.
- As we learned in the previous chapters, deleted data is not permanently removed from a SQLite database – it is merely marked as deleted. By using **forensic tools**, it is possible to recover deleted records from the `BrowserState.db` database, which may include details of tabs that have been opened in private mode. To learn how to attempt data recovery from SQLite databases, please refer to *Chapter 4, Working with Common iOS Artifacts*.

Internet browsing artifacts are an important part of any mobile forensics investigation. Analyzing the browsing history may provide some insight, but accessing the data that's relevant to private internet browsing can be far more enlightening, revealing any activity that the user wanted to keep hidden.

## Summary
