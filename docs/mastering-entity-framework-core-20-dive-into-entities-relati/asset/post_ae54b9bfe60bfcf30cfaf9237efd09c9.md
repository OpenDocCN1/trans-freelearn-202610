
We have looked, exhaustively, at the Compare field validations. Let's explore the Url validation in the next section.

## Url field validation

We will look into Url validation, how it should be configured, and how it works with the MVC engine. It does the basic syntax check on the URL value rather than verifying the URL itself, so it would be ideal to have a consistent mechanism to perform, such as verification, to have uniform behavior between systems. The Url validation is performed based on the pattern -

<protocol>://<domain-name>.<extension>. Let's explore the pattern in detail:

- **Protocol:** It should be HTTP, https, or FTP
- **Domain name:** It should be one or more characters
- **Extension:** It should be two or more characters (for instance, http://a.bc is valid since it follows the preceding pattern)

The Url attribute/data annotation can be used in the following ways:

```csharp
public class Blog
{
    // Code removed for brevity
    [Required(ErrorMessage = "Url is required")]
    [Url]
    public string Url { get; set; }
}
```

The **Url** validation reports the following value as the error **The Url field is not a valid fully-qualified http, https, or ftp URL.** since the value contains only the protocol and does not satisfy the URL pattern:

![](img/17e01f90db603e72c0f0c6a7dce8920c_142_0.png)

If the field value follows the URL pattern properly, the error will be removed, and we can see that the field value seems to be valid (following the pattern) in the following screenshot:

![](img/17e01f90db603e72c0f0c6a7dce8920c_143_0.png)

We can configure the custom error message for each model field as follows:

```csharp
public class Blog
{
    // Code removed for brevity
    [Required(ErrorMessage = "Url is required")]
    [Url(ErrorMessage = "Provide a valid url")]
    public string Url { get; set; }
}
```

The preceding method will force EF to emit a custom error message, **Provide a valid url**, if the field contains invalid data during validation, shown as follows:

![](img/17e01f90db603e72c0f0c6a7dce8920c_144_0.png)

The displayed error message will be cleared once the user starts typing a valid URL value into the field.

### Incorporating Url validation in blogging system models

We will update our blogging system model to accommodate Url field validation, the data model driven Url validation is performed in the below entities:

```csharp
public class Blog
{
    // Code removed for brevity
    [Required(ErrorMessage = "Url is required")]
    [Url(ErrorMessage = "Provide a valid url")]
    public string Url { get; set; }
}
```

```csharp
public class Post
{
    // Code removed for brevity
    [Url(ErrorMessage = "Provide a valid url")]
    public string Url { get; set; }
}

public class Person
{
    // Code removed for brevity
    [Url(ErrorMessage = "Provide a valid url")]
    public string Url { get; set; }
    [Url(ErrorMessage = "Provide a valid image url")]
    public string ImageUrl { get; set; }
}
```

We have exhaustively seen the Url field validations; let's explore about the `MinLength` validation in the next section.

## MinLength field validation

The `MinLength` validation is helpful for creating constraints on certain fields such as Username, Zip Code, and so on. We will start investigating `MinLength` validation in this section.

The validation error can be captured in `MinLength` data annotation in the following scenarios:

- If the field is `NULL` or empty or whitespace.
- If the field value length is less than configured. For instance, if the `MinLength` is configured as 6, then if the number of characters provided in the field is less than 6 the min length validation error would be thrown.

The `MinLength` attribute/data annotation can be configured as follows:

```csharp
public class RegistrationViewModel
{
    [Required(ErrorMessage = "Username is required")]
    [MinLength(6)]
    public string Username { get; set; }
    // Code removed for brevity
}
```

If field value does not meet the minimum length requirement, then the validation error **The field Username must be a string or array type with a minimum length of '6'** would be thrown, whereas the field name and length of the error message is based on the field name and configuration, shown as follows:

![](img/17e01f90db603e72c0f0c6a7dce8920c_146_0.png)

Once satisfied with the length constraint, the error message will be removed from the user interface, shown as follows:

![](img/17e01f90db603e72c0f0c6a7dce8920c_146_1.png)

We can configure the custom error message for each model field as follows:

```csharp
public class RegistrationViewModel
{
    [Required(ErrorMessage = "Username is required")]
    [MinLength(6, ErrorMessage = "Username needs minimum 6 characters")]
    public string Username { get; set; }
    // Code removed for brevity
}
```

The preceding method will force EF to emit a custom error message, **Username needs minimum 6 characters**, if the field contains invalid data during validation, shown as follows:

![](img/17e01f90db603e72c0f0c6a7dce8920c_147_0.png)

We have looked, exhaustively, at `MinLength` field validation; let's explore `MaxLength` validation in the next section.

## MaxLength field validation

The `MaxLength` validation is helpful for creating constraints on certain fields such as Username, Zip code, and so on. We will investigate `MaxLength` validation in this section.

The validation error can be captured in the MaxLength data annotation if the field value length is greater than the configured length. For instance, if the maximum length is configured as 30, then if the length of total characters provided in the field is greater than 30, the max length validation error would be thrown.

The MaxLength attribute/data annotation can be configured as follows:

```csharp
public class RegistrationViewModel
{
    [Required(ErrorMessage = "Username is required")]
    [MinLength(6, ErrorMessage = "Username needs minimum 6 characters")]
    [MaxLength(30)]
    public string Username { get; set; }
    // Code removed for brevity
}
```

If the field value does not meet the maximum length requirement, then the validation error, The field Username must be a string or array type with a maximum length of '30', would be thrown, whereas the field name and length of the error message is based on the field name and configuration, as follows:

![](img/17e01f90db603e72c0f0c6a7dce8920c_148_0.png)

We can configure the custom error message for each model field, shown as follows:

```csharp
public class RegistrationViewModel
{
    [Required(ErrorMessage = "Username is required")]
    [MinLength(6, ErrorMessage = "Username needs minimum 6 characters")]
    [MaxLength(30, ErrorMessage = "Username cannot exceed 30 characters")]
    public string Username { get; set; }
    // Code removed for brevity
}
```

The preceding method will force EF to emit a custom error message, Username cannot exceed 30 characters, if the field contains invalid data during validation, shown as follows:

![](img/17e01f90db603e72c0f0c6a7dce8920c_149_0.png)

We have looked, exhaustively, at MaxLength field validation; let's explore RegularExpression validation in the next section.

## RegularExpression field validation

Regular expression validation is the only solution used for most validations such as email, phone number, zip code, username, password, and so on. Most of the patterns used with regular expressions are wrapped into separate validations. Still, the usage is vast and requires a method to define custom validation. That's where RegularExpression validation comes in handy. Let's investigate regular expressions in this section.

If the field value doesn't follow the defined regular expression pattern, then a validation error would be returned by the engine. For instance, if the regular expression is configured to contain only letters of the alphabet, then any other character inclusion would throw a validation error.

The RegularExpression attribute/data annotation can be configured as follows:

```csharp
public class Person
{
    public int Id { get; set; }
    [Required(ErrorMessage = "First Name is required")]
    [RegularExpression("^[a-zA-Z]+$")]
    public string FirstName { get; set; }
    // Code removed for brevity
}
```

If the field value does not match the regular expression, then the validation error, The field Username must match the regular expression '^[a-zA-Z]+$', would be thrown, which is shown as follows:

![](img/17e01f90db603e72c0f0c6a7dce8920c_150_0.png)

We can configure the custom error message on each model field, as follows:

```csharp
public class Person
{
    public int Id { get; set; }
    [Required(ErrorMessage = "First Name is required")]
    [RegularExpression("^[a-zA-Z]+$", ErrorMessage = "Only alphabets were allowed")]
}
```
