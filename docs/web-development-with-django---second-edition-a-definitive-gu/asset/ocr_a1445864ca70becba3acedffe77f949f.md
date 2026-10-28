The validator function can be used on any number of fields. In our example, if we wanted lots of fields to have lowercase enforced, validate_lowercase could be passed to all of them. Now, let’s look at how we could implement this another way, with a custom clean method.

# Cleaning methods

A clean method is created on the Form class and is named in the clean_field_name format. For example, the clean method for text_input would be called clean_text_input, the clean method for books_you_own would be called clean_books_you_own, and so on.

Cleaning methods take no arguments; instead, they should use the cleaned_data attribute on self to access the field data. This dictionary will contain the data after being cleaned in the standard Django way, as we saw in the previous example. The clean method must return the cleaned value, which will replace the original value in the cleaned_data dictionary. Even if the method does not change the value, a value must be returned. You can also use the clean method to raise ValidationError, and the error will be attached to the field (the same as with a validator).

Let’s re-implement the lowercase validator as a clean method, like this:

```
class ExampleForm(forms.Form):
    text_input = forms.CharField()
    ...

    def clean_text_input(self):
        value = self.cleaned_data["text_input"]
        if value.lower() != value:
            raise ValidationError("{} is not lowercase."
                .format(value))

        return value
```

You can see that the logic is essentially the same, except we must return the validated value at the end. If we submit the form, we will get the same result as the previous time we tried (Figure 7.1).

Let’s look at one more cleaning example. Instead of raising an exception when the value is invalid, we could just convert the value into lowercase. We would implement that with this code:

```
class ExampleForm(forms.Form):
    text_input = forms.CharField()
    ...

    def clean_text_input(self):
        value = self.cleaned_data['text_input']
        return value.lower()
```