We could simplify the approach by removing the person retrieval and creating a Person object using just the Id value, and updating the state of the entity as Deleted would instruct EF Core to perform a delete operation directly without retrieving the object from the data store. The following code reduces the query into one which performs delete operation alone:

```
[HttpPost, ActionName("Delete")]
[ValidateAntiForgeryToken]
public async Task<IActionResult> DeleteConfirmed(int id)
{
    Person person = new Person() { Id = id };
    _context.Entry(person).State = EntityState.Deleted;
    await _context.SaveChangesAsync();
    return RedirectToAction("Index");
}
```

We can see that the preceding approach does not make any Person retrievals yet. Still, the delete operation was performed successfully, as shown in the following screenshot:

![](bbox=[0.199, 0.403, 0.801, 0.797])