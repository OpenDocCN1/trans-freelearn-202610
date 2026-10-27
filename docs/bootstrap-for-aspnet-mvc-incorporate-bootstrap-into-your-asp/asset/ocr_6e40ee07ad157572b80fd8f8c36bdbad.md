# The Content folder

The Content folder contains both the bootstrap.css and bootstrap.min.css files as well as a style sheet called Site.css. This file is used to apply any additional styling on top of the default styles provided by Bootstrap, and it is also used to specify the styles to use for the jQuery validation plugin required by ASP.NET MVC for form validation. For example, the following CSS highlights any input element with a reddish color and draws a border around the element if the validation for that field failed:

```
.field-validation-error {
    color: #b94a48;
}

.field-validation-valid {
    display: none;
}

input.input-validation-error {
    border: 1px solid #b94a48;
}

input[type="checkbox"].input-validation-error {
    border: 0 none;
}

.validation-summary-errors {
    color: #b94a48;
}

.validation-summary-valid {
    display: none;
}
```

# The fonts folder

The fonts folder contains the Glyphicons font in all the necessary formats.