To debug our Web Part from the SPS/WSS server using Visual Studio .NET, we must add the Web Part to a Web Part Page and perform the following actions:

+   1. Open the ExtensibleMCMSPageListingWebPart project in Visual Studio .NET.
+   2. Set a breakpoint on the RenderWebPart() method in NavigationWebPart.cs.
+   3. From the Tools menu, select Debug Processes (or hit Alt + Ctrl + P).
+   4. Select the w3wp.exe process and click Attach.
+   5. Verify that the Common Language Runtime is checked and click OK:

![](bbox=[0.2, 0.235, 0.795, 0.525])

+   6. Click Close on the Processes dialog and refresh the Web Part Page.
+   7. Your breakpoint should now be selected and you should be able to step through the code.

Your Web Part must be compiled in Debug configuration to ensure that the debug symbols are deployed to the server.

# Summary

In this chapter, we've explored the basics of the Web Part Framework by building an MCMS Navigation Web Part that can display MCMS channel items from within a SharePoint site. In development, we utilized the Web Part templates for Visual Studio .NET to create our Web Part. This allows us to leverage Visual Studio .NET's Server Control development architecture. Our Web Part generates an XML representation of the channel items and sub-channels that were found in a specified channel. The XML data was then transformed using a configurable XSLT stylesheet. Finally, we deployed our Web Part to the SharePoint server by creating a CAB file deployment project and running the SharePoint STSADM.EXE deployment tool.