You will need to specify the folder in which to store security keys. This folder should be accessible to as few users as possible. For testing purposes, you may want to select the Operate in credit card debug mode option, but you should remember to disable it before you begin accepting real payments.

5. The Checkout workflow section allows you to control whether or not credit card numbers should be validated before they are submitted to the processor, and whether or not cards should be processed when the order is submitted. Both settings should be left on unless you are testing your site.

Checkout workflow
These settings alter the way credit card data is collected and used during checkout.
☑ Validate credit card numbers at checkout.
Invalid card numbers will show an error message to the user so they can correct it.
This feature is recommended unless you are in debug mode.
☑ Attempt to process credit card payments at checkout.
Failed attempts will prevent checkout completion and display the error message from above.
This box must be checked to process customer credit cards if you are not in debug mode.

6. Next, you can specify the fields that must be entered when the visitor checks out.

Credit card fields
Specify what information to collect from customers in addition to the card number.
☑ Enable CVV text field on checkout form.
The CVV is an added security measure on credit cards. On Visa, Mastercard, and Discover cards it is a three digit number, and on AmEx cards it is a four digit number. If your credit card processor or payment gateway requires this information, you should enable this feature here.
☐ Enable card owner text field on checkout form.
☐ Enable card start date on checkout form.
☐ Enable card issue number text field on checkout form.
☐ Enable issuing bank text field on checkout form.
☐ Enable card type selection on checkout form.
If enabled, specify in the textarea below which card options to populate the select box with.
Card type select box options:
Visa
Mastercard
Discover
American Express
Enter one card type per line. These fields will populate the card type select box if it is enabled.