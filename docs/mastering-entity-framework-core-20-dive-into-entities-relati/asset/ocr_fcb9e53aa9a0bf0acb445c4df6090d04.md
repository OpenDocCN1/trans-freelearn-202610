We have looked, exhaustively, at the Compare field validations. Let's explore the Url validation in the next section.

# Url field validation

We will look into Url validation, how it should be configured, and how it works with the MVC engine. It does the basic syntax check on the URL value rather than verifying the URL itself, so it would be ideal to have a consistent mechanism to perform, such as verification, to have uniform behavior between systems. The Url validation is performed based on the pattern -

<protocol>://<domain-name>.<extension>. Let's explore the pattern in detail:

+   - **Protocol:** It should be HTTP, https, or FTP
+   - **Domain name:** It should be one or more characters
+   - **Extension:** It should be two or more characters (for instance, http://a.bc is valid since it follows the preceding pattern)

The Url attribute/data annotation can be used in the following ways:

```
public class Blog
{
    // Code removed for brevity
    [Required(ErrorMessage = "Url is required")]
    [Url]
    public string Url { get; set; }
}
```