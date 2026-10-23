![](bbox=[0.131, 0.096, 0.853, 0.357])

*Figure 5.12 – Microsoft Defender for Cloud Recommendations list*

Here, we can also see what kind of applications are installed on the machine and if there are any known vulnerabilities on that specific machine.

The second part of this, which is not so clearly visible in the UI, is that these servers with EDR capabilities can also send data to another data source, which is part of the Defender for Endpoint database.

However, this requires that we install an additional agent to onboard our servers to be able to send data to the service. This can be configured automatically so that Microsoft will install the additional agent. All we need to do is integrate Defender for Cloud and Defender for Endpoint.

To enable the integration, go to **Defender for Cloud** in the Azure portal, go to **Environment settings**, and then select the subscription with the machines that we recently set up for Defender for Servers. Select **Integrations**, choose the **Allow Microsoft Defender for Endpoint to access my data** setting, and click Save.

> **Note**
> 
> Microsoft Defender for Cloud will automatically enroll your machines in the Microsoft Defender for Endpoint service. The enrollment process, known as onboarding, may take up to 12 hours for machines that already exist when the integration is enabled. For new machines created after the integration has been activated, onboarding typically takes about an hour.

Once the integration is in place, the server should be automatically onboarded to the Defender for Endpoint service as well. Another option is to onboard the machines by installing the software manually, which can be done by logging in to `https://security.microsoft.com`, and under **Settings | Endpoints | Onboarding**, we have different onboarding mechanisms for different operating systems. Download the appropriate software version for your operating system and install it using the wizard.