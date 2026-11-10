# 22.2 Setting Up and Using Web View

In this section, we will cover the detailed steps for setting up and using Web View:

+   1. Log in your web server host (webview_server machine in our case) and go to Programs | Administrative Tools | Internet Services Manager.

The Internet Information Services console opens up as shown in figure 22.2. Choose the Default Website, right-click, and choose the Properties option.

![](bbox=[0.185, 0.27, 0.815, 0.605])

Figure 22.2: Internet Information Services console

+   2. Go to the Home Directory tab (refer to figure 22.3) in the Default Website Properties window and note down the location mentioned against the Local Path section.

The default location is: c:\inetpub\wwwroot.

We will need to specify this under the Target Root Directory field while setting up Web View Site Publishing Configuration.