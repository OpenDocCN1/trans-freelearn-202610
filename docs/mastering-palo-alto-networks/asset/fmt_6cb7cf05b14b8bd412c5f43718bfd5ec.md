
* `GlobalProtect` (`GP`) enables mobile applications on Android, iOS, Win10 UWP, Chrome OS, and Linux. It enables `Host Information Profile` (`HIP`) checks and agentless VPNs. It also allows split tunneling based on host application or domain names.
* `DNS security` (`DNS`): Dynamic DNS signature lookups and blocking or “sinkholing” of malicious DNS requests.
* `Advanced DNS security` (`A-DNS`): Adds inline cloud analysis for command-and-control lookups.
* `WildFire` (`WF`): Live threat signature feed, real-time ML-enabled analysis, and cloud-based sandbox analysis.
* `Advanced WildFire` (`A-WF`): Live threat signature feed, real-time ML-enabled analysis, and cloud-based sandbox analysis.
* `Decryption port mirroring`: Allows decrypted sessions to be copied to a different device for additional IDS scanning via a dedicated “port mirror” interface.
* `Internet of Things` (`IOT`): Enables detection of IoT devices and generates rulebase adjustments to protect vulnerable IoT devices.
* `Data loss prevention` (`DLP`): ML-powered data loss prevention scanning.

More features are being added as Palo Alto Networks announces new products.

## Activating licenses via the customer support portal

In the `CSP`, you can find your registered devices under the `Assets` tab as a device. There’s a pencil icon that allows you to activate auth codes:

![](Images/B31589_02_11.png)

Figure 2.11: The Devices page in the CSP

You will notice there is already a software warranty support license active for a limited amount of time. This is a temporary support license that allows an RMA to be started if your device arrives broken in the box. To add the actual support license and any feature licenses, click on the pencil icon in the `Actions` column you see in `Figure 2.11`. This will call the pop-over feature activation window:

![](Images/B31589_02_12.png)

Figure 2.12: Adding auth codes to activate services

Once you’ve added all your licenses, the device should look something like this:

![](Images/B31589_02_13.png)

Figure 2.13: A fully licensed device

The little download icons next to each license allow you to download the license key file so that you can upload the key onto the firewall. This is required if you intend to run the firewall without an internet connection and want to be able to upload signature files and enforce security profiles.

Besides activating licenses via the support portal, they can also be activated directly from the firewall interface.

## Activating licenses via the web interface

This procedure requires that the management interface has an internet connection and is able to resolve internet domain names via DNS. If an internet connection is not available, see the previous section on how to download the license keys.

To activate licenses via the web interface, navigate to `Device` | `Licenses`. If you activated the licenses in the CSP and then proceeded to download the license key files, you can click on `Manually upload license key`.

If you activated the licenses on the CSP and want to fetch the licenses, click `Retrieve license keys from license server`. Make sure the firewall has been set up with a functional default gateway and DNS servers.

If you want to activate new licenses with an auth code, click on `Activate feature using authorization code` and you will see a popup where you can enter each auth code individually:

![](Images/B31589_02_14.png)

Figure 2.14: Activating a license using an auth code

With each added license, a section will be added containing the license information:

![](Images/B31589_02_15.png)

Figure 2.15: Active licenses on the device

To activate the support license, you may need to activate the auth key through the `Support` menu item:

![](Images/B31589_02_16.png)

Figure 2.16: Activate support using an authorization code

`Important note`

On hardware firewalls the support license is more like a contract than a license required for a feature to work; a support person will take your call if something goes wrong, a replacement device will be sent if your unit is broken, and so on. This is the only license that does not need to be on the device necessarily before features start to function (i.e., all base functionalities will work without the support license present).

On VM firewalls, the support license is tied to the VM’s activation and the number of CPU cores it supports, so on the VM series, you are required to load the support license.

After all licenses are activated on the device, the next step is to start downloading and scheduling updates to the different databases.

## Downloading and scheduling dynamic updates

Now that all the licenses are active, you can set up dynamic updates and start downloading all the content packages.

Navigate to the `Dynamic Updates` menu under the `Device` tab, where you can manually download content packages and set up schedules and installation preferences. The first time you visit this menu, it may look a bit off as the available content has not been loaded onto the device yet. Click the `Check Now` button to connect to the updates server and fetch the available packages for your system, as shown:

![](Images/B31589_02_17.png)

Figure 2.17: The initial Dynamic Updates view

Once the updates have been fetched, you may still notice that some antivirus packages are missing. This is because the device first needs to be brought up to date with all the app ID and content ID application and decoder updates before further packages can be loaded onto the system. Go ahead and download the latest `Applications and Threats` package:

![](Images/B31589_02_18.png)

Figure 2.18: Downloading the first Applications and Threats package

`Important note`

If no threat prevention license has been activated, there will only be an `Applications` package available for download. This will be indicated by the package name starting with `panupv2-all-apps` instead of `panupv2-all-content` and the `FEATURES` column only listing `Apps`.

Once the package has been downloaded, click `Install`. Once the installation has been completed, click `Check Now` again, and the antivirus will become available. Go ahead and download and install the latest package of antivirus updates.

`Important note`

URL filtering and DNS security do not have update packages because URLs are looked up against the cloud service and then stored in the local cache.

You can now start building schedules by clicking on the blue `None (Manual)` option after `Schedule`:

![](Images/B31589_02_19.png)

Figure 2.19: The antivirus and WildFire schedules

The antivirus and WildFire schedules look very similar. They both include the `Recurrence` parameter, which tells the firewall how regularly it needs to check for updates. The options differ for both schedules, though:

* The update interval options for `Antivirus` are `Weekly`, `Daily`, `Hourly`, or `Manual`.
* The update interval options for `WildFire` are `Real-time`, `Every minute`, `15 minutes`, `30 minutes`, `1 hour`, or `Never`.

When `Recurrence` is set to any value higher than 1 minute (15 or 30 minutes, or hourly), you can additionally set at which minute within the frame the actual check should take place. (e.g., 13 minutes past the hour, 4 minutes into the quarter hour, etc.) This helps prevent conflicting update connections to the update server in cases where the outgoing internet bandwidth is restricted.

The `Action` can be set to simply `download` or `download-and-install`. If the action is set to `download`, manual installation is required.

`Threshold` is a feature that the antivirus update shares with `Applications and Threats`:

![](Images/B31589_02_20.png)

Figure 2.20: Antivirus and WildFire schedules
