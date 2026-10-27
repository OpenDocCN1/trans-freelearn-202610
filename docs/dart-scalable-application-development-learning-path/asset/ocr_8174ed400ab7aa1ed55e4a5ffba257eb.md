# Polymer Dart Recipes

```
```html
</div>
</template>
<script type="application/dart" src="pol_text.dart"></script>
</polymer-element>
```
```

![](bbox=[0.225, 0.219, 0.775, 0.391])

# How it works...

The input (content) from the simple text field or text area is bounded by the expression `value = {{variable}}` to variable. It shows up in the simple `{{variable}}` expression on the page, but also immediately reflects in the other input field, which proves that we have two-way data binding.

# Binding to a checkbox

In this recipe, we will show you how to bind an input value from a checkbox to a variable; so in effect, we now perform data binding from the UI to the code and have two-way data binding. You can find the code in the project `pol_check`.

# How to do it...

+   1. The script starts with `web\index.html`, where a component with the name `pol-check` is imported through the following line:
+   
+      ```html
+      <link rel="import" href="pol_check.html">
+      ```
+   
+      From this, we know that the component is defined in `pol_check.html`, and the code behind it is in a file named `pol_check.dart`. For a discussion of the other tags, refer to the first recipe.