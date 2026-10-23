+   - **Azure Resource Manager**: Threat protection.
+   - **Azure DNS**: Threat protection.

To give some context to what kind of threat protection features Defender for Cloud provides, the following is a screenshot from my test environment where we have enabled Defender for Cloud for servers, where the network traffic analysis mechanisms have detected anomalous incoming RDP traffic to our machine.

By collecting IPFIX metadata from Azure edge routers, the service can identify and monitor any potentially suspicious outbound traffic originating from VMs:

![](bbox=[0.19, 0.3, 0.79, 0.58])

Figure 3.21 – Alert from Microsoft Defender for Cloud

Regarding vulnerability management with Defender, we have two options: using a Qualys agent that has collected data and delivered the results back to Defender or using the built-in threat and vulnerability module from Microsoft. Note that these two options are only available for server operating systems. This feature is also available for Azure Arc machines.

Microsoft has also recently introduced an offline scan called agentless scanning that supports environments where you do not want to have additional clients installed. You can read more about this feature here: https://learn.microsoft.com/en-us/azure/defender-for-cloud/enable-vulnerability-assessment-agentless.

To enable Microsoft Defender for Azure Arc-enabled machines, you need to go into the Azure portal, go into **Microsoft Defender for Cloud**, choose **Environment settings**, and click on the subscription where you have the Azure Arc machines.