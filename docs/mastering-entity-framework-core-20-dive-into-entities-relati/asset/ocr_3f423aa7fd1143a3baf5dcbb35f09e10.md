We can configure the custom error message for each model field as follows:

```
public class RegistrationViewModel
{
    [Required(ErrorMessage = "Username is required")]
    [MinLength(6, ErrorMessage = "Username needs minimum 6 characters")]
    public string Username { get; set; }
    // Code removed for brevity
}
```

The preceding method will force EF to emit a custom error message, **Username needs minimum 6 characters**, if the field contains invalid data during validation, shown as follows:

![](bbox=[0.103, 0.36, 0.896, 0.63])

We have looked, exhaustively, at `MinLength` field validation; let's explore `MaxLength` validation in the next section.

# MaxLength field validation

The `MaxLength` validation is helpful for creating constraints on certain fields such as Username, Zip code, and so on. We will investigate `MaxLength` validation in this section.