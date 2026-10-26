```
left:-10px;\"> {0}</span><br>".
    posting.Description) );
}
// add the last date the posting was modified
html.Append(string.Format(
    "<span style=\"margin-left:10px; font-style:italic;\"
    + ">Last Modified: {0}</span>",
    posting.LastModifiedDate.ToString("F") )); 
// return the built HTML string
return html.ToString();
}
```

At this point the coding is complete within our custom view. We still have to strongly name our assembly, as required for any assembly running within the SharePoint environment.

In order to strongly name an assembly, we first need a key. To create a key, we'll run the Strong Name tool from a Visual Studio .NET Command Prompt:

+   1. Launch a command window by selecting Start | All Programs | Visual Studio .NET 2003, Visual Studio .NET Tools | Visual Studio .NET 2003 Command Prompt.

+   2. We'll place the key in the root of the C: drive for simplicity. Create a key by entering the following at the command prompt:
+   
+   sn -k c:\PlantCatalogCustomView.snk
+   
+   Now we need to tell the compiler to use the key we just created to strongly name our assembly when we build it. To do this, open the AssemblyInfo.cs file in our project and modify the version number and key file attributes as shown in the code below:
+   
+   [assembly: AssemblyVersion("1.0.0.0")]
+   [assembly: AssemblyKeyFile("c:\\PlantCatalogCustomView.snk")]

+   3. The final steps in creating a custom view involve building our assembly in release mode and deploying it to the global assembly cache (GAC). To add our assembly to the GAC, copy the release assembly version of our project, TropicalGreenPlantCatalogCustomView.dll, and paste it in the GAC, located at C:\Windows\assembly\.

+   4. Let's verify our custom assembly has been successfully installed to the GAC:

![](bbox=[0.225, 0.655, 0.77, 0.9])