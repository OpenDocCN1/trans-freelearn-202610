```
<p>
    <label for="email">Email </label>
    <input type="text" id="email" name="email" ngModel>
</p>
<button type="submit">Submit</button>
</form>
```

# When to use template-driven forms

Template-driven forms are very flexible and easy to implement in Angular applications. However, this approach has some limitations and can cause an impact in terms of maintainability; some of the best scenarios for using a template-driven approach in building forms are set out here:

+   - It’s easier to use template-driven forms when migrating from AngularJS to Angular2, such that both use the ngModel directive.
+   - Template-driven forms are more suitable in simple and small forms that do not require complex validations since validation is applied at the template level. This could be a disadvantage as it will be hard to maintain validations on larger applications at the same time. It has limitations on applying validations to the form controls.

In the second of the aforementioned scenarios, reactive forms are chosen over template-driven forms as complex forms can be handled better with reactive forms, especially in implementing validations. Let’s now understand the concept of reactive forms.

# The reactive approach

A reactive form is the second approach in building forms in Angular applications; this is the most commonly used approach as it is more effective in handling complex forms than template-driven forms. Reactive forms are also known as model-driven forms, in which we define the structure of our forms in the component class instead of defining it in the template.

We also define the validations in the class before we bind it in to our HTML form, which means that the logic and validation patterns will now be separated from the HTML template and will be maintained by the TypeScript side of our component.

# Using reactive forms

The first step for us to use reactive forms is to import ReactiveFormsModule; this is usually imported into the root module or the shared module of the application. ReactiveFormsModule contains all directives—such as formGroup and formControlName—that will allow us to implement reactive forms; this is also found under the @angular/forms package.