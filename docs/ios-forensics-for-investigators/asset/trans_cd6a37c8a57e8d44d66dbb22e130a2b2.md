

```
SELECT p.First AS "First Name",
       p.Last AS "Last Name",
       GROUP_CONCAT(c.value) AS "Contacts"
FROM   ABPerson AS p
       LEFT JOIN ABMultiValue AS c
             ON c.record_id = p.rowid
GROUP  BY p.rowid,
          p.first;
```

首先，我们必须选择与名字和姓氏相关的列；然后，我们必须使用 GROUP_CONCAT SQL 函数将所有联系人数据（电话号码、电子邮件等）聚合到一列 Contacts 中。然后，我们必须使用 JOIN 语句在 ABPerson 和 ABMultiValue 表之间建立关系。

此查询将提取地址簿中存储的所有联系人列表，如以下屏幕截图所示：

![](img/0b17a17d4cb56b1c25763a396dd83117_197_0.png)

图 7.3 – 从地址簿中提取的记录

现在我们已经学会了如何提取联系人数据，我们可以开始分析通话记录。

## 分析通话记录

所有已拨出、已接听或未接听的电话和 FaceTime 通话都记录在 CallHistory.storedata 数据库中，该数据库位于 /private/var/mobile/Library/CallHistoryDB/。

### Tip

`CallHistory.storedata` 数据库是在 iOS 8 中引入的。此前，通话记录位于 `call_history.db` 文件中。另外请注意，通话历史记录只有在 **After First Unlock (AFU)** 状态下才可用。在首次解锁之前，数据存储在名为 `CallHistoryTemp.storedata` 的临时文件中。

从取证角度来看，最重要的数据存储在 ZCALLRECORD 表中，您将在其中找到以下列：

- ZDATE 列包含通话发生的时间戳，采用 Mac Absolute Time 格式。
- ZADDRESS 表示拨打的电话号码，或如果是来电，则表示通话的来源。
- ZLOCATION 列存储位置，该位置是根据电话号码计算得出的。
- ZDURATION 列表示通话时长，以秒为单位。
- ZORIGINATED 列表示通话是来电（0）还是去电（1）。
- ZANSWERED 列表示来电是否被接听（1）或未接听（0）。
- ZSERVICE_PROVIDER 列显示产生通话的服务名称。通常，这将显示为传统通话的 `com.apple.Telephony` 或 FaceTime 通话的 `com.apple.FaceTime`。

以下 SQL 查询将从 ZCALLRECORD 表中提取有意义的数据：

```
SELECT
DATETIME(ZDATE + 978307200, 'unixepoch', 'localtime') AS "Date/Time",
ZADDRESS AS "Phone Number",
ZLOCATION AS "Location",
ZDURATION AS "Call in Seconds",
```

分析 iOS 设备提供的网络数据可以让调查人员准确了解哪些服务和应用程序正在消耗带宽，以及设备连接了哪些网络。

我们将通过学习如何从完整文件系统中提取一些基本网络相关数据来介绍该主题。

## 飞行模式

要检测设备是否处于飞行模式，可以检查位于 /private/var/preferences/SystemConfiguration/ 的 com.apple.radios.plist 文件。该 PLIST 包含一个 AirplaneMode 键，其值为 true 或 false。

## Wi-Fi MAC 地址

设备内无线网络接口的 MAC 地址可以通过分析位于 /private/var/preferences/SystemConfiguration/ 的 NetworkInterfaces.plist 文件找到。IOMACAddress 键指示所需的值。请注意，每台 iOS 设备都有多个网络接口，每个接口都有唯一的 MAC 地址。

## 已知 Wi-Fi 网络

已知无线网络的列表存储在 /private/var/preferences/SystemConfiguration/com.apple.wifi.plist 文件中。该 PLIST 包含一个已知网络的键列表，这也是一个项目数组：

- SSID_STR 键包含一个可读的 SSID 字符串，用于标识网络。
- BSSID 键指示接入点的 MAC 地址。
- `lastJoined` 键存储用户上次手动访问网络的时间戳。
- `lastAutoJoined` 键指示设备上次自动连接到无线接入点的时间：

