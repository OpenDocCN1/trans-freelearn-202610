If you need more validation options, you can write custom validators, which we will cover in the next section. Right now, we will add some validators to our ExampleForm to see them in action.

# Exercise 6.05 – adding extra field validation

In this exercise, we will add and modify the validation rules on the fields of ExampleForm. We will then see how these changes affect how the form behaves, both in the browser and when Django validates the form:

+   1. In PyCharm, open the forms.py file inside the form_example app directory.
+   2. We will make text_input require at most three characters. Add a max_length=3 argument to the CharField constructor:
+   3. Make password_input more secure by requiring a minimum of eight characters. Add a min_length=8 argument to the CharField constructor:
+   4. The user may have no books, so the books_you_own field should not be required. Add a required=False argument to the MultipleChoiceField constructor:
+   5. The user should only be able to enter a value between 1 and 10 in integer_input. Add min_value=1 and max_value=10 arguments to the IntegerField constructor:
+   6. Finally, add max_digits=5 and decimal_places=3 to the DecimalField constructor:
+   Save the file.

```
text_input = forms.CharField(max_length=3)
```

```
password_input = forms.CharField(min_length=8,
widget=forms.PasswordInput)
```

```
books_you_own =
forms.MultipleChoiceField(required=False,
choices=BOOK_CHOICES)
```

```
integer_input = forms.IntegerField(min_value=1,
max_value=10)
```

```
decimal_input = forms.DecimalField(max_digits=5,
decimal_places=3)
```