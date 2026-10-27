This code employs the @helper syntax to create a new helper called PrimaryButtonSmall. The helper accepts two parameters in order to specify the id attribute of the button element as well as the button's caption element. We've added appropriate Bootstrap classes to the button element in order to render a blue primary button. Any custom helper must reside inside the App_Code folder in order for ASP.NET MVC to recognize it inside your view.

# Using a helper in a view

Let's use the helper we just created in the preceding section. To do this, open a view in your site, and add the following code to the page:

```
```csharp
@BootstrapHelpers.PrimaryButtonSmall("myButton","My New Button")
```
```

When you run the site and view the source of the page on which the helper is used, you'll see that the helper generated the correct HTML markup to create the following primary Bootstrap button:

```
```html
<button class="btn btn-primary btn-sm" type="button" id="myButton">My New Button</button>
```
```

You'll notice that although this approach works, it is not practical because it means we would have to create an HTML helper for each style and size of button available. A possible solution to this problem is to change our helper code in order to accept a parameter with which the developer can specify which style and size the button should be. To accomplish this, perform the following steps:

+   1. Open the BootstrapHelpers.cshtml file that is located inside the App_Code folder.
+   2. Replace the code inside the file with the following code:
+   
+      ```csharp
+      @helper Button(string style, string size, string caption, string id)
+      {
+          <button id="@id" type="button" class="btn btn-@style btn-@size">@caption</button>
+      }
+      ```
+   
+   3. Open the view that uses the helper and add the following markup:
+   
+      ```csharp
+      @BootstrapHelpers.Button("danger","lg","A Large button","myLargeButton")
+      ```

In the preceding step, we've updated our helper to accept two string parameters to specify the size and style of the Bootstrap button. The problem with this approach is that the developer still needs to know the name of the style and size in order to render the correct button.