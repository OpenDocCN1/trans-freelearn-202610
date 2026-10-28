Values in `request.POST` will override values in `initial`. This means that even if we have an initial value for a required field, if it's left blank when submitted, then it will not validate. The field will not fall back to the value in `initial`.

Whether you decide to set initial values in the Form class itself or the view is up to you and depends on your use case. If you had a form that was used in multiple views but usually had the same value, it would be better to set the `initial` value in the form. Otherwise, it can be more flexible to use `setting` in the view.

In the next exercise, we will add placeholders and initial values to the `OrderForm` class from the previous exercise.

# Exercise 7.02 – placeholders and initial values

In this exercise, you will enhance the `OrderForm` class by adding placeholder text. You will simulate passing an initial email address to the form. It will be a hardcoded address but once the user can log in, it could be an email address associated with their account – you will learn about sessions and authentication in *Chapter 9, Sessions and Authentication*:

+   1. In PyCharm, open the `reviews` app's `forms.py` file. You will add placeholders to the `magazine_count`, `book_count`, and `email` fields on the `OrderForm`, which means also setting widget.
+   
+   To the `magazine_count` field, add a `NumberInput` widget with `placeholder` in the `attrs` dictionary. This `placeholder` should be set to **Number of Magazines**. Write the following code:

```
magazine_count = forms.IntegerField(
    min_value=0,
    max_value=80,
    widget=forms.NumberInput(attrs={"placeholder":
    "Number of Magazines"}),
)
```

+   2. Add a placeholder to the `book_count` field in the same manner. The placeholder text should be **Number of Books**:

```
book_count = forms.IntegerField(
    min_value=0,
    max_value=50,
    widget=forms.NumberInput(attrs={"placeholder":
    "Number of Books"}),
)
```