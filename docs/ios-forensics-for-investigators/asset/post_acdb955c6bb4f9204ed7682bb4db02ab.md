
You can read the device model by running the following command:

```
ideviceinfo | grep ModelNumber
```

## Identifying the iOS version:

To identify what version of iOS the device is running, you can run the following command:

```
ideviceinfo | grep ProductVersion
```

## Identifying the device serial number:

The device's serial number can be parsed by running the following command:

```
ideviceinfo | grep SerialNumber
```

## Identifying the phone number:

Finally, to identify the phone number related to the SIM card in the device, run the following command:

```
ideviceinfo | grep PhoneNumber
```

## Deciding the best acquisition method

Once the device has been seized, it has been correctly isolated, and you have identified the hardware model and iOS version, the next step in the examination process is assessing what the best acquisition method is to extract the richest dataset from the device while preserving data integrity.

Generally speaking, you'll want to keep these two concepts in mind before performing any actions on the device:

- Acquire evidence by order of **volatility**.
- Acquire evidence, starting with the less intrusive method.

The first step in any acquisition process is determining the volatility of the data of interest; for example, if the scope of the investigation is to extract photos from a device or parse text messages, clearly this doesn't pose any kind of concern because this data is written on the flash memory, it isn't volatile, and any acquisition method will extract these artifacts. However, if the investigation seeks to understand how the device was used, or if there was any human interaction on a particular date and time, or where the device was located a few days ago, these artifacts are much more volatile, as log files and databases on iOS devices are constantly overwritten. For instance, location data is generally maintained on the device for several weeks; however, *precise* location data, including cell tower data, is only maintained for a few days. In this case, we'll want to extract this kind of data as soon as possible before it gets overwritten. This means performing a full filesystem acquisition (if possible) before anything else, as logical acquisitions do not contain this data.

The investigator should have a clear understanding of how forensic tools work and what impact they have on data and on the device itself:

- Anything you decide to do (including nothing!) will have an impact on the device.
- What impact will your actions have on the data?
- Does the chosen acquisition method require a device reboot?
- Will artifacts of interest survive a device reboot?
- How long are artifacts stored on the device before being overwritten?
- What files or folders will be affected by a jailbreak?
- What changes will occur by running a Checkm8 acquisition?

## What changes will occur by performing an agent-based acquisition?

A logical acquisition will have virtually no impact on the device, so most often, this will be your first choice. If the device is vulnerable to Checkm8 and if you have tools that can perform this kind of acquisition, a Checkm8 acquisition will be the best option to gain access to the entire filesystem without making any changes to the device. If this is not an option, the next step would be considering an agent-based acquisition. Finally, as a last resort, jailbreaking the device will give the investigator access to the entire filesystem, but make sure you understand how this will affect the device and its data.

## Performing a logical acquisition

Now that we've learned the theory, it's time to put it into practice by performing our first logical acquisition.

Almost all forensic tools can perform a logical acquisition; we'll concentrate on two of the most popular ones, Cellebrite UFED and Elcomsoft iOS Forensic Toolkit.

Cellebrite UFED is considered the industry standard, as it allows investigators to perform a variety of different acquisitions, including a traditional logical acquisition, a partial filesystem, a full filesystem acquisition through Checkm8, and a full filesystem acquisition from devices that have been jailbroken.

Elcomsoft iOS Forensic Toolkit, on the other hand, can perform logical acquisitions, a full filesystem acquisition from jailbroken devices, full filesystem acquisitions through agent-based extractions, and it also supports physical acquisitions from older devices.

## Logical acquisition with Cellebrite UFED

In this example, we'll acquire an iPhone 6 running iOS 12:

1. The first step is to launch Cellebrite UFED and choose Mobile device from the main screen:

![](img/0b17a17d4cb56b1c25763a396dd83117_63_0.png)

Figure 2.8 – The home screen of Cellebrite UFED

2. Next, select the AUTO DETECT option or choose the device model using the search function in the upper-left part of the screen:

# Figure 2.9 – Choose the device or let UFED auto-detect it

3. Once the device is recognized, UFED will show the available acquisition methods depending on the iPhone model. Choose the Logical (Partial) acquisition to continue:

# Figure 2.10 – Choose the type of acquisition

4. One of the best features of Cellebrite UFED is that it will allow you to select what data you want to extract. This will greatly speed up the extraction process, allowing you to access useful evidence within minutes. Select the artifacts that should be extracted and click Next to start the acquisition process:

![](img/0b17a17d4cb56b1c25763a396dd83117_65_0.png)

# Figure 2.11 – Choose what artifacts should be extracted

5. To perform a logical acquisition, the device must be unlocked. Consider disabling Auto-lock from the device's settings. You should also Trust the computer when the device prompts you for the pairing request:

![](img/0b17a17d4cb56b1c25763a396dd83117_67_0.png)

Figure 2.13 – The extraction summary displays what data was extracted

You can view the extracted data by loading the logical acquisition into a forensic tool, such as Belkasoft Evidence Center X, Elcomsoft Phone Viewer, or Cellebrite Physical Analyzer.

## Logical acquisition with Elcomsoft iOS Forensic Toolkit

Another great tool to perform acquisitions from iOS devices is the forensic toolkit by Elcomsoft. This toolkit has been specially designed for iOS devices and offers some unique features, such as agent-based extractions. The steps for this are as follows:

1. Launch Elcomsoft iOS Forensic Toolkit and check whether the device is correctly recognized. As you can see from the following screenshot, the tool is divided into four sections, which each deal with different acquisition processes. In our example, we want to acquire a logical image and the device is not jailbroken, so we're going to choose the Backup option, which will perform a logical acquisition:

![](img/0b17a17d4cb56b1c25763a396dd83117_68_0.png)

Figure 2.14 – The home screen of Elcomsoft iOS Forensic Toolkit

2. Choose the folder where the backup should be stored:

![](img/0b17a17d4cb56b1c25763a396dd83117_69_0.png)

Figure 2.15 – Choose where the backup should be stored

3. To obtain the fullest logical image, backup encryption should be enabled. The toolkit will automatically detect whether encryption is enabled and, if it's not, it will enable it by setting the password to 123. Backup encryption will be disabled at the end of the acquisition process:

![](img/0b17a17d4cb56b1c25763a396dd83117_70_0.png)

Figure 2.16 – The toolkit will show progress and debug information on screen

At the end, the toolkit will give you a summary of the acquisition process. The extracted files can now be examined using the forensic tool of your choice.

We've seen how to perform logical acquisitions using two different tools. Keep in mind that there are a variety of different tools that can perform this kind of acquisition, including free tools. In the next section, we'll learn how to perform a full filesystem acquisition.

## Performing a filesystem acquisition
