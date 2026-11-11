This example displays how to query Windows services by leveraging the win32_serverfeature class:

+   1. You first start by using the get-wmiobject cmdlet with the win32_serverfeature argument.
+   2. You then pipe the results to the Select cmdlet with the values of ID, Name, and ParentID.
+   3. After execution, the Windows features that are installed are displayed with their ID, Name, and ParentID properties.

> This section leverages a prepopulated XML file for Windows Feature ID searching. To obtain this file, you can download it from this book's resources page.

When you query the win32_serverfeature class, the feature and role parent information are returned as IDs. When you want to determine what the ParentID is for the feature or role, you have to come up with an alternative method to perform the correlation. In this chapter, you will leverage the pre-populated ServerFeatureIDs.xml file from the examples of this book to correlate the ID to their friendly names. The ServerFeatureIDs.xml file should be copied to the c:\temp\POSHScript\ directory.

To call the win32_serverfeature class, query features and roles with ParentIDs, and correlate them by leveraging an XML file, you can do the following:

```
[xml] $frxml = Get-Content "C:\temp\POSHScript\ServerFeatureIDs.xml"
$featRoleTable = $frxml.GetElementsByTagName("feature")
$crntFeatures = Get-wmiobject win32_serverfeature
foreach ($feature in $crntFeatures) {
    $featurename = $feature.Name
    $featureparentID = $feature.ParentID
    if ($featureparentID) {
        $featureparentName = ($featRoleTable | where {$_ .ID -eq $featureparentID}).Name
        Write-host "The $featurename feature has the parentID of $featureparentID. Parent Name: $featureparentName."
    }
}
```