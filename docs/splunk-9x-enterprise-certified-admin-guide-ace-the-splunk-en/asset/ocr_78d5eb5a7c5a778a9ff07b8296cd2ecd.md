+   - License manager and license peer
+   - License usage and alerting

Let’s discuss each of these in detail in the following sections.

# Adding a license

A Splunk Enterprise license is a file with the .license extension, which is generally purchased from Splunk. The following steps are applicable to other types of license, such as the Developer license.

A license file can be added to Splunk in either of the following two approaches:

+   - On the Splunk web home page (https://<hostname>:8000), go to the menu in the top-right corner of the window and navigate to Settings | Licensing. Click on the Add license button, and on the next screen, select Choose File to upload a .license file obtained from Splunk sales. Then, click the Install button. At this stage, you are required to restart the Splunk instance. The following screenshot shows the licensing web page, and you can see the Add license button, which is used to upload a license file.

![](bbox=[0.11, 0.44, 0.85, 0.71])

Figure 2.2: The Licensing web page

+   - The second approach is to execute a CLI command on the host the Splunk instance runs on. You should have access and execution permissions to the host where Splunk license manager is installed. A restart is required after the successful command execution:

```
splunk add licenses <path_to_license_file_required>
```