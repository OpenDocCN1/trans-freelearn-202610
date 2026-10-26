```
```csharp
if (this.RenderUpLevel && this.EnableClientScript)
{
    // Perform validation only in authoring mode.
    if (WebAuthorContext.Current.Mode == WebAuthorContextMode.AuthoringNew
        || WebAuthorContext.Current.Mode == WebAuthorContextMode.AuthoringReedit)
    {
        writer.AddAttribute("evaluationfunction", "validate"
            + this.ControlToValidate);
    }
}
```
```

# Overriding the EvaluateIsValid() Method

To complete the code, we need to implement the EvaluateIsValid() method of the base class or the project will not compile. This is where we implement server-side checks. For now, we will leave the EvaluateIsValid() empty and program it to always return true. While this isn't something you should do in a production environment, don't worry. We'll be filling this method out later to provide server-side validation in the section *Implementing Server-Side Validation*. Add the EvaluateIsValid() method directly below the AddAttributesToRender() method.

```
```csharp
protected override bool EvaluateIsValid()
{
    bool valid = true;
    return valid;
}
```
```

The custom validation control is now complete. Save and compile the solution.

# Adding the Custom Validator to the Template File

Before using our custom control, we first need to copy it to the bin directory of our project file or add it as a reference to the MCMS web application project. After you have done that, add the control to the Visual Studio .NET Toolbox by choosing the Add/Remove Items option (or Customize Toolbox option for VS.NET 2002). You can then drag and drop it onto template files. For the control to work properly, you will have to set the ControlToValidate property to contain the value of the ID of the HtmlPlaceholderControl that you are validating and the ErrorMessage property to contain a friendly error message for the author.

![](bbox=[0.076, 0.656, 0.925, 0.882])