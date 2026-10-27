
Smartphones are, by design, meant to communicate through cellular networks, Bluetooth connections, and wireless Wi-Fi networks. To prevent the device from communicating, it is important to isolate the device as soon as possible. Isolating the device prevents new data (incoming calls, incoming messages, and so on) from modifying existing evidence and thus enforces data preservation. Additionally, if the device were allowed network access, data destruction could occur as Apple products can be remotely wiped via a *kill signal.*

The easiest way to isolate an Apple device is by enabling **Airplane** mode; this deactivates the device's wireless and cellular connections. You can enable **Airplane** mode from the device's **Settings** or, if the device is locked, directly from the control center by swiping up from the bottom. If the device is an iPhone X or newer, you can access the control center by swiping down from the top-right corner. However, it's worth noting that isolating the device can also be accomplished by putting the device in a Faraday bag, a container made of metallic shielding that blocks the radio frequencies used by cellular networks, GPS, and Wi-Fi networks.

Don't forget to keep the device charged to ensure it doesn't power off. Generally speaking, you want the device to remain in the state in which you found it:

- If the device was off, leave it off.
- If it was on, make sure you leave it on so that it doesn't lock!

This is particularly important if the device is passcode protected and the investigator doesn't have the code: if the device powers off, it would transition from an AFU state to a BFU state, rendering data recovery much more challenging and, in some cases, impossible. We will learn more about device states later in this chapter.

## Tip

One of the most common mistakes is removing the SIM card from an unlocked device to isolate it from a cellular network. This may make sense when you're working with different devices, but you want to avoid removing the SIM card from iPhones or iPads because doing so will result in iOS automatically locking the device, biometric unlock will be disabled, and USB restricted mode will be activated.

After successfully seizing and isolating the device, care should be taken to maintain the device's chain of custody. NIST defines the chain of custody as follows:

> "A process that tracks the movement of evidence through its collection, safeguarding, and analysis life cycle by documenting each person who handled the evidence, the date/time it was collected or transferred, and the purpose for the transfer."

You can learn more about this topic by visiting the following website:

[CSRC.nist.gov](https://csrc.nist.gov/).

## Acquisition

Acquisition is the process of obtaining information and evidence from a device.

Before the actual acquisition process begins, you should have a clear understanding of what kind of evidence you're looking for as this will have a great influence on the tools you choose to use.

Generally speaking, there are three different kinds of data acquisition, and the method the investigator chooses affects how much data can be extracted from the device:

- Logical acquisition
- Full filesystem acquisition
- Physical acquisition

In the following table, you can see what kind of data can be extracted with each method:

| | Logical | Filesystem | Physical |
|---|---|---|---|
| Calls, calendars, contacts, and messages | ✓ | ✓ | ✓ |
| User content (photos, videos, notes, and so on) | ✓ | ✓ | P |
| Keychain | Limited | ✓ | ✓ |
| Third-party app data | Limited | ✓ | ✓ |
| Apple Pay | - | ✓ | ✓ |
| Location data (radio cells, GPS fixes, and so on) | - | ✓ | ✓ |
| Mail | - | ✓ | ✓ |
| User activity logs | - | ✓ | ✓ |
| System logs | - | ✓ | ✓ |
| Deleted data (unallocated space) | - | - | ✓ |
| Deleted data (SQLite databases) | Limited | ✓ | ✓ |

Table 1.2 – Comparison of data that can be extracted with different acquisition methods

## Logical and filesystem extractions

As you can see, a logical acquisition will extract some user-generated data from the device such as photos, messages, and notes. A logical extraction will usually be the easiest and quickest type of extraction and most forensic tools support it. However, considering the number of files available on an iOS device, only acquiring the full filesystem will give access to precious evidence such as third-party app data, precise location data, and pattern-of-life information.

A filesystem extraction is a representation of the files and folders from the user partition of the device; it is possible to partially recover deleted data by analyzing databases and temporary files, such as WAL files.

## Physical extraction

Devices up to and including the iPhone 4 can undergo a full, forensically sound physical extraction of the entire raw disk. This kind of extraction delivers the most data as, much like a standard hard drive, both the system and the user partitions are extracted, as well as unallocated areas of the disk. This is incredibly useful as it allows investigators to recover deleted data, including messages, photos, and videos. However, with the introduction of iOS 5, Apple changed the way data was encrypted on disk and enabled other security features, such as data protection class keys. At the time of writing, extracting a physical image of modern iOS devices is simply not possible.

## Choosing the best acquisition method

Investigators should always perform a logical acquisition first and then, if possible, attempt a filesystem or physical acquisition.

Jailbreaking the device is generally required to enable unrestricted access to the device's filesystem; however, more and more vendors are adding the possibility to run forensically sound checkm8-based extractions (all operations are performed in the device's volatile RAM) or agent-based extractions. Both options allow you to obtain the full filesystem without jailbreaking the device.

At the end of the day, the method you choose will usually depend on four variables:

- The state in which you find the device (locked or unlocked)
- The device model and iOS version
- The tools you have access to
- The possibility of jailbreaking the device

If the device is locked and you don't have the passcode, there is not a simple solution that can consistently bypass iOS security. However, depending on the device model, several options can provide a limited amount of data. We will examine these in detail in the next chapter.

If the device is unlocked, depending on the available tools, you can attempt a filesystem extraction. You should start with the less intrusive option, such as an in-memory checkm8 exploit, if your tools support it. If the device doesn't allow checkm8 exploiting, you can attempt an agent-based extraction if the iOS version is compatible. Although this entails installing an agent on the device and some minor changes can occur, this is considered forensically safe. If neither of these options is available, you can consider jailbreaking the device through checkra1n or unc0ver. Make sure you understand how the jailbreak works, how it affects the device, and what the risks are. Finally, if none of these options are available, you will have to go for a logical acquisition.

We will learn all about extracting data from iOS devices in the next chapter, but for now, it's important to note that the phone should be acquired using a tested method that is repeatable and is as forensically sound as possible. It is highly recommended to experiment with various tools on test devices to determine which acquisition method and tool work best with different devices.

If you don't have direct access to the device, there are other sources of evidence you can consider, such as iTunes backups and iCloud extractions.

## Analysis

The main point of doing a forensic examination is to find, extract, and process evidence related to a particular case or investigation.

Although there is no standard process to analyze what you extracted from a device, here are some guidelines that should help you get started:

- Go through all the available data to become familiar with the main sources of evidence found on the device.
- Use your tools and forensic software to quickly parse common data sources, such as SMS databases, installed apps, call logs, photos, and so on.
- Attempt to recover any deleted data both with software and by manually carving binary files, logs, plists, and SQLite databases.
