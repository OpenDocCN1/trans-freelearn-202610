We have commented code in the repository implementations since, currently, it supports only one query handler for returning a single object and having a switch case doesn't make any sense. Once we start adding new implementations for handling single objects, we will be using commented code instead, which supports multiple queries through a single action:

```
public class PostsController : Controller
{
    // Code removed for brevity

    // GET: Posts/Details/5
    public async Task<IActionResult> Details(int? id)
    {
        if (id == null)
        {
            return NotFound();
        }

        var post = await _repositoryWithQueries.GetSingleAsync(_context,
            new GetPostByIdQuery(id, true));
        if (post == null)
        {
            return NotFound();
        }

        return View(post);
    }

    // GET: Posts/Edit/5
    public async Task<IActionResult> Edit(int? id)
    {
        if (id == null)
        {
            return NotFound();
        }

        var post = await _repositoryWithQueries.GetSingleAsync(_context,
            new GetPostByIdQuery(id, false));
        // Code removed for brevity
    }
}
```