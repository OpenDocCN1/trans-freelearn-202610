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
```