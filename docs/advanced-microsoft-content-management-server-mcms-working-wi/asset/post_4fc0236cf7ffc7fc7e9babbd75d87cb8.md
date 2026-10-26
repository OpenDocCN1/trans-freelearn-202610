
## Creating the Web Part Project

Web Parts should be built using the Web Part Library project. So let's go ahead and create this project.

- 1. In Visual Studio .NET, select Create New Project (Shift + Ctrl + N).
- 2. Select the project type as Visual C# Projects, and choose the new Web Part Library template from the right-hand pane.
- 3. Name the new project ExtensibleMCMSPageListingWebPart, and place the project directory under My Documents\Visual Studio Projects.
- 4. Click OK.

![](img/442e26bf28f2df81fbf8dff3478f88e3_219_0.png)

Once our new project is created, we can see a variety of files and references that have been created by the template:

![](img/442e26bf28f2df81fbf8dff3478f88e3_219_1.png)

| Property | Description | Data type | Default Value | Category |
|---|---|---|---|---|
| PresentationXsl | The XSL stylesheet used to control the presentation of the data | String | Load from DefaultXsl.xslt | MCMS Listing Presentation |
| TestXml | XML for testing the transformation without connecting to MCMS | String | Load from TestXml.xml | MCMS Listing Presentation |

After the Web Part has been added to a Web Part page, its custom properties can be modified in the browser by selecting Modify Shared Web Part (or Modify Personal Web Part) from the top right corner of the Web Part.

![](img/442e26bf28f2df81fbf8dff3478f88e3_222_0.png)

As you can see above, the controls that are used for each property will vary based on the data type. The property we specified earlier, Mode, is of type enumMode, which has four possible values so the ToolPane renders a drop-down list. The ToolPane will also render a textbox for numbers and strings. Custom properties can be grouped using the Category attribute on the property and this allows us to group multiple properties, for example MCMS Listing Configuration.

## Developing Custom Properties for the Web Part

In NavigationWebPart.cs, we'll remove some additional code that has been added by the template before we create our properties. Remove all the generated comments and the following:

- - **DebugShowException**—used to test the actual MCMS data together with the current configured stylesheet. If exceptions occur, they will be displayed in clear text—something you would avoid on a production site.
- - **Production**—similar to DebugShowException but hides the exception details and displays a friendly error message.

```
#region declarations - enumerations, constants and private variables

// The mode of the Web Part used for troubleshooting and exception handling.
public enum enumMode
{
    DebugWithTestData,   // Render the Web Part using the XSLT and TestXml
    DebugShowXml,        // Output the XML
    DebugShowException,  // If an exception is thrown return the details
    Production           // Mode for running in production, suppress error
                         // messages.
}
```

The second enumeration, enumSortBy, specifies the sort order of the channel items returned by MCMS:

```
public enum enumSortBy
{
    Default,      // No Sort
    ChangeDate,
    DisplayName,
    DisplayPath,
    ExpiryDate,
    Importance,
    Ordinal,
    StartDate
}
```

```
#endregion
```

In the next step, we define the custom properties for the Web Part. Each public property will get and set a private variable and will also have a default value. The following table shows the Web Part Framework attributes assigned to each of the public properties:

| Attribute Name | Description |
|---|---|
| Browsable | Must be set to True for the custom property to appear in the ToolPane. |
| Category | Defines the grouping in the ToolPane. If this value is left empty or is not defined, the category will be Custom Properties. |
| DefaultValue | The value of the Web Part will be stored in the Web Part storage system if it differs from this attribute. |
| Description | The tooltip that will be displayed over the friendly name in the ToolPane. |
| FriendlyName | The name that will be displayed in the ToolPane. |
| ReadOnly | If this is set to True, the custom property will be displayed in the ToolPane but the user will be unable to modify it. |
| WebPartStorageAttribute | Custom properties can be stored at a per-user level by setting this to Storage.Personal or on a global level by using Storage.Shared. If it is set to Storage.None the value will not be stored in the Web Part storage system and the property will not appear in the ToolPane. |

Required properties for our Web Part are the mode it should be running in (Mode), the column to use for sorting (SortBy), the channel that should be enumerated (StartChannel), and a property to define the XSLT stylesheet to be used for rendering (PresentationXsl).

We'll also have a property (TestXml) that is only used in DebugWithTestData mode. This property will allow us to configure the test XML data to be used when debugging the XSLT stylesheet.

```
```csharp
#region properties

