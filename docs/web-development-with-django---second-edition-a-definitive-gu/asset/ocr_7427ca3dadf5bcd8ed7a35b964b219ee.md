So far, we have learned about multi-field validations, and how to implement them to make sure that we can validate the form in one go rather than highlighting errors one by one.

In the next section, we will cover how to add placeholders and initial values to the form.

# Adding placeholders and initial values

There are two things our first manually built form had that our current Django form still does not have – placeholders and initial values. Adding placeholders is simple; they are just added as attributes to the widget constructor for the form field. This is similar to what we've already seen for setting the type of DateField in our previous examples.

Here's an example:

```
class ExampleForm(forms.Form):
    text_field = forms.CharField(
        widget=forms.TextInput(attrs={"placeholder": "Text Placeholder"})
    )
    password_field = forms.CharField(
        widget=forms.PasswordInput(attrs={"placeholder": "Password Placeholder"})
    )
    email_field = forms.EmailField(
        widget=forms.EmailInput(attrs={"placeholder": "Email Placeholder"})
    )
    text_area = forms.CharField(
        widget=forms.Textarea(attrs={"placeholder": "Text Area Placeholder"})
    )
```