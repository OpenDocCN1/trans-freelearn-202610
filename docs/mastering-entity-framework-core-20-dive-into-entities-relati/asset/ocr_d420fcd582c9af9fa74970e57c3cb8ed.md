```
return View(await _context.Blogs.FromSql("Select * from dbo.Blog").ToListAsync());
}

private void SetTenantId()
{
    if (this.User == null) return;
    var person = _context.People.FirstOrDefault(item =>
        item.User.Username.Equals(this.User.Identity.Name));
    if (person != null)
    {
        _context.TenantId = person.TenantId.HasValue ?
            person.TenantId.Value : Guid.Empty;
    }
    return;
}
```

Do not forget to drop the predicates included in the previous section, otherwise, the application may not yield any results from the Blog or Post entities. Execute the following SQL query to remove predicates from the tenant policy:

```
ALTER SECURITY POLICY dbo.TenantPolicy
DROP FILTER PREDICATE ON dbo.Blog,
DROP FILTER PREDICATE ON dbo.Post,
DROP BLOCK PREDICATE ON dbo.Blog,
DROP BLOCK PREDICATE ON dbo.Post
GO
```

The **Blog** listing page looks like the following screenshot before the tenant configuration is in place, which contains all the blog information even though the user from Prabhakar-Tenant tenant is logged in:

![](bbox=[0.1, 0.66, 0.9, 0.88])