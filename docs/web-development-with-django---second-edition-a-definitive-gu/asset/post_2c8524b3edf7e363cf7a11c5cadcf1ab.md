
Now, let's say we enter text into the input as uppercase:

Text input: ALL UPPERCASE

Figure 7.2 – ALL UPPERCASE text entered

If we were to examine the cleaned data using our debug output from the view, we would see that it is lowercase:

```
text_input: (<class 'str'>) all uppercase
```

Figure 7.3 – The cleaned data has been transformed into lowercase

These were just a couple of simple examples of how to validate fields using both validators and clean methods. You can, of course, make each type of validation much more complex if you wish and transform the data in more complex ways using a clean method.

So far, you've only learned simple methods for form validation, where you've treated each field independently. A field is valid (or not) based only on the information it contains and nothing else. What if the validity of one field depends on what the user entered into another field? An example of this might be that you have an email field to collect someone's email address if they want to be signed up for a mailing list. The field is only required if they check a checkbox that indicates they wanted to be signed up. Neither of these fields is required on their own – we do not want the checkbox to be required to be checked, but if it is checked, then the email field should be required too.

In the next section, we will show how you can validate a form whose fields depend on each other by overriding the clean method in your form.

## Multi-field validation

We've just looked at the clean_field_name methods that can be added to a Django form, to clean a specific field. Django also allows us to override the clean method, in which we can access all cleaned_data from all fields, and we know that all custom field methods have been called. This allows the fields to be validated based on another field's data.

Referring to our previous example with a form that has an email address that is only required if a checkbox is checked, we'll see how we can implement this using the clean method.

First, create a Form class and add two fields – make them both optional with the required=False argument:

```
class NewsletterSignupForm(forms.Form):
    signup = forms.BooleanField(label="Sign up to newsletter?", required=False)
    email = forms.EmailField(help_text="Enter your email address to subscribe", required=False)
```

We've also introduced two new arguments that can be used for any field:

- label
  This allows you to set the label text for a field. As we've seen, Django will automatically generate label text from the field name. If you set the label argument, you can override this default. Use this argument if you want to have a more descriptive label.
- help_text
  If you need more information to be displayed regarding what input a field requires, you can use this argument. By default, it's displayed after the field.

When rendered, the form looks like this:

![](img/4840a43efd335abd10b93708225692d6_346_0.png)

Figure 7.4 – Email signup form with a custom label and help text

If we were to submit the form now, without entering any data, nothing would happen. Neither field is required, so the form validates fine.

Now, we can add the multi-field validation to the clean method. We will check whether the Sign up to newsletter checkbox is checked, and then check that the Email field has a value. The built-in Django methods have already validated that the email address is valid at this point, so we just need to check that a value exists for it. We will then use the add_error method to set an error for the email field. This is a method you haven't seen before but it's very simple; it takes two arguments – the name of the field to set the error on, and the text of the error.

Here’s the code for the `clean` method:

```
class NewsletterSignupForm(forms.Form):
    …

    def clean(self):
        cleaned_data = super().clean()

        if cleaned_data["signup"] and not
            cleaned_data.get("email"):
                self.add_error("email", "Your email address is
                    required if signing up for the newsletter.")
```

Your `clean` method must always call the `super().clean()` method to retrieve the cleaned data. When `add_error` is called to add errors to the form, the form will no longer validate (the `is_valid` method returns `False`).

Now, if we submit the form without the checkbox checked, still, no error is generated, but if you check the checkbox without an email address, you will receive the error we just wrote the code for:

![](img/4840a43efd335abd10b93708225692d6_347_0.png)

Figure 7.5 – Error displayed when attempting to sign up with no email address

You might notice that we are retrieving the email from the `cleaned_data` dictionary using the `get` method. The reason for doing this is that if the `email` value in the form is invalid, then the `email` key will not exist in the dictionary. The browser should prevent the user from submitting the form if an invalid email has been entered, but a user might be using an older browser that does not support this client-side validation, so, for safety, we use the `get` method. Since the signup field is `BooleanField`, and not required, it will only be invalid if a custom validation function is used. We’re not using one here, so it’s safe to access its value using square bracket notation.

There’s one more validation scenario to consider before moving on to our first exercise, and that’s adding errors that are not specific to any field. Django calls these *non-field errors*. There are many scenarios where you might want to use these when multiple fields are dependent on each other.

Take, for example, a shopping website. Your order form could have two numeric fields whose totals can’t exceed a certain value. If the total were exceeded, the value of either field could be decreased to bring the total below the maximum value, so the error is not specific to either of the fields. To add a non-field error, call the add_error method with None as the first argument.

Let’s look at how to implement this. In this example, we’ll have a form where the user can specify a certain number of items to order, for item A or item B. The user cannot order more than 100 items in total. The fields will have a max_value of 100, and a min_value of 0, but custom validation in the clean method will need to be written to handle the validation of the total amount:

```
class OrderForm(forms.Form):
    item_a = forms.IntegerField(min_value=0, max_value=100)
    item_b = forms.IntegerField(min_value=0, max_value=100)

    def clean(self):
        cleaned_data = super().clean()
        if cleaned_data.get("item_a", 0) +
        cleaned_data.get("item_b", 0) > 100:
            self.add_error(None, "The total number of items must be 100 or less.")
```

The fields (item_a and item_b) are added in the normal way, with standard validation rules. You can see that we have used the clean method the same way we used it before. Moreover, we have implemented the maximum item logic inside this method. The following line is what registers the non-field error if the maximum items are exceeded:

```
self.add_error(None, "The total number of items must be 100 or less.")
```

Once again, we access the values of item_a and item_b using the get method, with a default value of 0. This is in case the user has an older browser (from 2011 or earlier) and was able to submit the form with invalid values.

In a browser, the field-level validation ensures values between 0 and 100 have been entered in each field, and prevents the form from being submitted otherwise:

![](img/4840a43efd335abd10b93708225692d6_349_0.png)

Figure 7.6 – The form cannot be submitted if one field exceeds the maximum value

However, if we put in two values that sum to more than 100, we can see how Django displays the non-field error:

- The total number of items must be 100 or less.

![](img/4840a43efd335abd10b93708225692d6_349_1.png)

Figure 7.7 – Django non-field error displayed at the start of the form

Django non-field errors are always displayed at the start of a form, before other fields or errors.

In the following exercise, we will build a form that implements a validation function, a field clean method, and a form clean method.

## Exercise 7.01 – custom clean and validation methods

In this exercise, you will build a new form that allows the user to create an order for books or magazines. It must have the following validation criteria:

- The user may order up to 80 magazines and/or 50 books, but the total number of items must not be more than 100
- The user can choose to receive an order confirmation, and if they do, they must enter an email address
- The user should not enter an email address if they have not chosen to receive an order confirmation
