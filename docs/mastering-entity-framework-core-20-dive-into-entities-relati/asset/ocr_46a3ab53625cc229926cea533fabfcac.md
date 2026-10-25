After addressing the concerns, we have a fair idea of what we will be covering in this chapter:

+   - Client-side validation
+   - Consequences if the client-side scripting stops working
+   - Server-side validation
+   - Manual validation
+   - Custom validation
+   - Create client-side logic for custom validation
+   - Remote validation

# Client-side validation

The client-side validation is really a boon to modern web applications. There were many instances that proved jQuery is helpful on both the development and the user experience front. The client-side validations were the biggest advantage we had in saving multiple round trips between client/browser to the server just to perform basic validations.

The jQuery, jQuery Validate, and jQuery Validate Unobtrusive libraries helped us in performing those basic validations on the client (browser) end rather than relying on the server in performing the same. There were multiple discussions or even arguments regarding validating the models at the client-side, which opens security considerations. The following sections will address this issue.

The task to enable client-side validation is just to provide a section for MVC to inject validation errors in the user interface, which has nothing to do with validation-related attributes (as it will be available in the model):

```
```html
<div class="form-group">
    <label asp-for="Title" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <input asp-for="Title" class="form-control" />
        <span asp-validation-for="Title" class="text-danger"></span>
    </div>
</div>
```
```