// Declare the private variables for the Mode property
// By default we run the Web Part in Production mode (suppressing exceptions)
const NavigationWebPart.enmMode _ModeDefault =
    NavigationWebPart.enmMode.Production;
private NavigationWebPart.enmMode _Mode = _ModeDefault;

// Set the attributes for the property
[ Browsable(true) ]
[ Category("MCMS Listing Mode") ]
[ DefaultValue(_ModeDefault) ]
[ Description("The mode of the Webpart.") ]
[ FriendlyName("Mode") ]
[ WebPartStorageAttribute(Storage.Shared) ]
public NavigationWebPart.enmMode Mode
{
    get{ return _Mode; }
    set{ _Mode = value; }
}
```
```

The SortBy property sets the sort order of the channel items returned by MCMS. By default the channel items will be sorted by ordinal.

```
```csharp
// Declare the start channel private variables
const NavigationWebPart.enmSortBy _SortByDefault =
    NavigationWebPart.enmSortBy.Ordinal;
private NavigationWebPart.enmSortBy _SortBy = _SortByDefault;

// Set the attributes for the property
[ Browsable(true) ]
[ Category("MCMS Listing Configuration") ]
[ DefaultValue(_SortByDefault) ]
[ Description("The Sort order of the Webpart.") ]
[ FriendlyName("Sort By") ]
[ WebPartStorageAttribute(Storage.Shared) ]
public NavigationWebPart.enmSortBy SortBy
{
    get{ return _SortBy; }
    set{ _SortBy = value; }
}
```
```

The StartChannel property sets the path of the channel whose child items (channels and/or postings) we will list:

```
```csharp
// Declare the start channel private variables, default is /Channels/
const string _StartChannelDefault = "/Channels/";
private string _StartChannel = _StartChannelDefault;

// Set the attributes for the property
[ Browsable(true) ]
[ Category("MCMS Listing Configuration") ]
[ DefaultValue(_StartChannelDefault) ]
[ Description("The channel to list the child items of.") ]
[ FriendlyName("Start Channel") ]
[ WebPartStorageAttribute(Storage.Shared) ]
public string StartChannel
```

## Building SharePoint Web Parts

```
```csharp
{
    get{ return _StartChannel; }
    set{ _StartChannel = value; }
}
```
```

The next two properties we will add are the TestXml and PresentationXsl properties. Both properties will load data from a file stored in the Web Part resources folder (wpresources) if their value is not set. This ensures that the Web Part will have a default XSLT stylesheet and XML test data present when it is first added to a page:

```
```csharp
// Declare the Presentation XSLT private variables
// Default is empty because we will load from a file
const string _PresentationXslDefault = "";
private string _PresentationXsl = _PresentationXslDefault;

// Set the attributes for the property
[ Browsable(true) ]
[ Category("MCMS Listing Presentation") ]
[ DefaultValue(_PresentationXslDefault) ]
[ Description("The XSLT the MCMS listing will display.") ]
[ FriendlyName("Presentation Xsl") ]
[ WebPartStorageAttribute(Storage.Shared) ]
public string PresentationXsl
{
    get
    {
        if (_PresentationXsl == "")
        {
            // Load XSLT from the wpresources folder if PresentationXsl not set
            string XslFile = "./wpresources/ExtensibleMCMSPageListingWebPart/"
                           + "DefaultXsl.xslt";

            try
            {
                System.Xml.XmlDocument _doc = new System.Xml.XmlDocument();
                // Get the path of the DefaultXsl.xslt that is stored in
                // /wpresources/ExtensibleMCMSPageListingWebPart/
                _doc.Load(HttpContext.Current.Server.MapPath(XslFile));
                // Set the local variable to be the string representation
                // of the XSL file
                _PresentationXsl = _doc.OuterXml;
            }
            catch
            {
                _PresentationXsl = "Failed to load from "
                                  + HttpContext.Current.Server.MapPath(XslFile);
            }
        }
        return _PresentationXsl;
    }
    set{ _PresentationXsl = value; }
}

// Declare the TestXsl private variables
const string _TestXmlDefault = "";
private string _TestXml = _TestXmlDefault;

// Set the attributes for the property
[ Browsable(true) ]
[ Category("MCMS Listing Presentation") ]
[ Description("The Xml to be parsed for testing when in debug mode.") ]
[ FriendlyName("Test XML") ]
[ WebPartStorageAttribute(Storage.Shared) ]
[ DefaultValue(_TestXmlDefault) ]
```
```
