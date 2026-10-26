```
```csharp
+ "spaces. No period at the end and no "
+ "consecutive periods are allowed. ");
}

// save the file to a specified temp folder
File1.PostedFile.SaveAs(filePath);

// create the new resource
Resource newResource = parentGallery.CreateResource(filePath);
newResource.Name = fileName;
newResource.DisplayName = txtDisplayName.Text;

// commit the changes
cmsContext.CommitAll();

// delete the file that was saved to the temp folder
System.IO.File.Delete(filePath);

// display the success message
lblErrorMessage.Text = "Resource created successfully!";
}
else
{
    // no file was selected for uploading
    lblErrorMessage.Text = "Please select a file to upload";
}
}
catch(Exception ex)
{
    // rollback all changes
    cmsContext.RollbackAll();

    // the CMS context needs to be disposed of after a rollback
    cmsContext.Dispose();

    // display error message
    lblErrorMessage.Text = ex.Message;
}
}
```
```

> Files that are to be added to the resource gallery must first be uploaded to the MCMS server as physical files. The ResourceGallery.CreateResource() method only accepts a valid path to an existing file in the file system.

Save and build the solution. To test the new dialog:

+   1. Open CMS Explorer.
+   2. Click Resources and navigate to the PlantCatalog resource gallery.
+   3. Select New | New Resource from the toolbar.
+   4. In the Create Resource dialog, enter the following values:
+   5. Click Create Resource.

| Property | Value |
|---|---|
| DisplayName | MyResource |
| Description | This resource was created from the PAPI |
| File | Select any file located on your desktop |