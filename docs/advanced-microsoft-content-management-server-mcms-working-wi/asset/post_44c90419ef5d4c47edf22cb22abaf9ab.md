
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

> Files that are to be added to the resource gallery must first be uploaded to the MCMS server as physical files. The ResourceGallery.CreateResource() method only accepts a valid path to an existing file in the file system.

Save and build the solution. To test the new dialog:

- 1. Open CMS Explorer.
- 2. Click Resources and navigate to the PlantCatalog resource gallery.
- 3. Select New | New Resource from the toolbar.
- 4. In the Create Resource dialog, enter the following values:
- 5. Click Create Resource.

| Property | Value |
|---|---|
| DisplayName | MyResource |
| Description | This resource was created from the PAPI |
| File | Select any file located on your desktop |

Watch as the selected file is uploaded and created as a new resource in the PlantCatalog resource gallery (click the Refresh button to see the latest view).

## Replacing Resources

When updating a resource, it is a good idea to replace its content instead of deleting it and uploading it again. Replacing means you preserve the GUID and URL so that hyperlinks to the resource in postings will not only be kept intact but also immediately linked to the updated content.

Using the PAPI, resources are replaced using the Resource.SetContentFromFile() method. In this example, we will build a new dialog for replacing resources in CMS Explorer:

- 1. Add a new web form to the CMSEXplorer project. Name the new web form ReplaceResource.aspx.
- 2. In Design view, drag styles.css from Solution Explorer and drop it onto the web form.
- 3. Toggle to HTML view and set the form's ENCTYPE property to multipart/form-data:
- 4. Add a table with four rows:
- 5. In Design view, drag and drop controls and set them up as detailed below:

```
<form id="Form1" method="post" runat="server"
    enctype="multipart/form-data">
</form>
```

```
<table>
<tr>
    <td colspan="2">
        <h1>Replace Resource</h1>
        <h2>Current Resource: (Add Literal for displaying the path here)</h2>
    </td>
</tr>
<tr>
    <td><INPUT type="file" id="File1" runat="server"></td>
</tr>
<tr>
    <td colspan="2">(Add the Label for error messages here)</td>
</tr>
<tr>
    <td colspan="2" align="right">
        (Add the Replace Resource button here)
        <INPUT type="button" value="Close" onclick="javascript:window.close();">
    </td>
</tr>
</table>
```

| Control | Property | Value |
|---|---|---|
| Literal | ID | litCurrentResource |
| Label | ID | lblErrorMessage |
|  | Text | (empty string) |
| Button | ID | btnReplace |
|  | Text | Replace Resource |

- 8. Switch to Design view and double-click on the btnReplace button. In the btnReplace_Click() event handler, add the following code:

```
private void btnReplace_Click(object sender, System.EventArgs e)
{
    try
    {
        if (File1.PostedFile != null)
        {
            // the temp folder for saving the posted file to
            string filePath = CmsHttpContext.Current.TemporaryUploadFolder;

            // get the file name from the posted file
            string fileName = File1.PostedFile.FileName;
            int indexOfSlash = fileName.LastIndexOf("\\") + 1;
            fileName = fileName.Substring(indexOfSlash, fileName.Length - indexOfSlash);

            // check to see if the file name is valid
            if (!Utility.ValidateCMSObjectName(fileName))
            {
                throw new Exception("Specified template gallery name is not "
                    + "valid. Must be only alphanumeric "
                    + "characters, open or close parens, "
                    + "hyphens, underscores, periods, or "
                    + "spaces. No period at the end and no "
                    + "consecutive periods are allowed.");
            }

            // append the file name to the path of the temp folder
            filePath += "\\" + fileName;

            // save the file to a specified temp folder
            File1.PostedFile.SaveAs(filePath);

            // replace the contents of the resource
            currentResource.SetContentFromFile(filePath);
            currentResource.Name = fileName;

            // commit the changes
            cmsContext.CommitAll();

            // display the success message
            lblErrorMessage.Text = "Resource replaced successfully!";
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

## Managing Templates, Template Galleries, and Resources

We first retrieve the name of the file to be uploaded and check to see if it is valid. After saving the uploaded file to the Temp folder in the MCMS Server directory, we call the Resource.SetContentFromFile() method, which accepts the path of a valid file in the file system. Just as when creating resources, this file must reside on the MCMS server. After the contents of the resource are updated, we commit the changes to the content repository.

Save and build the solution. Let's try out the Replace Resource dialog:

- 1. Open CMS Explorer.
- 2. Click Resources and navigate to the PlantCatalog resource gallery.
- 3. Click the Edit link next to the CoconutTree.JPG resource.
- 4. Click Replace.
- 5. In the Replace Resource dialog, browse to an existing JPG file.
- 6. Click Replace Resource.
- 7. Click Close to close the dialog.

The content of the resource is replaced with the newly uploaded file. Notice that you are only allowed to replace the resource with a file of the same MIME Type. For example, the CoconutTree.JPG resource could only be replaced with other *.JPG files. Should you have chosen, say a *.XLS file, you would have gotten the following error message:

MIME Content-Types do not match. application/vnd.ms-excel does not match image/jpg.

## Deleting Objects

The PAPI has a Delete() method for each channel, posting, template, template gallery, resource gallery, and resource object. We have created quite a few test objects in the repository, so let's write a Delete dialog so that CMS Explorer can remove them:

- 1. Add a new web form to the CMSExplorer project and name it Delete.aspx.
- 2. In Design view, drag and drop styles.css from Solution Explorer onto the web form.
- 3. Toggle to HTML view and add a table with four rows:

```
<table>
<tr>
  <td><h1>Confirm Delete</h1></td>
</tr>
<tr>
  <td>
    Click YES to permanently delete
    (Add literal to show the path of the item to be deleted here)
  </td>
</tr>
<tr>
  <td align="right">
    (Add the Yes button here)
    <INPUT type="button" value="No" onclick="javascript:window.close();" style="width:120px">
  </td>
</tr>
<tr>
  <td>(Add label for displaying error messages here)</td>
</tr>
</table>
```
