![](bbox=[0.168, 0.103, 0.823, 0.557])

There are multiple profiles that can be used to enable **Application Guard** in Intune. These options, as shown in *Figure 4.5*, only enable it for Microsoft Edge in standalone mode. If we want to enable application guard for Office and Managed mode, we need to use another profile setting in Intune under **Endpoint Security | Attack Surface Reduction | Create Profile** and select **App and browser isolation**.

Additionally, it should be noted that Application Guard is available as an extension for other browsers such as Firefox and Google Chrome if you want the same level of protection for those browsers. You can find the extension at `https://www.microsoft.com/security/blog/2019/05/23/new-browser-extensions-for-integrating-microsofts-hardware-based-isolation/`.

When you deploy this setting, it will configure the application guard feature in a standalone mode that will add an additional button within Microsoft Edge at the bottom, which users can start to initiate a browser session in Application Guard when needed, as shown in the following screenshot: