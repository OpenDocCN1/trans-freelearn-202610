Another installation tool for packaging and deploying Web Parts, called WPPackager, is available at http://www.microsoft.com/downloads/details.aspx?familyid=0fda5912-c136-4b44-911a-011adfcc66e3&displaylang=en. WPPackager creates a Windows Installer File (MSI) for your Web Part, which performs the necessary configuration of SPS/WSS during installation.

For more information on this tool, see *Using WPPackager to Package and Deploy Web Parts for Microsoft SharePoint Products and Technologies* at http://msdn.microsoft.com/library/default.asp?url=/library/en-us/odc_SP2003_ta/html/sharepoint_deployingwebparts_msi.asp.

# Preparing the Web Part Description (.DWP) File

When deploying a Web Part to SharePoint, several files should be populated during the deployment process. One of these files is the Web Part Description (.DWP) file.

+   1. In Visual Studio .NET, right-click on the `ExtensibleMCMSPageListingWebPart` project and choose Add New Item.
+   2. In the Categories pane on the left, expand Local Project Items and select Data.
+   3. Select Web Part DWP.
+   4. Enter the name as `NavigationWebPart` and click OK.

The .DWP file describes our Web Part, giving the default values for properties and any assemblies that the Web Part requires. Let's create this file for our navigation Web Part:

```
<?xml version="1.0" encoding="utf-8"?>
<WebPart xmlns="http://schemas.microsoft.com/WebPart/v2" >
  <Title>Extensible MCMS Page Listing Web Part</Title>
  <Description>
    Displays MCMS Postings and Channels within a specified channel and
    transforms the display using XSLT.
  </Description>
  <Assembly>ExtensibleMCMSPageListingWebPart</Assembly>
  <TypeName>ExtensibleMCMSPageListingWebPart.NavigationWebPart</TypeName>
</WebPart>
```

The values specified in this file allow Windows SharePoint Services to generate metadata for the Web Part. The meanings of the elements are given in the table below:

| Element Name | Description |
|---|---|
| `Title` | Title that the user will see in the Web Part Library and the default title of the Web Part once it is added in a Web Part Zone. |
| `Description` | Tooltip that will appear over the Web Part title in the Web Part library. |
| `Assembly` | Assembly name. For this sample we are only using the assembly name but it is also possible to use the complete Web Part assembly description like so: `AssemblyName` (without the `.DLL` extension), `Version=VersionNumber`, `Culture=Culture`, `PublicKeyToken=PublicKeyToken` |
| `TypeName` | The namespace-qualified class name. |