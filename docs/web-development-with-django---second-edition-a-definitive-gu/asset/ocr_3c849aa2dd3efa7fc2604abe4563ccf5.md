If you’re running the dev server in Debug mode, click Debug instead.

Inside the Run console, the list of the values that were posted to the server should be displayed:

![](bbox=[0.12, 0.16, 0.85, 0.42])

Figure 6.14: Input values shown in the Run console

Here are some things you should note:

+   - All values are sent as text, even number and date inputs
+   - For the select inputs, the selected value attributes of the selected options are sent, not the text content of the option tag.
+   - If you select multiple options for books_you_own, then you will see multiple values in the request. This is why we use the getlist method since multiple values are sent for the same input name.
+   - If the checkbox was checked, you will have a checkbox_on input in the debug output. If it was not checked, then the key will not exist at all (that is, there is no key instead of having the key existing with an empty string or None value).
+   - We have a value for the submit_input name, which is the text Submit Input. You submitted the form by clicking the Submit Input button, so we receive its value. Notice that no value is set for the button_element input since that button was not clicked.

8. We will experiment with two other ways of submitting the form, first by pressing Enter on the keyboard when your cursor is in a text-like input (text, password, date, email, but not text area as pressing Enter will add a new line).