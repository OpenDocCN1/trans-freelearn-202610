
![](img/442e26bf28f2df81fbf8dff3478f88e3_370_0.png)

A project should now be created and you will notice all the required MCMS references are already created. You can delete the Console folder from the project as we do not need it.

## Creating the Business Layer

Now we have created our project let's look at the business layer. The classes in the business layer will encapsulate the key properties of MCMS objects such as Name, Path, Value, and ID (equating to MCMS's Guid property). All the objects in our object model must be serializable, that is, it must be possible to convert them into XML so they can be transferred via Web services. We cannot transfer the standard PAPI objects directly via ASP.NET Web services as there is no immediate way to serialize them to XML. Therefore, we will be recreating portions of the MCMS object model in our own classes that do support serialization so they can be sent and received by ASP.NET Web services.

The PAPI classes do not contain default constructors, hence cannot be serialized into XML. For more on XML Serialization see http://msdn.microsoft.com/library/default.asp?url=/library/en-us/cpguide/html/cpconintroducingxmlserialization.asp.

We will develop the following classes:

- LightweightPosting
- LightweightTemplate
- LightweightPlaceholderCollection
  - LightweightPlaceholder
- LightweightCustomPropertyCollection
  - LightweightCustomProperty

- 4. For an object to be serialized, it must have a constructor that takes no arguments. This default constructor is needed by the .NET Framework during serialization, and is added by VS .NET when we create a class. However, we also need to add the following constructor that accepts values for the public variables as its arguments:

```csharp
/// <summary>
/// Instantiate the custom property setting the Name and Current value.
/// </summary>
/// <param name="_Name">The name of the custom property</param>
/// <param name="_Value">The current value of the custom property</param>
public LightweightCustomProperty(string _Name, string _Value)
{
    this.Name = _Name;
    this.Value = _Value;
}
```

- 5. The custom property values will be saved back to the relevant MCMS posting. So let's create the Save() method, which sets the custom property's value for the posting passed in as a parameter.

```csharp
/// <summary>
/// Save the custom property value
/// </summary>
/// <param name="_Posting">The posting to be updated</param>
public void Save(Posting _Posting)
{
    // Get the custom property of the posting and set the value
    _Posting.CustomProperties[this.Name].Value = this.Value;
}
```

A posting may use a template with multiple custom properties defined. To store this collection of custom properties, we will define a strongly typed collection of LightweightCustomProperty objects called LightweightCustomPropertyCollection.

- 1. Create a new class in Visual Studio .NET and give it a name of LightweightCustomPropertyCollection. Add the namespace Microsoft.ContentManagement.Publishing to it, and make it extend System.Collections.CollectionBase.

```csharp
using System;
using Microsoft.ContentManagement.Publishing;
namespace McmsAuthoringWebService
{
    /// <summary>
    /// Summary description for LightweightCustomPropertyCollection.
    /// </summary>
    public class LightweightCustomPropertyCollection
        : System.Collections.CollectionBase
    {
        . . . code continues . . .
    }
}
```

- 2. As LightweightCustomPropertyCollection is a strongly typed collection, the default property will be a LightweightCustomProperty.

```csharp
/// <summary>
/// The indexed LightweightCustomProperty
/// </summary>
public LightweightCustomProperty this[int index]
{ get { // Cast the list item to a LightweightCustomProperty return(this.InnerList[index] as LightweightCustomProperty); } set { this.InnerList[index] = value; } }
```

- 3. We will now define the Add() method that we will need in order to add a LightweightCustomProperty to the collection.

```csharp
/// <summary>
/// Add an item to the collection
/// </summary>
/// <param name="item">The LightweightCustomProperty to be added</param>
public void Add(LightweightCustomProperty item)
{
    this.InnerList.Add(item);
}
```

- 4. The LightweightCustomPropertyCollection will be populated by a Microsoft.ContentManagement.Publishing.Posting object so we will define the Load() method. The Load() method will enumerate the custom properties of the posting and for each custom property it will instantiate a LightweightCustomProperty and add it to the collection.

  For the purposes of this example we are only maintaining custom properties of type Text. This code could be extended to support custom properties of type Selection.

```csharp
/// <summary>
/// Load the lightweight custom property collection from a posting.
/// </summary>
/// <param name="_Posting">Posting to be loaded from</param>
public void Load(Posting _Posting)
{
    if (_Posting != null)
    {
        // Loop through the custom properties
        foreach (CustomProperty _CustomProperty in _Posting.CustomProperties)
        {
            // Check if the custom property is of type `text`
            // (custom properties will have > 0 allowed values)
            if (_CustomProperty.AllowedValues.Count == 0)
            {
                // Instantiate a lightweight custom property and
                // add it to the collection
                this.Add(new LightweightCustomProperty(_CustomProperty.Name, _CustomProperty.Value));
            }
        }
    }
}
```

- 5. We also need to be able to save all the custom properties that are held in the collection.

```csharp
/// <summary>
/// Save the collection of custom properties
/// </summary>
/// <param name="_Posting">
/// Posting to save the custom property value to
/// </param>
public void Save(Posting _Posting)
{
    // Loop through the custom properties in the collection
    foreach (LightweightCustomProperty _LightweightCustomProperty in this)
    {
        // Save the custom property against the posting
        _LightweightCustomProperty.Save(_Posting);
    }
}
```

Now we have defined our classes for maintaining the custom properties, the project should look like this in Class View:

![](img/442e26bf28f2df81fbf8dff3478f88e3_374_0.png)

## Creating the Lightweight Template Class

The template's path and GUID will be included in the InfoPath form so users know what template a posting is using. To do this we will create the LightweightTemplate class, which is a serializable cut-down representation of the Microsoft.ContentManagement.Publishing.Template class.

- 1. In Visual Studio .NET, right-click on the McmsAuthoringWebService project and create a new class called LightweightTemplate.
- 2. Add the Microsoft.ContentManagement.Publishing namespace.

```csharp
using System;
using Microsoft.ContentManagement.Publishing;
namespace McmsAuthoringWebService
{
    /// <summary>
    /// Summary description for LightweightTemplate.
    /// </summary>
    public class LightweightTemplate
    {
        ... code continues ...
    }
}
```

- 3. Define the following public variables in the class:

```csharp
/// <summary>
/// The name of the template in MCMS
/// </summary>
public string Name;

/// <summary>
/// The path of the template in MCMS
/// </summary>
public string Path;

/// <summary>
/// The Guid of the template in MCMS
/// </summary>
public Guid ID;
```

- 4. As the template object will be loaded from an instance of the Microsoft.ContentManagement.Publishing.Template class, we will now define the Load() method:

```csharp
/// <summary>
/// Load the lightweight object from the MCMS Template
/// </summary>
/// <param name="_Template">The MCMS template to load from</param>
public void Load(Template _Template)
{
    this.Name = _Template.Name;
    this.Path = _Template.Path;
    // Create a new guid object because MCMS stores Guids in string format.
    this.ID = new Guid(_Template.Guid);
}
```

## Maintaining Placeholder Values

The ASP.NET Web service will be able to load and maintain placeholder values of postings. To do this we will create two objects, namely LightweightPlaceholder and a corresponding LightweightPlaceholderCollection.

- 1. In Visual Studio .NET, right-click on the project and create a new class called LightweightPlaceholder.
- 2. Add the following namespaces to the file:

```csharp
using System;
using System.Xml;
using System.IO;
using Microsoft.ContentManagement.Publishing;
using Microsoft.ContentManagement.Publishing.Extensions.Placeholders;
```
