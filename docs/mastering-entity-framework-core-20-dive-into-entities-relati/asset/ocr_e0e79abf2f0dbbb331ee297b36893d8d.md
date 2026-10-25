![](bbox=[0.24, 0.1, 0.75, 0.5])

When we include the Comments navigational property in the LINQ query, it would perform eager loading of the Comments entity to serve its request. The following code would let the EF Core to perform eager loading the comments:

```
var people = await _context.People
.Include(item => item.Comments)
.AsNoTracking()
.ToListAsync();
var peopleViewModel = new List<PersonViewModel>();
people.ForEach(item =>
{
    var comment = item.Comments;
    peopleViewModel.Add(new PersonViewModel
    {
        Person = item,
        NoOfComments = item.Comments != null ?
        item.Comments.Count : 0
    });
});
```