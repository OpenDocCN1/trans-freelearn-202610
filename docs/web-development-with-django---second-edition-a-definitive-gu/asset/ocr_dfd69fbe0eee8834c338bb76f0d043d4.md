+   - To ensure they are part of our company, the email address must be part of our company domain (in our case, we’ll just use `example.com`)
+   - For consistency with other email addresses in our fictional company, the address should be converted into lowercase

This sounds like a lot of rules, but with Django, it’s simple if we tackle them one by one. We’ll carry on with the `form_project` app you started in *Chapter 6, Forms*. If you haven’t completed *Chapter 6, Forms*, you can download the code from `https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter06/final/form_project`.

+   1. In PyCharm, open the `form_example` app’s `forms.py` file.

> **Note**
> 
> Make sure the Django dev server is not running; otherwise, it may crash as you make changes to this file, causing PyCharm to jump into the debugger.

Since our work with `ExampleForm` is done, you can remove it from this file.

+   2. Create a new class called `OrderForm` that inherits from `forms.Form`:

```
```python
class OrderForm(forms.Form):
```
```

+   3. Add four fields to the class, as follows:
+   
+   - `magazine_count`, IntegerField with a `min_value` of 0 and a `max_value` of 80
+   - `book_count`, IntegerField with a `min_value` of 0 and a `max_value` of 50
+   - `send_confirmation`, BooleanField, which is not required
+   - `email`, EmailField, which is also not required

The class should look like this:

```
```python
class OrderForm(forms.Form):
    magazine_count = forms.IntegerField(min_value=0,
                                        max_value=80)
    book_count = forms.IntegerField(min_value=0,
                                    max_value=50)
    send_confirmation = 
        forms.BooleanField(required=False)
    email = forms.EmailField(required=False)
```
```