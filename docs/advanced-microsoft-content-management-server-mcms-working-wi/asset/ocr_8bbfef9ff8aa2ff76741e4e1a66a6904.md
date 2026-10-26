```
<input type="hidden" value="" name="NCPHHRef_ImagePlaceholderDefinitionName"/>
<input type="hidden" value="" name="NCPHAltText_ImagePlaceholderDefinitionName"/>
<input type="hidden" value="" name="SingleImagePlaceholderControl1:AuthoringModeControlsContainer:ImageEditControl" />
- - - code continues - - -
```

The highlighted line shows the hidden form field that is used to store the name of the image that has been uploaded. To get its value, we simply use the following JavaScript:

```
<script language="javascript">
var fileName = document.all.NCPH_ImagePlaceholderDefinitionName.value;
</script>
```

> The other hidden form fields contain the alternate text and the hyperlink that the image points to when clicked. The alternate text is held in the NCPHAltText_NameOfPlaceholderDefinition hidden field. The NCPHHRef_NameOfPlaceholderDefinition hidden field corresponds to the hyperlink. Replace the text NameOfPlaceholderDefinition with the name of the placeholder as defined in Template Explorer.

# Checking for an Empty SingleImagePlaceholderControl

The NCPH_NameOfPlaceholderDefinition field contains the path and filename of the uploaded image. It stores an empty string when an image has not been uploaded to the placeholder control. To determine whether or not the author has left it blank, we will use the following JavaScript:

```
<script language="javascript">
function validateIDofPlaceholder()
{
var fileName = document.all.NCPH_ImagePlaceholderDefinitionName.value;
if (filename == "")
{
return false;
}
else
{
return true;
}
}
</script>
```

The string IDofPlaceholder will be replaced with the actual ID of the placeholder control. In this way, each placeholder control will have its own client-side validation function.

# The RequiredImagePHValidator

Let's build the RequiredImagePHValidator. First, add a class file named RequiredImagePHValidator.cs to the MCMSValidator project.

Add the required namespaces above the namespace declaration.

```
using System;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Text;
```