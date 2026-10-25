```
return RedirectToAction("Index");
}
else
{
    ViewBag.Message = "Error occurred while validating the user";
}

return View(user);
}
```

The preceding implementation illustrates how we perform manual validation, and it does the following:

+   - Maps the data from **Registration View Model** to **User Data Model**
+   - We should always suspect that the mapped data may not be valid
+   - It is always safer to validate the mapped data manually before we persist the data back to the data store
+   - If we have any validation errors, we would be publishing the information to the user through the `ViewBag` message

The `IsValid` property of the model would be false before we trigger `TryValidateModel`, and later it will be updated based on validation. We have seen how to perform manual validation, and now we will move to custom validation in the next section.

# Custom validation

The built-in validations exposed in the framework may be solving most of our technical validation problems, but there may be some business needs/requirements that require additional provision on top of built-in validations.

The custom validation can be performed at two levels:

+   - Field level
+   - Class level

The field-level custom validator can be created using the following:

+   - Inherit the custom validation class from the `ValidationAttribute` base type
+   - Override the `IsValid` method and handle your business validation
+   - The `IsValid` method accepts two parameters—value and `validationContext`