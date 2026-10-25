The preceding method would force EF to emit a custom error message, **Author is required, kindly pick one!**, if the field contains invalid data during validation, which is shown in the following screenshot:

![](bbox=[0.246, 0.191, 0.754, 0.538])

The displayed error message will be cleared once the user starts typing any value into the field.

> The client-side validation logic works based on the jQuery validate library and is performed on the fly when the user types the value by using the jQuery validate unobtrusive library.

We have explored `Required` field validation by configuring it in the `Blog` entity; let's configure the validation for the remaining entities of the blogging system in the next section.