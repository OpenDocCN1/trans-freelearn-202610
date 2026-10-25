Finally, the `Timestamp` column should be configured as a hidden column, just like we would perform for an identity column. This is required since the UI should retain the timestamp value on the `Post`:

```
```html
// Code removed for brevity
<form asp-action="Edit">
    <div class="form-horizontal">
        // Code removed for brevity
        <input type="hidden" asp-for="Timestamp" />
        // Code removed for brevity
    </div>
</form>
// Code removed for brevity
```
```

We are not there yet; we have updated the entity, but we are supposed to update the database. So, we need to update the schema as shown in the following screenshot, by adding migration and updating the database:

![](bbox=[0.104, 0.422, 0.894, 0.669])