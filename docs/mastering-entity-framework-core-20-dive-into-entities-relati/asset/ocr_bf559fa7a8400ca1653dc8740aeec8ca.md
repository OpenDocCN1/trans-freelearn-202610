4. Click **Add**, shown as follows:

![](bbox=[0.344, 0.157, 0.657, 0.506])

5. The scaffolded code includes the CRUD operation in the MVC **Controllers** and **Views**. Examining the scaffolded MVC code would be out of the scope of this chapter, so we will focus on the EF scaffolded part alone:

```
public class BlogsController : Controller
{
    private readonly BlogContext _context;
    public BlogsController(BlogContext context)
    {
        _context = context;
    }
    // GET: Blogs
    public async Task<IActionResult> Index()
    {
        return View(await _context.Blogs.ToListAsync());
    }
    ...
}
```