The preceding method would force EF to emit the standard validation error message, **The Url field is required**, if the field contains invalid data during validation. The scaffolded `<span>` element is used for populating the validation error as displayed in the following screenshot:

![](bbox=[0.257, 0.209, 0.743, 0.539])

We can also perform validation using a custom error message using the `ErrorMessage` field; the configuration is highlighted as follows:

```
public class Blog
{
    public int Id { get; set; }
    [Required]
    public string Url { get; set; }
    public ICollection<Post> Posts { get; set; }
    [Required(ErrorMessage = "Author is required, kindly pick one!")]
    public int AuthorId { get; set; }
    public User Author { get; set; }
}
```

The preceding configuration would use the custom error message in the `data-val-required` attribute instead of the standard validation error message: