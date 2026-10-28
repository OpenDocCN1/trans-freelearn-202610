+   - You can manually define the fields on PublisherForm, using the same rules defined on the Publisher model, as shown here:

```
class PublisherForm(forms.Form):
    name = forms.CharField(max_length=50)
    website = forms.URLField()
    ...
```

+   - In the view, the initial values would be retrieved from the model queried from the database, then passed to the form using the initial argument. If you were creating a new instance, the initial value would be blank – something like this:

```
if create:
    initial = {}
else:
    publisher = Publisher.objects.get(pk=pk)
    initial = {"name": publisher.name, "website":
    publisher.website, ...}

form = PublisherForm(initial=initial)
```

+   - Then, in the POST flow of the view, you can either create or update the model based on cleaned_data:

```
form = PublisherForm(request.POST, initial=initial)
if create:
    publisher = Publisher()
else:
    publisher = Publisher.objects.get(pk=pk)

publisher.name = form.cleaned_data['name']
publisher.website = forms.cleaned_data['website']
...
publisher.save()
```