```
```xml
<key>SSID</key>
<data>
Q2Nvb2tpZXNEY2FzdGlugR3Vlc3Q=
</data>
<key>SSID_STR</key>
<string>Cookiesdcastler5 Guest</string>
<key>SaveDataMode</key>
<integer>2</integer>
<key>ScaledRSSI</key>
<real>0.82863259315490723</real>
<key>ScaledRate</key>
<real>1</real>
<key>ShareableStatus</key>
<integer>1</integer>
<key>Strength</key>
<real>0.82863259315490723</real>
<key>VHT_CAPS_IE</key>
<dict>
    <key>VHT_CAPS</key>
    <integer>864041394</integer>
    <key>VHT_SUPPORTED_MCS_SET</key>
    <data>
    +v8AArP/AAA=
    </data>
</dict>
<key>WiFiAutoInstantHotspotJoining</key>
<false/>
<key>WiFiInstantHotspotJoining</key>
<false/>
<key>WiFiManagerKnownNetworksEventType</key>
<integer>1</integer>
<key>WiFiNetworkAttributeIsKnown</key>
<true/>
<key>WiFiNetworkAttributeIsMoving</key>
<false/>
<key>WiFiNetworkAttributeIsPotentiallyMoving</key>
<true/>
<key>WiFiNetworkAttributeLowPopularity</key>
<true/>
<key>WiFiNetworkAttributePopularityScore</key>
<integer>0</integer>
<key>WiFiNetworkAttributeProminentDisplay</key>
<true/>
<key>WiFiNetworkAttributeSource</key>
<integer>2</integer>
<key>WiFiNetworkIsAutoJoined</key>
<true/>
<key>WiFiNetworkPasswordModificationDate</key>
<date>2020-03-22T19:00:00Z</date>
<key>addedAt</key>
<date>2020-03-22T19:00:00Z</date>
<key>enabled</key>
<true/>
```
```

图 7.5 – com.apple.wifi.plist 显示已知网络的详细信息

现在我们知道如何分析设备上的基本网络数据，让我们更仔细地查看网络使用痕迹。

## 分析网络使用情况

分析哪些服务或应用程序正在使用网络可以帮助调查人员识别任何异常进程，例如设备上运行的 **malware**。

iOS 设备上的数据使用情况存储在两个不同的文件中：

- `CurrentPowerLog.PLSQL` 数据库，可在 `/private/var/containers/Shared/SystemGroup/<GUID>/Library/BatteryLife/` 中找到，包含多个感兴趣的表。通过关联不同表中的数据，检查人员可以提取每个进程的精确数据使用情况，包括 **Wi-Fi 接口** 和 **WAN 接口**。需要注意的是，此数据库仅存储大约 24 小时的进程数据。

- `DataUsage.db` 数据库，可在 `/private/var/wireless/Library/Databases/DataUsage.sqlite` 中找到，包含两个重要的表 – ZPROCESS 和 ZLIVEUSAGE。第一个存储应用程序的名称、应用程序包及其关联的进程以及时间戳；第二个表存储每个进程通过 WAN 接口进出的数据。虽然该数据库通常会存储数周的数据，但请注意，根据已开展的研究，似乎通过 Wi-Fi 接口进出的数据未被记录。

这些数据库可以帮助调查人员确定不仅哪些进程正在使用网络，而且受试者在特定时间是否正在使用应用程序。

可以从 `DataUsage.db` 数据库中运行以下查询来提取进程和网络使用数据：

```
SELECT
    DATETIME(ZPROCESS.ZTIMESTAMP+ 978307200,
'UNIXEPOCH') AS "PROCESS TIMESTAMP",
    ZPROCESS.ZPROCNAME AS "PROCESS NAME",
    ZPROCESS.ZBUNDLENAME AS "BUNDLE ID",
    ZLIVEUSAGE.ZWWANIN AS "WWAN IN",
    ZLIVEUSAGE.ZWWANOUT AS "WWAN OUT"
FROM ZLIVEUSAGE
LEFT JOIN ZPROCESS ON ZLIVEUSAGE.ZHASPROCESS =
ZPROCESS.Z_PK;
```

以下屏幕截图显示了输出结果；它包含对调查人员非常有价值的数据，特别是如果调查中对应用程序使用感兴趣：

![](img/0b17a17d4cb56b1c25763a396dd83117_203_0.png)

图 7.6 – 查询结果显示设备上运行的进程及其数据使用情况

在下一节中，我们将介绍蓝牙取证。

## 介绍蓝牙取证

使用蓝牙技术，用户可以在不同设备之间传输数据，并将耳机、扬声器或任何类型的无线设备连接到智能手机。通常，iOS 设备作为 Class 2 蓝牙设备运行，这意味着它们的工作范围约为 10 米（33 英尺）。

