Similarly, an error will be added to Email if an email address is entered but Send confirmation is not checked:

![](bbox=[0.115, 0.15, 0.85, 0.435])

Figure 7.9 – Error because an email has been entered but the user has not chosen to receive confirmation

8. Add the final check, also inside the clean method. The total number of items should not be more than 100. We will add a non-field error if the sum of magazine_count and book_count is greater than 100:

```
class OrderForm(forms.Form):
    ...
    def clean(self):
        ...
        item_total = cleaned_data.get("magazine_count", 0) + cleaned_data.get("book_count", 0)

        if item_total > 100:
            self.add_error(None, "The total number of items must be 100 or less.")
```

This will add a non-field error by passing None as the first argument to the add_error call.