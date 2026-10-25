The tags were populated in the view as a `select` control assigning source from `ViewBag`. Also, we need to ensure the `multiple` attribute is enabled; otherwise, the control will not let the user select multiple tags, the `multiple` attribute is set as shown below:

```
```html
<form asp-action="Create">
    <div class="form-horizontal">
        // Code removed for brevity
        <div class="form-group">
            <label asp-for="TagIds" class="col-md-2 control-label"></label>
            <div class="col-md-10">
                <select asp-for="TagIds" class="form-control"
                        asp-items="ViewBag.TagIds" multiple="multiple"></select>
            </div>
        </div>
        // Code removed for brevity
    </div>
</form>
```
```

In the `Post` action, we still persist the `MultiSelectList`, but this time we pass on the selected `TagIds` along with the source data. This ensures that if there were any model errors while repopulating the view, the tags control will be pre-selected with the values from the model. The `TagIds` field should be included in the view model white-listing and finally the `MultiSelectList` is populated with the `TagIds` as shown in the following code:

```
```csharp
public async Task<ActionResult>
Create([Bind("Id,Title,Content,Summary,PublishedDateTime,Url,
VisitorCount,CreatedAt,ModifiedAt,BlogId,AuthorId,
CategoryId,TagIds")] Post post)
{
    // Code removed for brevity
    ViewData["TagIds"] = new MultiSelectList(_context.Tags, "Id",
        "Name", post.TagIds);
    return View(post);
}
```
```

Similar changes are required for the `Update` and `Delete` operations of posts. Let's take it up as an assignment; the solution to the assignment is available in the Git repository. This concludes the default behavior of transactions in this section; let's explore creating a simple transaction in the next section.