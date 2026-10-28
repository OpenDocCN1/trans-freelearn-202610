+   Choices can also be set after the form has been instantiated. You may want to generate the `list/tuple` choice inside your view dynamically and then assign it to the field's `choices` attribute, like so:

```
form = ExampleForm()
form.fields["books_you_own"].choices = [("1", "Deep
Learning with Keras"), ...]
```

Next are radio inputs, which are similar to selects:

+   Like selects, radio inputs use ChoiceField, as they provide a single choice between multiple options.

+   The options to choose between are passed into the field constructor with the choices argument.

+   The choices are provided as a tuple of two-element tuples, also like selects:

```
choices = (
    ("1", "Option One"),
    ("2", "Option Two"),
    ("3", "Option Three")
)
```

+   ChoiceField defaults to displaying as a select input, so the widget must be set to RadioSelect to have it render as radio buttons. Putting the choice setting together with this, we can add radio buttons to the form like this:

```
RADIO_CHOICES = (
    ("Value One", "Value One"),
    ("Value Two", "Value Two"),
    ("Value Three", "Value Three")
)

class ExampleForm(forms.Form):
    ...
    radio_input =
        forms.ChoiceField(choices=RADIO_CHOICES,
        widget=forms.RadioSelect)
```