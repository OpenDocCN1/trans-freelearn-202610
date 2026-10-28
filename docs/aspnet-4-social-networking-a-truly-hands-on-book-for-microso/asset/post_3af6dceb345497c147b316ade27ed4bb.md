
## User Profiles

### Manage profile

As we know that this area has a big chance of growing and expanding as the site grows, we have decided to build this page using a series of wizard steps. The code for the UI is not difficult by any means, but it is longer than we want to show here in the text! So we will only show the steps for the wizard.

Here, we collect some attributes about the user's fish tank as shown in the following screenshot.

![](img/15a62e134135dde036b0ac3cbb01dab6_156_0.png)

Then, we gather the user's signature (which we can use for any postings to the forum or other features) as shown in the following screenshot:

![](img/15a62e134135dde036b0ac3cbb01dab6_156_1.png)

Next we collect all of the user's methods to communicate with other users in the site. We focus on collecting their Instant Messaging client IDs here.

![](img/15a62e134135dde036b0ac3cbb01dab6_157_0.png)

Finally, we have some attributes about the user that we can display on their profile page.

![](img/15a62e134135dde036b0ac3cbb01dab6_157_1.png)

The only steps that are a bit different from the norm are the first and the last steps. The first step's **level of experience** menu and all the inputs in the last step are dynamically generated.

During initialization, the presenter initiates the UI with a couple of previously discussed service methods.

```csharp
//Fisharoo/Web/Profiles/Presenters/ManageProfilePresenter.cs
public void Init(IManageProfile view, bool IsPostback)
{
    _view.LoadLevelOfExperienceTypes(
        _levelOfExperienceTypeService.
        GetAllLevelOfExperienceTypes());
    _view.LoadProfileAttributeTypes(GetProfileAttributeTypes());

    if (!IsPostback)
    {
        _view.LoadProfile(GetProfile());
    }
}
```

The presenter then feeds the data back to the UI to dynamically create the required controls, appropriately format them, and finally display the output.

```csharp
//Profiles/ManageProfile.aspx.cs
public void LoadProfileAttributeTypes(List<ProfileAttributeType> types)
{
    foreach (ProfileAttributeType type in types)
    {
        Label lbl = new Label();
        lbl.ID = "lblAttribute" + type.ProfileAttributeTypeID.ToString();
        lbl.Text = type.AttributeType;
        lbl.CssClass = "formLabel";

        Label lblAttributeTypeID = new Label();
        lblAttributeTypeID.ID = "lblAttributeTypeID" + type.ProfileAttributeTypeID.ToString();
        lblAttributeTypeID.Text = type.ProfileAttributeTypeID.ToString();
        lblAttributeTypeID.Visible = false;

        Label lblProfileAttributeID = new Label();
        lblProfileAttributeID.ID = "lblProfileAttributeID" + type.ProfileAttributeTypeID.ToString();
        lblProfileAttributeID.Visible = false;

        TextBox tb = new TextBox();
        tb.ID = "txtProfileAttribute" + type.ProfileAttributeTypeID.ToString();
        tb.TextMode = TextBoxMode.MultiLine;
        tb.Columns = 40;
        tb.Rows = 2;

        CustomValidator cv = new CustomValidator();
        cv.ControlToValidate = "txtProfileAttribute" + type.ProfileAttributeTypeID.ToString();
        cv.ClientValidationFunction = "MaxLength2000";
        cv.ErrorMessage = "This field can only be 2000 characters long!";
        cv.Text = "*";
        cv.ForeColor = System.Drawing.Color.Red;

        phAttributes.Controls.Add(lblAttributeTypeID);
        phAttributes.Controls.Add(lblProfileAttributeID);
        phAttributes.Controls.Add(lbl);
        phAttributes.Controls.Add(new LiteralControl("<br />"));
        phAttributes.Controls.Add(tb);
        phAttributes.Controls.Add(cv);
        phAttributes.Controls.Add(new LiteralControl("<br />"));
    }
}
```

Once the UI is created, the rest is a simple matter of plumbing and UI upkeep. Each time someone clicks the **Next** button, we clear the error message label. And when the **Finish** button is clicked, we extract the data from the UI and pass it to the presenter to be saved to the database.

### Avatar

Now let's look at creating the user's Avatar, that will be displayed next to all his/her interactions with the site.

### Upload avatar

Pretty much all the work for this feature occurs in the presenter. The UI presents a file upload box with a standard browse button and an additional button to submit the selected file. When the Submit button is clicked, the view passes the PostedFile to the UploadFile() method of the presenter.

