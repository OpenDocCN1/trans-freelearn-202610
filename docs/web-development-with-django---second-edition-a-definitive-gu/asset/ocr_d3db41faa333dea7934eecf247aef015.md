In a browser, the field-level validation ensures values between 0 and 100 have been entered in each field, and prevents the form from being submitted otherwise:

![](bbox=[0.3, 0.14, 0.68, 0.24])

Figure 7.6 – The form cannot be submitted if one field exceeds the maximum value

However, if we put in two values that sum to more than 100, we can see how Django displays the non-field error:

+   - The total number of items must be 100 or less.

![](bbox=[0.28, 0.37, 0.57, 0.47])

Figure 7.7 – Django non-field error displayed at the start of the form

Django non-field errors are always displayed at the start of a form, before other fields or errors.

In the following exercise, we will build a form that implements a validation function, a field clean method, and a form clean method.

# Exercise 7.01 – custom clean and validation methods

In this exercise, you will build a new form that allows the user to create an order for books or magazines. It must have the following validation criteria:

+   - The user may order up to 80 magazines and/or 50 books, but the total number of items must not be more than 100

+   - The user can choose to receive an order confirmation, and if they do, they must enter an email address

+   - The user should not enter an email address if they have not chosen to receive an order confirmation