```csharp
//Fisharoo/Web/Profiles/Presenters/UploadAvatarPresenter.cs
public void UploadFile(HttpPostedFile File)
{
    string extension = Path.GetExtension(File.FileName).ToLower();
    string mimetype;
    byte[] uploadedImage = new byte[File.InputStream.Length];
    switch (extension)
    {
        case ".png":
        case ".jpg":
        case ".gif":
            mimetype = File.ContentType;
            break;
        default:
            _view.ShowMessage("We only accept .png, .jpg, and .gif!");
            return;
            break;
    }
    if (File.ContentLength / 1000 < 1000)
    {
        File.InputStream.Read(uploadedImage, 0, uploadedImage.Length);
        profile.Avatar = uploadedImage;
        profile.AvatarMimeType = mimetype;
        profile.UseGravatar = 0;
        _profileRepository.SaveProfile(profile);
        _view.ShowCropPanel();
    }
    else
    {
        _view.ShowMessage("The file you uploaded is larger than the 1mb limit. Please reduce the size of your file and try again.");
    }
}
```

This method is responsible for receiving the file and performing some basic checks on the file. We first get the extension of the file being uploaded and check to make sure that the file that was uploaded is an image that we support. If not, we show a message stating the issue.

Once we make it past that check, we check to make sure that the file size is not too large. If the file is in an acceptable size, we read the file into the uploadedImage byte array. We then pass the byte array into the current profile's Avatar property along with its mimetype (we also set the UseGravatar flag to 0, which we will discuss shortly). Finally, we save this data to the user's profile and update the display to show the cropping UI. Alternatively, if the file size was too large, we show an error message.

### Image manipulation

To start with, the entire UI for this image-cropping tool is located at: http://www.defusion.org.uk/code/javascript-image-cropper-ui-using-prototype-scriptaculous/. This is an open source JavaScript tool that provides us with all the fancy interface options that can be dragged and resized. It uses prototype (http://www.prototypejs.org/), scriptaculous (http://script.aculo.us/), and some fancy footwork from Dave Spurr. To implement this script, download the source from the defusion site above and include the references to the prototype, scriptaculous, and cropper files in the UploadAvatar.aspx.

```
//Fisharoo/Web/Profiles/UpdateAvatar.aspx
<script type="text/javascript" src="/js/cropper/lib/prototype.js" language="javascript"></script>
<script type="text/javascript" src="/js/cropper/lib/scriptaculous.js?load=builder,dragdrop" language="javascript"></script>
<script type="text/javascript" src="/js/cropper/cropper.js" language="javascript"></script>
```

Once you have the JavaScript side plugged in, you need to add some items to your ASPX page to get this widget factory working. To start with, we need to add an image control that will load the image that we want to operate on.

```
<asp:Image ImageUrl="~/images/ProfileAvatar/ProfileImage.aspx" id="imgCropImage" runat="server"/>
```

> Note that this Image control has an ImageUrl reference to a page rather than an image. The images/ProfileAvatar/ProfileImage.aspx page displays a user's avatar depending on the current configuration of his/her profile. If it doesn't have an avatar, it shows a default avatar. If it is decided to use the Gravatar service instead (discussed shortly) it shows the avatar stored on Gravatar.

This is optional, but the cropper script provides us with a preview of what we are doing. To implement this, we need to have a location for the preview to output in the form of a div tag.

```html
<div id="previewWrap"></div>
```

We then need to add a JavaScript function that hooks up the cropper scripts to our UI. This script sets some of the basic properties such as what the image's ID is, where to stash the output for the preview, the minimum height and width, a fixed ratio (forcing the cropper to be a square in our case), and what to do when the cropping is complete. Also note that the name of the image control (Content_imgCropImage) is unlike what you would have typically seen working with earlier versions of ASP.NET. With ASP.NET 4, the control name is much simpler to get to and no longer uses the ctl00 prefix.

```javascript
<script type="text/javascript" language="javascript">
    Event.observe(window, 'load', function() {
        new Cropper.ImgWithPreview(
            'Content_imgCropImage',
            {
                previewWrap: 'previewWrap',
                minWidth: 100,
                minHeight: 100,
                ratioDim: {x: 100,y: 100},
                displayOnInit: true,
                onEndCrop: onEndCrop
            }
        );
    });
</script>
```

We want the cropper to store some of the data that is captured in some hidden fields so that we can work with it on the server side. We then need to add a function that handles the onEndCrop event that is fired off by the cropper. This will actually handle storing the data into the HiddenField controls. We skip showing this part of the code as it is fairly trivial.

Moving on, once a user has selected the area of the image that he/she would like to use for the avatar and submits that selection to the server, we have what we need to perform an image crop. The presenter's CropFile() method will handle this task.

```csharp
//Fisharoo/Web/Profiles/Presenters/UploadAvatarPresenter.cs
public void CropFile(Int32 X, Int32 Y, Int32 Width, Int32 Height)
{
    byte[] imageBytes = profile.Avatar.ToArray();
```

